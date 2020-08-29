---
title: "The Standard of Code Review"
source: https://google.github.io/eng-practices/review/reviewer/standard.html
date: 2020-05-14 11:00:00
categories: basic
---
<h1>단순 번역 및 요약</h1>
<h2>The Standard of Code Review</h2>
Code의 질을 올리기 위한 위해서 trade-off의 균형이 맞춰져야 한다. 

첫 번째는 개발자들이 그들의 업무에 대해 개선할 수 있어야 한다. 아무도 변경 사항을 내지 않는다면 아무런 발전이 없을 것이다. 또한 reviewer가 진행되는 변경 사항에 대해 많은 어려움을 요구할 경우 개발자들은 그런 수정을 진행하는데 있어서 거리낌이 생길 것이다.

두 번째는 reviewer들은 코드 변경이 시간이 시간이 지남에 따라 품질이 떨어지지 않는다는 것에 확신을 해야 한다. 가끔 시간에 쫓기거나 간단한 프로토타입을 만들 때 발생하는 조그마한 코드 품질 변화가 나중에는 전체적인 코드의 품질을 낮추게 된다. reviewer는 그들이 보는 코드에 대하여 코드가 일관적이고 유지 가능하도록 주인의식과 책임감을 가져야 한다.

우리는 하나의 룰을 정했다 : 해당 변경 사항이 명확하게 전체 코드 품질을 향상시킨다면 reviewer는 일반적으로 변경을 승인해줘야 한다. 만약, 변경 사항이 완벽하지 않더라도.

중요한 것은 완벽한 코드가 아니라 더 좋은 코드이다. reviwer는 그들이 나아가는 방향과 변경 사항의 중요도 사이의 균형을 찾아야 한다. 완벽함을 찾기보다는 continuous improvement을 찾아야 한다. 더 좋은 코드를 만들 수 있는 변경 사항이 완벽한 코드를 위해 긴 시간이 지체되서는 안 된다. 이런 부분은  "Nit"를 이용하여 전달할 수 있다. 그렇다고 이 글이 전체 코드 품질을 떨어뜨리는 변경 사항을 정당화시켜주는 것은 아니다.

<h2>Mentoring</h2>
코드 리뷰는 개발자에게 새로운 것을 가리키는 역할도 있다. 개발자가 새로운 것을 배우게 하기 위해 comment를 다는 행위는 언제나 좋다. 지식을 공유하는 것 또한 결국 코드의 품질을 개선하는 것이다. 만약 너의 comment가 중요한 사안은 아니지만 가르쳐주고 싶은 내용일 때도 "Nit"를 사용해라.

<h2>Principles</h2>
* Google style guide에 있는 것 외의 스타일은 개인의 선호이다. 이미 존재하던 스타일을 따라가면 되고 존재하지 않는다면 가이드를 따라도 된다.
* 소프트웨어 디자인에서의 스타일 이슈나 개인의 선호 같은 것은 거의 존재할 수 없다. 아주 가끔 더 좋은 의견이 있을 수 있지만 대부분 소프트웨어 디자인의 이론을 따라야 한다.
* 만약 적용된 아무 룰이 없다면, reviewer는 시스템의 전체 품질을 떨어뜨리지 않는 기한 내에서 개발자에게 현재 코드와의 일관성을 유지시키도록 요구할 수 있다.

<h2>Resolving Conflicts</h2>
코드 리뷰에서의 의견 충돌에 있어서 처음해야 할 것은 개발자와 리뷰어가 관련 문서들에 기반하여 의견을 맞추는 것이다. 만약에 이것이 쉽지 않다면 대면 미팅을 통해서 진행하는 것이 좋을 것이다. 그래도 해결이 안 된다면 이 충돌을 escalating(상정)하는 것이다. 기술 리더가 함께하는 팀 토론에서 진행하는 것이다. 개발자와 리뷰어가 의견이 일치되지 않는다고 코드를 방치해두는 일은 절대 해선 안 된다.

<h1>나에게 하는 말</h1>

* 코드 리뷰에 있어서 완벽한 코드만을 추구하려고 하지 말자.
* 소프트웨어 디자인을 더 깊게 공부하자.