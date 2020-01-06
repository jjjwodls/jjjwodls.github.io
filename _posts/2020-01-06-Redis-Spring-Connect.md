---
layout: post
title:  "Spring Boot와 Redis 연동"
date:   2020-01-06
desc: "Spring과 Redis 연동"
keywords: "Redis,Spring"
categories: [Spring]
tags: [Redis,Spring,Java,DB,Server]
icon: server
---
안녕하세요 Dev JaeIn 입니다.

이번 포스팅에서 다룰 주제는 Redis 와 SpringBoot 연동입니다.

Spring Boot 설치와 Redis 설치는 이전 포스팅을 참조하시면 되겠습니다.

- [스프링부트 설치 방법](https://jjjwodls.github.io/spring/2020/01/05/Springboot-Set.html)
  
- [Redis 설치 방법_Window 기준](https://jjjwodls.github.io/etc/2020/01/02/Redis-Setup.html)

이번 포스팅은 간단한 예제로 진행합니다. 

Redis 는 기존 포스팅에서도 작성했지만 저장소 입니다. 하지만, Orcacle이나 Mysql과 다른 NoSQL 형태의 DB라고 생각하시면 됩니다. 
그래서 관계형 데이터 베이스 구조가 아닌 **key,value** 쌍으로 저장됩니다.

Redis 와 SpringBoot 연동 설명 시작하겠습니다.

***

1.먼저 Redis 서버를 구동합니다.(redis-server.exe)

2.SpringBoot Tool을 수행 후 프로젝트를 새로 생성합니다. 
기존에 테스트 했던 프로젝트를 사용하시거나 또는 프로젝트에 적용시키고 싶으신 분들은 pom.xml에 Dependency를 추가하시면 됩니다.

```
<dependency>
   <groupId>org.springframework.boot</groupId>
   <artifactId>spring-boot-starter-data-redis-reactive
   </artifactId>
</dependency>
```

3.새로 프로젝트를 만드시는 분들은 NoSQL 영역에 Spring Data Reactive Redis 버튼을 클릭한 후 프로젝트를 생성합니다.
![](/assets/img/blog/2020-01-06-Redis-Spring-Connect/2020-01-06-11-49-22.png){: .img-center} 


4.서버가 수행 될 때 데이터를 저장하기 위해 Redis 관련 Component 를 생성합니다. 
![](/assets/img/blog/2020-01-06-Redis-Spring-Connect/2020-01-06-11-52-48.png){: .img-center} 

5.redis-cli.exe 를 수행하여 데이터를 확인합니다.

6.get "keyname" 을 통해 데이터가 입력된 것을 확인 할 수 있습니다.
![](/assets/img/blog/2020-01-06-Redis-Spring-Connect/2020-01-06-11-54-04.png){: .img-center} 

간단한 테스트를 위해 서버가 구동 될 때 데이터를 Set 하도록 진행하였습니다. 위 내용들은 테스트 예제이므로 실무에서 꼭 이렇게 적용해야 하는것은 아니라 생각합니다. 

이상 Redis 윈도우에 설치 및 테스트 진행 결과였습니다.

### 추가
*** 
이번 포스팅에서는 String 으로 key,value 쌍으로만 테스트 진행했지만,

List, Set 등등 다른 형태로 데이터로도 저장이 가능합니다. 

그렇게 되면 하나의 key에 여러개의 데이터를 담아 하나의 key 로 많은 데이터도 가져 올 수 있습니다.

추 후 데이터를 다루는 테스트는 진행하겠습니다.

지금까지 읽어주셔서 감사합니다. 
