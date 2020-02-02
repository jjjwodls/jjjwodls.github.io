---
layout: post
title:  "Object 배열의 Stack VS 제네릭(Generic) Stack"
date:   2020-02-02
desc: "블로그 개설 후 소개하기 위해 작성합니다."
keywords: "Java,Generic,제네릭,제네릭 과 Object 비교"
categories: [Java]
tags: [Java,자바,Generic,제네릭,컬렉션,Collection]
icon: icon-java
---

안녕하세요 Dev JaeIn 입니다.

지난 포스팅에 이어 제네릭으로 스택을 만들어 

일반 스택과 제네릭 스택을 비교해보는 포스팅을 진행하겠습니다.

그리고 비교를 통해 제네릭 스택이 어떠한 이점이 있는지 확인해보겠습니다. 

***

## Object 배열을 사용한 스택


```java

public class NomalStack {
	
	private Object[] elements;
	private int size = 0;
	private static final int DEFAULT_SIZE = 16;
	
	public NomalStack() {
		elements = new Object[DEFAULT_SIZE];
	}
	
	public void push(Object e) {
		ensureCapacity();
		elements[size++] = e;
	}
	
	public Object pop() {
		if(size == 0)
			throw new EmptyStackException();
		
		Object result = elements[--size];
		elements[size] = null; //할당 해제

		return result;
	}
	
	public boolean isEmpty() {
		return size == 0;
	}
	
    public int size() {
		return size;
	}
	
	/**
	 * 사이즈체크 후 배열 사이즈 증가
	 */
	private void ensureCapacity() {
		if(elements.length == size)
			elements = Arrays.copyOf(elements, 2*size+1);
	}
	
}

```

위와 같이 소스가 구성되어 있습니다. 별 문제 없이 잘 동작합니다. 

그런데 문제점이 존재합니다. Object로 이루어져 있어서 어떤 타입이 들어와도 동작이 잘 되는건 확실합니다.

하지만, 여러 타입이 중복되어 들어갔을 경우 어떤 현상이 발생하는지 보도록 하죠.

```java

public class Main {

	public static void main(String[] args) {
		
		//해당 스택은 String 타입의 스택입니다.
		NomalStack st = new NomalStack();
		st.push("abcd");
		st.push(1234);
		
		String[] ary = new String[st.size()];
		
        //ClassCastException 발생.
		for(int i = 0 ; i < ary.length ; i++) {
			ary[i] = (String) st.pop();
		}
	}

}

```

* String만 담기로 설정했지만 실수로 int형을 담는다면 ClassCastException이 발생하죠. 이건 컴파일 단계도 아닌 런타임 단계에서 발생합니다. 실제 이클립스에서 수행한 것을 보여드리겠습니다.

![](/assets/img/blog/2020-02-02-02-Java-Generic-Exam/2020-02-02-23-56-44.png){: .img-center}

위 사진과 같이 컴파일은 진행되고 런타임시에 발생하는것을 확인 할 수 있습니다.

***

## 제네릭을 사용한 스택

* 제네릭을 사용해서 크게 달라지는 부분은 없습니다. 다만, 클래스 선언부에 타입 매개변수만 추가해주고 리턴 타입이 Object인 부분을 타입 매개변수로만 변경해주면 됩니다.

* 제네릭 타입으로 변경 후 소스입니다. 하지만 다음 사진과 같이 컴파일 에러가 발생합니다. 그 이유는 이전 포스팅에서 설명한것과 같이 제네릭은 type-safe 하지 않기 때문입니다. 그래서 다음 방법을 통해 우회하여 생성해보도록 하겠습니다.

![](/assets/img/blog/2020-02-02-02-Java-Generic-Exam/2020-02-03-00-00-21.png){: .img-center}


* Object 배열로 선언 후 제네릭으로 캐스팅을 합니다. 하지만, type-safe 한지 체크할 수 없다는 경고가 나타납니다. 하지만, 실제 우리들은 타입이 안전한지 확인 할 수 있습니다. 

![](/assets/img/blog/2020-02-02-02-Java-Generic-Exam/2020-02-03-00-03-52.png){: .img-center}

* 배열의 멤버필드를 private로 선언하여 클라이언트로 반환되거나 다른 메서드에서 접근 불가능하도록 만들면 가능합니다.

* 또한 push 메소드에서 원소 타입은 항상 E 인스턴스만 담기 때문에 문제가 없습니다.

* @SuppressWarnings("unchecked")를 추가해주고 윗부분에 상세하게 왜 해당 부분이 타입이 안전한지 주석으로 달아주면 됩니다. 

* 다음은 완성된 코드입니다.

```java
public class GenericStack<E> {
	
	private E[] elements;
	private int size = 0;
	private static final int DEFAULT_SIZE = 16;
	
	//push 메소드는 E 인스턴스만 담는다. 따라서 타입 안전성을 보장한다.
	//해당 배열의 런타임 타입은 E[]가 아닌 Object[] 이다.
	@SuppressWarnings("unchecked")
	public GenericStack() {
		elements = (E[])new Object[DEFAULT_SIZE];
	}
	
	public void push(E e) {
		ensureCapacity();
		elements[size++] = e;
	}
	
	public E pop() {
		if(size == 0)
			throw new EmptyStackException();
		
		E result = elements[--size];
		elements[size] = null;

		return result;
	}
	
	public boolean isEmpty() {
		return size == 0;
	}
	
	public int size() {
		return size;
	}
	
	/**
	 * 사이즈를 늘려준다.
	 */
	private void ensureCapacity() {
		if(elements.length == size)
			elements = Arrays.copyOf(elements, 2*size+1);
	}
	
	
}
```

* 이제 실제 제네릭 타입으로 수행 후 Object 배열에서와 같이 다른 타입의 값을 넣어보도록 하겠습니다.

![](/assets/img/blog/2020-02-02-02-Java-Generic-Exam/2020-02-03-00-08-08.png){: .img-center}

* IDE에서 컴파일 에러를 잡아주고 실제 수행했을 때 컴파일 에러가 나는것을 확인 할 수 있습니다.


***

## 결론

* 제네릭을 사용하면 컴파일 단계에서 에러를 잡을 수 있으므로 더 안전하게 코딩이 가능하다. 


여기까지 읽어주셔서 감사합니다!!