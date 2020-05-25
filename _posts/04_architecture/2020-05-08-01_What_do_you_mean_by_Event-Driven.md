---
title: "What do you mean by Event-Driven?"
source: https://martinfowler.com/articles/201701-event-driven.html
date: 2020-05-08 09:00:00
categories: architecture
---
<h1>단순 번역 및 요약</h1>
<h2>Event Notification</h2>
도메인 내에서의 변화에 대해서 다른 시스템에 알리기 위해 event message를 보낸다. Event notification의 주요 특성은 event를 보내는 시스템은 event를 받는 시스템의 결과에 신경을 쓰지 않는다는 것이다. 답이 없을 수도 있고 답이 있어도 이에 대해 간접적으로 반응할 것이다.

Event는 두 시스템의 decoupling을 매우 쉽게 만든다. 따라서 event notification과 관련된 다양한 로직이 있다면 flow를 따라가는 것이 실제 시스템을 모니터링 하는 것 밖에 없는 상황이 생길 수도 있으며 전체 거대한 시스템의 flow를 보기 어려워진다는 문제도 있다.

만약 event가 수동적 공격성향의(passive-aggressive) command일 때 문제가 발생한다. recipient가 무언가를 할 것이라 생각하거나, 어떤 의도를 표현하기 위한 command message를 event라는 형태로 보내게 될 때이다.

event는 많은 데이터를 전달할 필요가 없다. receiver는 변화가 있었다는 최소한의 정보만 받는다. 하지만 그 다음에 sender가 다음에 무엇을 할지 결정하기 위한 정보를 전달한다.

<h2>Event-Carried State Transfer</h2>
이 패턴은 source system에 대한 접근 없이 시스템의 client를 업데이트하고자 할 때 나타날 수 있다. customer management system이 변화한 client 정보를 event로 보내면 각 recipient는 자신들이 가지고 있는 customer data의 복제본에 이 변화를 적용하기만 하면 된다. 이는 main customer system과의 종속성을 끊어낸다. 하나의 단점은 많은 데이터의 복사본이 생길 수 있다는 것인데 이보다는 우리가 얻게될 장점이 훨씬 많다. Main system이 죽어도 상관이 없으며 latency도 줄일 수 있다.

<h2>Event-Sourcing</h2>
Event sourcing의 주된 아이디어는 상태 변화가 생길 때마다 해당 event를 기록하는 것이다. 이를 통해 시스템의 rebuilding, reprocessing 등이 가능해진다. 예를 들어 Git을 생각하면 된다. 

Event sourcing과 관련된 많은 오해가 event processing이 asynchronous이여야 한다는 것이다. 하지만 git은 완전하게 synchronous 동작으로 움직인다. 또 다른 오해는 유용한 정보를 찾기 위해서는 event-sourced system을 이용하는 모두가 event log에 대해 이해하고 접근해야 한다는 것이다. 하지만 event log에 대한 지식을 모두 알 필요는 없다. 나 또한 현재 나의 source tree의 모든 commit을 모른다. event log의 정보가 필요한 부분에서만 event log를 다루면 된다. Domain processing과 event log로부터의 작업은 명백하게 분리될 수 있기 때문이다.

Event sourcing을 통하여 지난 state에 대해 재생성하거나 가상의 event를 넣어서 다른 기록을 만들고 조회해볼 수도 있다. 또한 Memory Image와 같은 무기한의 working copies를 만들 수도 있다. 하지만 해당 과정에서 외부 시스템에 종속이 있다면 이는 문제가 될 수도 있다. 따라서 우리는 시간에 따른 이벤트 스키마의 변경을 어떻게 다룰지에 대해 생각해봐야 한다.

<h2>CQRS</h2>
CQRS는 reading과 writing 데이터 구조를 분리하는 개념을 말한다. CQRS 자체는 event에 관한 개념은 아니다. 하지만 event와 잘 어울리기 때문에 많은 사람들이 CQRS를 쓸 때 event를 적용하고 있다. 만약 당신 시스템의 read/write가 복잡하거나 차이가 크다면 이런 구조를 고려해봐라.

<h2>Making sense of these patterns</h2>
개념에 대한 혼동과 얕은 지식을 피하기 위해 노력해달라. 저자의 예시로는 CQRS와 event-driven을 혼동하는 사람, asynchronous 기능 때문에 event-sourcing 또는 CQRS에서 문제가 많이 발생한다는 사람들을 보아서 많이 혼동스럽다고 한다. 또한 모든 좋은 기술은 적합한 곳에 쓰여야 좋은 기술이 되는 것이라는 것을 명심하길 바란다.

<h1>내 의견</h1>
* Event-driven system에 대해서 헷갈릴 때는 Git을 잘 생각해보자.
* Event processing이 꼭 비동기적으로 진행될 필요는 없다.
* Domain processing과 event log은 분리되어야 한다.