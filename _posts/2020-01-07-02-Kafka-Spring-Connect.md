---
layout: post
title:  "Kafka 와 SpringBoot 연동"
date:  2020-01-07
desc: "Kafka 와 SpringBoot 연동"
keywords: "Kafka"
categories: [Spring]
tags: [Kafka,Server,Spring]
icon: icon-apache
---
안녕하세요 Dev JaeIn 입니다.

이번 포스팅은 Kafka 와 SpringBoot 연동을 통해 Produecr에서 Consumer로 데이터가 전달되는 것을

확인하고자 합니다. 그리고 SpringBoot 콘솔창에 Producer에서 넘어오는 데이터를 Listener를 통해 출력까지 진행하겠습니다.

기본적으로 Kafka는 설치되어 있다고 가정하고 시작합니다. 만약 Kafka가 설치되어 있지 않으신 분들은

지난번 포스팅 

## [윈도우 환경에서의 Kafka 설치 및 테스트 입니다.](https://jjjwodls.github.io/etc/2020/01/07/01-Kafka-Setup.html)

를 참조해주시면 됩니다.

***


* ### 우선 Zookeeper 와 Kafka 를 구동시켜 놓습니다. 그리고 필요에 따라 Consumer, Producer도 같이 켜두시면 되겠습니다.
  
### 저는 Consumer, Producer 도 같이 켜놓은 후 테스트 할 때 사용했습니다.
  

1.환경은 SpringBoot를 이용했습니다. 기존 프로젝트에 Dependency 를 추가하시면 됩니다. 

```
<dependency>
   <groupId>org.apache.kafka</groupId>
   <artifactId>kafka-streams</artifactId>
</dependency>
<dependency>
   <groupId>org.springframework.kafka</groupId>
   <artifactId>spring-kafka</artifactId>
</dependency>
<dependency>
   <groupId>org.springframework.kafka</groupId>
   <artifactId>spring-kafka-test</artifactId>
   <scope>test</scope>
</dependency>
```

2.KafkaConfiguration 파일 생성 후 설정

전 properties 파일을 새로 만들었는데 application.properties에도 속성을 지원하니 거기에 선언하셔도 됩니다.

제가 선언한 속성입니다.

```
bootstrap.servers=localhost:9092
retries=0
batch.size=4096
linger.ms=1
buffer.memory=40960
```

그리고 스프링 Bean에 등록해야 하므로 다음과 같이 작성했습니다.

Producer 생성입니다.

```
@Configuration
@PropertySource("classpath:kafka.properties")
public class KafkaConfiguration {
	
	@Autowired
	private Environment env;
	
	public Map<String,Object> producerConfig(){
		
		Map<String,Object> props = new HashMap<>();
		
		//server host 지정
		props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG,
				env.getProperty(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG));
		
		// retries 횟수
		props.put(ProducerConfig.RETRIES_CONFIG,
				env.getProperty(ProducerConfig.RETRIES_CONFIG));
		
		//batcch size 지정
		props.put(ProducerConfig.BATCH_SIZE_CONFIG,
				env.getProperty(ProducerConfig.BATCH_SIZE_CONFIG));
		
		// linger.ms 
		props.put(ProducerConfig.LINGER_MS_CONFIG,
				env.getProperty(ProducerConfig.LINGER_MS_CONFIG));
		
		//buufer memory size 지정
		props.put(ProducerConfig.BUFFER_MEMORY_CONFIG,
				env.getProperty(ProducerConfig.BUFFER_MEMORY_CONFIG));
		
		//key serialize 지정
		props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG,StringSerializer.class);
		
		//value serialize 지정
		props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG,StringSerializer.class);
		
		
		return props;
		
	}
	
	public ProducerFactory<String,String> producerFactory(){
		return new DefaultKafkaProducerFactory<>(producerConfig());
	}
	
	@Bean
	public KafkaTemplate<String, String> kafkaTemplate(){
		return new KafkaTemplate<>(producerFactory());
	}
}
```

3.KafkaConsumerConfig 설정

kafka의 Consumer 설정입니다.

```
@EnableKafka
@Configuration
@PropertySource("classpath:kafka.properties")
public class KafkaConsumerConfig {

	@Autowired
	private Environment env;
	
	@Bean
    public ConsumerFactory<String, String> consumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(
                ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG,
                env.getProperty("bootstrap.servers"));
        props.put(
                ConsumerConfig.GROUP_ID_CONFIG,
                "foo");
        props.put(
                ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG,
                StringDeserializer.class);
        props.put(
                ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG,
                StringDeserializer.class);
        return new DefaultKafkaConsumerFactory<>(props);
    }
 
    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, String> kafkaListenerContainerFactory() {
 
        ConcurrentKafkaListenerContainerFactory<String, String> factory
                = new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        return factory;
    }
	
}
```

4.컨트롤러 생성 후 테스트

컨트롤러에 Listner를 등록하여 테스트를 진행했지만 따로 Class 생성 후 진행해도 됩니다.

대신에 Bean으로 등록되어야 하므로 클래스에 @Component 만 선언해주시면 됩니다.

```
@Controller
public class HomeController {
	
	
	private static final DateTimeFormatter fmt = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
	
	private static final String topicName = "jjjwodls";
	
	@Autowired
    private KafkaTemplate<String,String> kafkaTemplate;
	
	@RequestMapping("/kafka.do")
	public void kafka(String msg) {
		LocalDateTime date = LocalDateTime.now();
		String dateStr = date.format(fmt);
		kafkaTemplate.send(topicName, "8080 send " + dateStr + " : " + msg);
	}
	
	@KafkaListener(topics = topicName,groupId = "foo")
	public void listen(String message) {
		System.out.println("Received Msg jjjwodls " + message);
	}
	
	/**
	 * groupId 를 다르게 하여 내용을 보여주기 위해 일부로 다르게 설정하였음.
	 * @param headers header에 담긴 내용을 보여준다.
	 * @param payload 넘어온 문자열에 대해 보여줌.
	 */
	@KafkaListener(topics = topicName,groupId = "bar")
	public void listen2(@Headers MessageHeaders headers,@Payload String payload ) {
		System.out.println("=======================================");
		System.out.println("Consume Headers : " + headers.toString());
		System.out.println("=======================================");
		System.out.println("PayLoad : " + payload);
		System.out.println("=======================================");
	}
	
}
```

***

* 실제 요청 시 나타나는 msg (console 을 통한 메시지 전송)
  
  ![](/assets/img/blog/2020-01-07-02-Kafka-Spring-Connect/2020-01-07-18-10-27.png){: .img-center} 

* Request를 통한 Send
  
  ![](/assets/img/blog/2020-01-07-02-Kafka-Spring-Connect/2020-01-07-18-12-27.png){: .img-center}


***  

### 정리

*** 

테스트 수행 결과 성공적으로 잘 마무리 됐습니다. 

카프카에 대한 개념이 아직 부족하지만, 상세한 내용은 더 공부하고 포스팅 진행하도록 하겠습니다.

부족한 부분이나 궁금한 부분 있으면 댓글 부탁드립니다!!



