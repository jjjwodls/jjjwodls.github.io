---
layout: post
title:  "Spring 으로 Redis 성능 테스트"
date:   2020-01-06
desc: "Redis 성능테스트"
keywords: "Redis,Spring"
categories: [Spring]
tags: [Redis,Spring,Java,DB,Server]
icon: server
---
안녕하세요 Dev JaeIn 입니다.

이번 포스팅에서 다룰 주제는 Redis와 SpringBoot 를 통한 Redis 의 성능 테스트 입니다.

스프링 부트와 Redis 연동은 다음 포스팅을 참조하시면 됩니다.

- [스프링부트와 Redis 연동](https://jjjwodls.github.io/spring/2020/01/06/Redis-Spring-Connect.html)

우선 DB는 더미 테이블을 생성하여 준비하였습니다.

* Dummy 데이터 및 테이블 구조
  
  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-08-35.png)

  테이블 구조의 Primary key 가 1부터 순차적으로 증가하여 Random 한 숫자를 생성하면 접근 가능하도록 구성되어 있습니다.

* 테스트 툴은 Jmeter 를 활용했습니다.

## Redis는 캐시서버로 많이 활용되어 비지니스 로직에 도달하기 전에 처리되어 응답하는게 BEST라 생각합니다.

## 하지만, 현재 구조는 테스트 진행이기 때문에 랜덤 데이터를 주기적으로 Request 할 수 없을거라 판단했습니다. 그래서 서비스로직에서 

## 랜덤한 id를 생성하여 데이터를 가져오도록 구성했습니다. 

* 진행할 내용

## DB에서 데이터를 가져올 때 VS Redis에서 데이터를 가져 올 때를 비교해보는 방식으로 진행했습니다.

***

* 제가 구성한 로직은 다음과 같이 구성했습니다.
  
  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-13-40.png)
  
서비스 로직에 DB에서 가져오는 로직과 Redis에서 가져오는 로직을 만들었습니다.

## 구현한 소스 보여드린 후 진행하도록 하겠습니다.

* User class
  
  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-14-41-44.png)

* Controller
  
  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-14-43-29.png)

* Service
 
  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-14-43-47.png)

* Dao
  
  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-14-38-58.png)

* Redis Data Set
  
  Redis에는 1만개 데이터를 미리 넣습니다. (Redis 서버가 꼭 실행중이여야 합니다.)

  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-14-40-49.png)

* 실제 Request 결과 페이지
  
  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-14-45-45.png)

  ![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-14-46-00.png)

***

jmeter 를 사용하여 Request 를 날려 분석해보겠습니다.

요청은 유저수 4천명 기준으로 잡고 진행했습니다. 동일한 유저수로 Redis,DB 요청을 진행한 결과를 보여드리겠습니다.

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-31-56.png)

***

### 1.DB 에서 데이터 가져온 결과 

* Summary Report

중점적으로 살펴본 것은 처리량 및 응답받은 데이터 입니다.

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-41-46.png)

* Transactions Per Second

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-45-40.png)

대략 50 정도에서 움직이는 것을 확인 할 수 있습니다. DB 에서 데이터를 가져오는 시간이 있기 때문에 요정도 수치가 나왔다고 생각합니다.

* Response Times Over Time

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-48-16.png)

처리 가능한 시간 대비 사용자수가 급격히 증가하여 응답시간은 점점 늘어나는 것을 확인 할 수 있습니다.

* Active Threads Over Time

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-49-14.png)

쓰레드 수도 순차적으로 줄어드는것을 확인 할 수 있습니다.

***

### 2.Redis 에서 데이터 가져온 결과 

* Summary Report

이번에도 중점적으로 살펴본 것은 처리량 및 응답받은 데이터 입니다.

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-50-53.png)

* Transactions Per Second

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-51-44.png)

최대 800 이상까지 올라가는 것을 확인 할 수 있습니다. 아무래도 DB에 접근하지 않고 바로 가져오니 빠르게 처리하는 것을

확인 할 수 있습니다.

* Response Times Over Time

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-51-54.png)

응답시간도 전체적으로 빠른편 입니다. TPS가 높다 보니 응답 데이터가 최대한 들어오는 대로 처리가 가능하다 보여집니다.

* Active Threads Over Time

![](/assets/img/blog/2020-01-06-Redis-Performance/2020-01-06-13-52-12.png)

쓰레드 숫자도 누적되기 전에 처리되는 것을 확인 가능합니다.

***

## 결론 DB vs Redis

***

전체적인 수치를 비교해 봤을 때 

1. Throughput (MAX 기준)

      DB : 43.5/sec
      Redis : 568.8/sec

      Redis가 압도적으로 빠른 처리량을 보입니다.

2. Transactions Per Second(TPS)
   
      DB : 50 
      Redis : 800 

      TPS도 마찬가지로 Redis 가 빠른 처리량을 보입니다.

3. Response Times Over Time

      DB : 가장 마지막에 들어온 사람은 최대 87초 동안 대기.
      Redis : 가장 마지막에 들어온 사람은 2.3초 동안 대기

      응답 시간도 매우 차이나는 것을 확인 가능합니다.

      Redis가 매우 우세.

4. Active Threads Over Time

      DB : Thread 갯수도 DB접근의 경우 요청이 계속 쌓여서 순차적으로 처리하여 점차 줄어드는 것으로 확인
      
      Redis : Thread 갯수가 많이 쌓이지 않고 금방 처리가 되는것을 확인

### 전체적으로 Redis가 우수한 성능을 확인 할 수 있습니다.

***

### 후기

이번 테스트를 통해 Redis 서버의 빠른 퍼포먼스를 알 수 있었습니다.

많은 대규모 서비스를 하는 기업들이 Redis를 왜 사용하는지 대규모 서비스에서 왜 빠른 응답이 가능 한지 알 수 있는 시간이였습니다.

읽어주셔서 감사합니다. 


