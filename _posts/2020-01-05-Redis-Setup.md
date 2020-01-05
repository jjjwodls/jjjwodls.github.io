---
layout: post
title:  "Redis 설치 입니다.(Window)"
date:   2020-01-02
desc: "윈도우 환경에서의 Redis 설치"
keywords: "Redis,Spring"
categories: [Etc]
tags: [Redis,Spring,Java]
icon: icon-html
---

안녕하세요 Dev JaeIn 입니다.

최근 진행했던 Redis 윈도우 설치 후 테스트를 진행했습니다.

우선 설치 방법입니다.

1. https://github.com/microsoftarchive/redis/releases/tag/win-3.2.100 에서 윈도우 전용 Redis 를 다운로드 후 압축 해제 합니다.

2. 압축을 해제하면 다음과 같은 파일들이 보입니다.
![](/assets/img/blog/2020-01-05-Redis-Setup/2020-01-05-22-14-41.png)

3. Redis-server.exe 를 수행합니다. 
 ![](/assets/img/blog/2020-01-05-Redis-Setup/2020-01-05-22-17-26.png)
4. 서버가 수행되면 cmd 창이 나타나며 서버가 수행중인것이 나타납니다. 포트번호 6379 라고 나타납니다. 
   redis.windows.conf 파일에 기본으로 설정된 서버 포트입니다.

5. Redis-cli.exe 를 수행하면 redis 서버는 올라갔고 Redis 를 수행 할 수 있는 콘솔창이 나타납니다.
   테스트로 데이터를 set 해주고 get 해준 모습입니다. 
![](/assets/img/blog/2020-01-05-Redis-Setup/2020-01-05-22-19-25.png)

이상 Redis 윈도우에 설치 및 테스트 진행 결과였습니다.

- 정리 
Redis는 주로 캐시서버로 이용됩니다. Inmemory 방식이므로 서버를 거쳐 처리하게되면 오래 걸리는 데이터들을 담아 두고 

사용자의 요청이 왔을 때 캐시서버에서 원하는 정보를 넘겨주면 빠르게 응답이 가능하겠죠 ? 

이어서 Redis 를 Spring과 연동한 것과 하지 않은것의 차이점에 대해 알아보도록 하겠습니다.

궁금한점은 코멘트 부탁드립니다. 감사합니다.
