---
layout: post
title:  "Redis 설치 입니다.(Window)"
date:   2020-01-02
desc: "윈도우 환경에서의 Redis 설치"
keywords: "Redis,Spring"
categories: [Etc]
tags: [Redis,Spring,DB,Server]
icon: server
---
안녕하세요 Dev JaeIn 입니다.

최근 진행했던 Redis 윈도우 설치 후 테스트를 진행했었습니다.

그래서 진행 과정 및 테스트에 대한 내용을 공유하고자 합니다.

우선 설치 방법부터 설명드리겠습니다.

1.<https://github.com/microsoftarchive/redis/releases/tag/win-3.2.100> 에서 윈도우 전용 Redis 를 다운로드 후 압축 해제 합니다.

2.압축을 해제하면 다음과 같은 파일들이 보입니다.

![](/assets/img/blog/2020-01-05-Redis-Setup/2020-01-05-22-14-41.png){: .img-center}

3.Redis-server.exe 를 수행합니다. 

4.서버가 수행되면 cmd 창이 나타나며 서버가 수행중인것이 나타납니다. 포트번호 6379 라고 나타납니다. redis.windows.conf 파일에 기본으로 설정된 서버 포트입니다.

 ![](/assets/img/blog/2020-01-05-Redis-Setup/2020-01-05-22-17-26.png){: .img-center}


5.Redis-cli.exe 를 수행하면 redis 서버는 올라갔고 Redis 를 수행 할 수 있는 콘솔창이 나타납니다.
   테스트로 데이터를 set 해주고 get 해준 모습입니다. 


![](/assets/img/blog/2020-01-05-Redis-Setup/2020-01-05-22-19-25.png){: .img-center}


이상 Redis 윈도우에 설치 및 테스트 진행 결과였습니다.

### 정리
*** 
Redis는 주로 캐시서버로 이용됩니다. Inmemory 방식이므로 서버를 거쳐 처리하게되면 (EX: DB를 통한 데이터 접근 등) 오래 걸리는 데이터들을 

Redis 저장소에 미리 담아 두고 사용자의 요청이 왔을 때 원하는 데이터가 먼저 캐시버서에 있는지 확인 후 

만약 존재한다면 WAS로 접근하지 않고 캐시서버에서 원하는 정보를 응답해주면 됩니다.

이어서 Redis 를 사용했을때와 안했을 때 비교해보겠습니다.

궁금한점은 코멘트 부탁드립니다. 

읽어주셔서 감사합니다!
