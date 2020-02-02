---
layout: post
title:  "String,StringBuilder, StringBuffer 비교"
date:   2020-01-13
desc: "String,StringBuilder, StringBuffer에 대한 설명입니다. "
keywords: "Java,String,StringBuilder,StringBuffer"
categories: [Java]
tags: [Java,String,StringBuilder,StringBuffer]
icon: icon-java
---

안녕하세요 Dev JaeIn 입니다.

이번 포스팅은 String 과 StringBuilder, StringBuffer에 대한 내용입니다.

기본적으로 String 을 사용하면 연산이 느리다는 것은 많은 사람들이 알고 있는 내용입니다.

그런데 왜 느린지 이유 그리고 StringBuilder 또는 StringBuffer 를 왜 사용하는지에 대해 알아보고자 합니다.

***

## 우선 String이 저장되는 과정입니다.

```java
String a = "a";
String b = "b";
String c = a+b;
```

이런식으로 String을 생성하게 되면 각각 heap 메모리에 a와 b 라는 영역의 String 객체를 생성하게 됩니다. 

여기에 c 라는 값이 생성되면 a와 b를 참조하는 것이 아닌 새로운 "ab"에 대한 공간이 생겨 저장이 됩니다.

이런식으로 각각 스트링을 더하는 연산을 진행하면 메모리 공간 낭비 뿐만이 아닌 새로운 공간을 생성하며 연산하는데 시간이 오래걸리게 됩니다. 

***

## 그러면 StringBuilder 와 StringBuffer 는 언제 사용되는지 알아보겠습니다.

*** 

문자열 연산이 적은 경우에는 String을 사용해도 퍼포먼스가 그렇게 떨어지진 않겠지만 연산이 많은 경우에는 각각 String 객체를 메모리에 생성하기 때문에 속도가 많이 느려집니다.

이런 경우에 StringBuilder 또는 StringBuffer 의 append 메소드를 사용하여 문자열을 연산 후 반환 시 toString 메소드를 사용합니다.

이렇게 되면 toString메소드를 사용 할 때 문자열을 Heap 메모리에 생성하므로 String 연산보다 속도가 빠릅니다.

*** 

* StringBuilder 와 StringBuffer의 차이점 ? 

StringBuffer는 Thread-safe 입니다.

![](/assets/img/blog/2020-01-13-String%20Builder%20Buffer/2020-01-13-22-36-25.png){: .img-center}

StringBuffer는 Thread-safe 하지 않는다.

![](/assets/img/blog/2020-01-13-String%20Builder%20Buffer/2020-01-13-22-38-08.png){: .img-center}

이런 차이점이 존재합니다. 

따라서 동기화 하는 시간이 존재하는 StringBuffer 가 속도면에서 더 떨어집니다.

단일스레드 환경이라면 StringBuilder를 사용하면 더 좋은 퍼포먼스가 납니다.


***

결론

***

## 속도

StringBuilder >> StringBuffer >> String

## 멀티쓰레드 환경

StringBuffer 를 사용하자 (동기화 할 필요가 있는 경우)

아닌경우

StringBuilder 를 사용하자. 

이상 String에 대한 설명이였습니다.

읽어주셔서 감사합니다.

부족한 부분 있으면 댓글 부탁드립니다!

***

