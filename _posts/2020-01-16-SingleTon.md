---
layout: post
title:  "Java의 싱글톤(SingleTon) 패턴"
date:   2020-01-16
desc: "싱글톤 패턴의 소개 및 쓰임"
keywords: "Java,SingleTon,싱글톤,자바 싱글톤,Java SingleTon"
categories: [Java]
tags: [Java,SingleTon]
icon: icon-java
---

안녕하세요 Dev JaeIn 입니다.

오늘은 많은 분들이 사용하고 있고 알고 있는 싱글톤 패턴에 대해 포스팅 합니다.

***
## 싱글톤 패턴이란?

클래스가 인스턴스를 하나만 갖게 하고 전역 범위에서 이 인스턴스에 접근하는 단일 지점을 제공하기 위해 사용함.

***

즉, 애플리케이션 전역에서 해당 객체의 인스턴스는 하나만 존재한다고 생각하시면 됩니다.

대부분 싱글톤을 사용하면 퍼포먼스가 좋아진다 라고 생각을 합니다.

하지만, 싱글톤을 남용하면 필요하지 않은 리소스를 캐시하고 가비지 컬렉터가 객체를 회수하지 못하는 현상이 발생하여 오히려 메모리 리소스가 줄어들 수 있는 현상이 발생합니다.

그러면 일반적으로 알고있는 싱글톤 코드에 대해 작성해보겠습니다.

```java
public class SingleTon{

    private static SingleTon instance;

    private SingleTon(

    )

    public static SingleTon getInstance(){
        if(instance == null){
            instance = new SingleTon();
        }
        return instance;
    }
}

```

이런식의 코드를 작성합니다. 하지만, 다중 스레드 환경에서라면 여러개의 인스턴스가 생성 될 수도 있습니다. 그래서 다음과 같이 메소드에 synchronized 를 붙여 잠금 장치를 생성해줍니다. 

```java
public static synchronized SingleTon getInstance(){
        if(instance == null){
            instance = new SingleTon();
        }
        return instance;
    }
```

이중 잠금 장치를 이용해서 만든 싱글톤 입니다.

```java
public class SingleTon{

    //volatile 은 Java 변수를 Main Memory에 저장하겠다는 의미.
    private volatile static SingleTon instance;

    private SingleTon(

    )

    public static SingleTon getInstance(){
        if(instance == null){
            synchronized(SingleTon.class){
                if(instance == null){
                    instance = new SingleTon();
                }
            }
            
        }
        return instance;
    }
}
```

많이 사용하는 싱글톤 방식입니다.

하지만, 자바의 리플렉션을 통해 API로 생성자 접근 접근 수정자를 

퍼블릭으로 바꾸면 싱글톤을 다시 만들어 낼 수 있습니다.

그래서 싱글톤은 Java5에서 도입된 enum 타입을 써서 생성하는게 최선입니다.

### enum은 태생부터 싱글톤이라 객체 생성 및 동기화, 그리고 초기화 관련 문제를 고민하지 않아도 됩니다.

```java
public enum SingleTonEnum{
    INSTANCE;
    public void doAnyThing(){

    }
}
```

싱글톤 객체 인스턴스는 다음과 같이 참고합니다.

```java
SingleTonEnum enumSingleTon = SingleTonEnum.INSTANCE;
```

이게 싱글톤 객체 참조의 끝입니다.

***
정리
***

평소 싱글톤 패턴에 대해 가장 처음에 소개한 방법으로만 하면 문제 없겠다고 생각했습니다.

하지만 이번 공부를 통해 싱글톤 객체를 사용하는게 무조껀 좋은것만은 아니라는 것을 알 수 있었고 정확한 사용법에 대해 알 수 있었습니다.

참고 도서

<http://www.yes24.com/Product/Goods/36954014?Acode=101>

읽어주셔서 감사합니다! 

