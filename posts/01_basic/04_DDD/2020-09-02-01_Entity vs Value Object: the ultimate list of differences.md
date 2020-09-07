---
title: "Entity vs Value Object: the ultimate list of differences"
source: https://enterprisecraftsmanship.com/posts/entity-vs-value-object-the-ultimate-list-of-differences/
date: 2020-09-02 16:00:00
categories: basic
---
<h1>단순 번역 및 요약</h1>

* Identifier eqaulity

> 2개의 objects가 있을 때 identifer가 같은 경우. 안의 다른 attribute는 달라도 상관 없음
>> Identity가 있는 객체 = Entity


* Structural equality
> 2개의 objects가 서로 모든 attribute가 같을 때
>>  Identity가 없는 객체 = Value Object


* 추가적인 차이

> lifespan
>> Entity는 history가 있는 객체이지만 value object는 필요할 때 생성되고 없어진다. 즉 lifespan=0으로 볼 수 있다. value object는 entity에 속성 중 일부로 사용될 수 있다.

> immutability
>> Value Object는 변경될 수 없다. 변경되면 다른 값이다. Entity는 변경될 수 있다. Identifer만 가지고 있으면 상관이 없다.

<h1>나에게 하는 말</h1>
