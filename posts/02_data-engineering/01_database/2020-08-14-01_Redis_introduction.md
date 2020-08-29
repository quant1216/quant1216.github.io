---
title: "Redis introduction"
source: https://redis.io/topics/introduction
date: 2020-08-14 16:00:00
categories: data engineering
---
<h2> Transactions </h2>
EXEC를 기점으로 모든 command들은 실행되지 않거나(queued) 실행된다.

MULTI 를 이용하여 command들을 queue에 넣을 수 있다. EXEC가 입력되면 모두 실행된다. 

만약 syntax 수준의 에러가 있다면 이는 바로 감지된다. 대부분의 client는 이 transaction을 버리는 작업을 하지 않는다. 하지만 Redis 2.6.5 이 후 버전에서는 이 transaction을 버린다. 2.6.5 이전에는 이런 오류가 성공적으로 queue에 들어간 다른 커맨드들을 실행시키는 것이었다. 즉 오류 이전까지를 EXEC 한다. 새롭게 변경된 방법은 transaction을 pipelining 과 섞는 작업을 더 쉽게 할 것이다.

만약 EXEC를 실행시킨 후에 발생하는 오류라면 이는 오류 command는 제외하고 다른 command들은 모두 실행한다. 즉 이런 경우 OK와 ERR 모두 return되는 경우가 존재한다.

왜 roll back을 진행하지 않고 일부 command는 실행, 일부 command는 실패로 처리할까? 
* Redis는 syntax 수준의 오류는 찾지만 그렇지 않은 에러에서 문제가 발생한다. 대부분 프로그래밍 에러이고 아마 개발 중에 발견될 것이다.
* Roll back에 대한 기능을 없앰으로써 redis는 내부적으로 간단하고 더 빠를 수 있다.

WATCH command를 이용하여 특정 key를 monitoring 한다. 이는 EXEC가 실행되기 전에 값의 변화가 생기면 EXEC는 실패한다. 이는 static variable을 사용하고 싶을 때 쓸 수 있다. Client는 하나가 아니기 때문에 특정 변수에 대해 여러 client가 접근하여 데이터를 변경하면 문제가 발생한다. 이를 막기 위해 사용될 수 있다.

<h2> Pub/Sub </h2>
Publish, Subscribe를 의미한다. Subscribe 상태에 돌입하게 되면 redis command를 사용할 수 없다. Pub/Sub은 key space와는 관련이 없도록 만들어졌다.

<h2> Lua scripting </h2>
EVAL 함수를 이용하여 lua script를 redis server에서 사용하는 방법.

<h2> Keys with a limited time-to-live </h2>
Key를 expire하는 방법이 존재한다. EXPIRE를 없애는 방법은 DEL, SET, GETSET and all the *STORE commands이다. 즉 key를 건들지 않고 value만 바꾸는 경우에는 timeout에 영향을 미치지 못한다. key를 persistent로 바꾸는 PERSIST command는 timeout을 없애지만 RENAME은 timeout을 그대로 가져간다. 만약 EXPIRE/PEXPIRE에서 시간을 과거 또는 음수로 정할 경우 key는 바로 제거된다.

만약에 이미 존재하는 key에 대해 EXPIRE를 실행하면 기존 key의 timeout을 업데이트한다.



<h1>나에게 하는 말</h1>

* Roll back을 지원하지 않는 이유가 너무 인상적이다. 개발할 때 다 찾을 것이기 때문에..
* WATCH command를 이용하여 데이터를 static하게 사용한다고 하였는데 database라면 key와 상관 없이 모든 데이터에 대해 이런 기능을 제공해야 하는 것이 아닌지..
* 