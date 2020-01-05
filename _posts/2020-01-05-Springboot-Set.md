---
layout: post
title:  "스프링부트(Spring Booot) 설치 및 구동입니다."
date:   2020-01-05
desc: "블로그 개설 후 소개하기 위해 작성합니다."
keywords: "Info,JaeIn"
categories: [Spring]
tags: [Spring,Java,SpringBoot]
icon: icon-html
---

안녕하세요 Dev JaeIn 입니다.

이번엔 SpringBoot 설치 및 기본 구동을 진행해보겠습니다.

우선 스프링 부트 다운로드를 진행합니다. 

1.(https://spring.io/tools) 에 가셔서 환경에 맞는 툴을 다운로드 진행합니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-35-16.png){: .img-center}

2.다운로드 후 압축을 해제합니다. 알집으로 압축 해제 했더니 글자수가 긴 파일명들이 압축 해제가 잘 안되었습니다. 그런 경우 압축 프로그램을 변경하신 후 다시 시도해주시면 됩니다. 
전 반디집으로 진행했습니다.

압축 해제 후 content.zip 이라는 압축이 하나 더 있었고 해당 파일을 압축을 풀면 SpringToolSuite라는 exe 파일이 들어있습니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-37-47.png){: .img-center}

3.해당 파일을 수행하면 workspace 지정하는 창이 나타나는데 자신이 원하는 workspace 폴더를 생성 또는 사용하시면 됩니다.

4.테스트 진행을 위해 Spring Stater Project 를 만들어 보겠습니다. 
File -> New -> Spring Stater Project 를 클릭합니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-39-57.png){: .img-center}

5.NAME에 프로젝트 이름을 작성합니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-40-37.png){: .img-center}

6.Next 버튼을 클릭하면 현재 프로젝트에 필요한 기능을 선택 할 수 있습니다. 전 Web Project를 생성 후 테스트 하기 위해 하단에 Web을 펼친 후 Spring Web을 선택했습니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-41-46.png){: .img-center}

7.잠시 기다리면 pom.xml에 설정된 파일들을 maven 저장소에서 받아 온 후 프로젝트가 생성됩니다. 
생성 후 디렉토리 구조는 다음과 같습니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-50-13.png){: .img-center}

프로젝트네임+Application.java 파일이 스프링Boot를 수행 하는 메인 파일입니다.
기존 웹프로젝트와는 다르게 Tomcat 이 내장되어 있어 생성하지 않아도 됩니다.

8.프로젝트 수행은 다음과 같이 Spring Boot App을 클릭하면 서버가 구동됩니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-52-24.png){: .img-center}

9.콘솔창에 다음과 같이 나타나면 서버가 정상적으로 수행된 것입니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-06-00-00-27.png){: .img-center}


하지만, 실제 서버(localhost:8080)에 접속하면 다음과 같이 나타납니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-55-08.png){: .img-center}

해당 url에 대한 response 할 수 있는 페이지가 없기 때문입니다. 

간단한 테스트를 위해 컨트롤러를 하나 생성합니다.

전 다음과 같이 RestController 를 이용해 생성했습니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-57-49.png){: .img-center}

이렇게 생성 후 서버를 재기동 하고 다시 (localhost:8080) 으로 접속하면 다음과 같이 정상적인 화면이 출력되는 것을 확인 할 수 있습니다.

![](/assets/img/blog/2020-01-05-Springboot-Set/2020-01-05-23-59-20.png){: .img-center}

이상 스프링 부트 설치 및 테스트에 대한 예제였습니다. 

감사합니다.

