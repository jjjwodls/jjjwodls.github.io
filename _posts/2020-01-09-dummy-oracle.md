---
layout: post
title:  "Oracle Dummy 데이터 생성"
date:   2020-01-09
desc: "오라클에서 Dummy 데이터를 생성하는 방법입니다."
keywords: "Info,JaeIn,오라클 더미데이터,오라클 더미Data,오라클 Dummydata"
categories: [Database]
tags: [Database,Oracle]
icon: icon-database
---

안녕하세요 Dev JaeIn 입니다.

오라클에서 Dummy 데이터가 필요해서 자료를 찾다보니 Level 쿼리를 사용하면 손쉽게 만들 수 있었습니다.

제가 사용했던 방법을 소개합니다.


실제 제가 테스트를 위해 테이블 생성 했던 쿼리입니다.

백만건의 데이터를 생성하며 기본키로는 id, 
추가 칼럼에는 랜덤한 숫자를 넣어 생성하도록 했습니다.

```js
CREATE TABLE dummy AS
SELECT  level AS id,
        '저는 숫자 ' || ROUND(DBMS_RANDOM.VALUE(1, 1000000),0) || ' 가 좋아요' AS customer
FROM    dual
        CONNECT BY level <= 1000000;
```

* 테이블 생성 결과

![](/assets/img/blog/2020-01-09-dummy-oracle/2020-01-10-14-15-16.png){: .img-center}


```js
SELECT LEVEL AS ID FROM DUAL
CONNECT BY LEVEL <=10;
```

* 날짜도 하루씩 증가시키면서 가능합니다.
  
  2018년 12월 1일부터 시작하여 하루씩 증가하는 쿼리입니다.

```js
SELECT TO_DATE('20181201','YYYYMMDD') + (ROWNUM-1) AS CAL
FROM DUAL
CONNECT BY LEVEL <=50;
```

![](/assets/img/blog/2020-01-09-dummy-oracle/2020-01-10-14-16-31.png){: .img-center}

* 다음과 같은 쿼리를 통해 1개월치 날짜만 가져오는것도 가능합니다.

```js
SELECT TO_DATE('20181201','YYYYMMDD') + (ROWNUM -1) AS TEMP_CAL
FROM DUAL
CONNECT BY LEVEL <= (TO_DATE('20181231','YYYYMMDD') - TO_DATE('20181201','YYYYMMDD') + 1);
```

![](/assets/img/blog/2020-01-09-dummy-oracle/2020-01-10-14-18-11.png){: .img-center}

* 마지막으로 1시간씩 증가하는 쿼리입니다. 

```js
SELECT TO_TIMESTAMP('01'+(ROWNUM-1) || ':00','HH24:MI') AS TEMP FROM DUAL
CONNECT BY LEVEL <=23;
```

![](/assets/img/blog/2020-01-09-dummy-oracle/2020-01-10-14-19-04.png){: .img-center}

***
## 정리

위 쿼리들을 이용해 필요한 더미 데이터들이 있을 때 만들어서 사용하시면 됩니다.

***

### 참고
<https://yangyag.tistory.com/120>