---
title: "FeatureBranch"
source: https://martinfowler.com/bliki/FeatureBranch.html
date: 2020-05-07 09:00:00
categories: basic
---
<h1>단순 번역 및 요약</h1>

<h2>Simple (isolated) Feature Branch</h2>
...DVCS(Distributed Version Control System)과 feature-branch에 대한 기본적인 설명...(생략)

Textual conflict(merging을 진행하면서 동시에 하나의 소스코드를 수정하여 발생하는 충돌)들은 현재 다양한 방법들로 해결되고 있다. 중요한 것은 semantic conflict이다. 예를 들어 A 개발자가 B 개발자가 사용하는 method의 이름을 바꾸는 경우이다. 리팩토링 도구들은 renaming을 지원한다. 하지만 그 경계는 당신의 code base(branch) 안에서만 이루어진다. 이 경우 merge가 진행되면 문제가 될 수 있다. 이는 간단한 예시이고 이 문제는 훨씬 다양하다. 주로 이를 막기 위하여 testing을 사용하지만 merge 되는 코드가 많아지면 충돌도 많아질 것이고 수정은 점점 어려워질 것이다. 이는 리팩토링을 주저하게 만들 것이고 점차 코드를 보기 어렵게 만들 것이다. 이것이 내가 feature branching이 나쁜 아이디어라고 생각하게 된 결정적인 요인이다.

<h2>Continuous Integration</h2>
이 문제는 더 빈번하고 더 작은 여러 번의 merge를 진행하는 방법으로 해결될 수 있다. CI는 이를 해결하는 효율적인 방법이자 살아있는 의사소통 방식이다. 이를 통해 문제가 커지기 전에 더 빨리 찾고 쉽게 해결할 수 있다. 한 가지 강조하고 싶은 것은 feature branching은 CI 접근과는 다르다는 것이다. CI의 기본 개념은 모든 사람이 매일 mainline에 commit을 하는 것이다. feature branch가 하루 이하의 크기로 운영되는 것이 아니라면 feature branch와 CI는 다른 개념이라는 것을 명심해라. 그리고 가끔 continuous building을 CI랑 헷갈리는 사람도 있는데 이것 또한 주의가 필요하다.

<h2>Promiscuous Integration</h2>
이전에 나는 feature branching을 대체하는 다른 방법이 있다고 말하였다. 서로 연관이 있는 서로 다른 두 개의 branch가 mainline을 통해서 소통하는 것이 아니라 서로 소통하는 것이다. 이 경우 mainline에는 마지막에 단 한 번만 push할 수 있다. 이는 매일 mainline에 commit을 하는 CI와는 또 다른 개념이다. CI의 목적은 commit들이 production-readiness에서 오류가 있는지 점검하는 것이다. CI는 현재 공유되는 가장 최근에 완성된 그림을 나타낸다.

<h2>Promiscuous Integration vs Continuous Integration</h2>
PI는 mainline을 직접적으로 건들지 않는다는 점이 좋지만 서로 다른 branch에서 누가 어떤 작업을 하고 있는지 매번 알기 쉽지 않다는 단점이 있다. 다행히 많은 tool들이 이를 해결해주고 있다.

하지만 나는 VCS를 cherry-picking으로 쓰는 것이 좋은 아이디어라고 생각하지는 않는다. 어떤 feature를 넣거나 빼는 작업을 할 때마다 VCS에 종속이 되어버리기에 이를 해결하기 위한 유용한 기술로 FeatureToggles, BranchByAbstraction이 존재한다. 이 기술들은 무엇이 모듈화될 필요가 있는지 그리고 어떻게 변화를 관리할 것인지에 대한 생각을 요구한다. 그러나 VCS를 이용하는 것보다 그렇게 어렵지도 않다.

PI는 또한 오픈소스에서 문제가 발생한다. 다른 사람이 뭘하는지 알 수가 없는 상태이기 때문이다. 또한 사람마다 개발에 투자하는 시간과 속도가 다르기 때문에 누군가는 자신의 목적에 이미 완성된 상태를 만들어내고 다른 사람은 아직 작업 중일 수도 있다. 이런 상황 속에서 먼저 완성된 시스템을 사용하여 cherry-picking을 하는 것이 더 중요할 수도 있다.

당신이 사용하는 툴과 당신이 선택한 integration 전략은 서로 독립적이라는 것을 아는 것이 매우 중요하다. 많은 사람들이 DVCS를 feature branching으로 생각하는데 이것은 CI가 쓰일 수도 있다. 당신이 해야할 것은 repository의 하나의 branch를 mainline으로 정하는 것이다. 만약 모든 사람들이 매일 그 branch에 pull/push를 진행한다면 이것은 CI mainline이 되는 것이다. 실제로 능숙한 팀들과 함께라면 나는 CI와 DVCS를 더 선호한다. 그렇지 않은 팀이라면 DVCS는 점점 길어지는 branch를 만들 수 있다. 반면에 centralized VCS와 branch reluctance(branch에 대한 비익숙함)는 그들 자신이 더욱 mainline에 자주 commit하도록 만들 것이다. Paul Hammant : "I wonder though, if a team should not be adept with trunk-based development before they move to distributed."

<h1>나에게 하는 말</h1>

* CI/PI에 대한 정확한 개념 없이 상황에 따라 맞춰 사용해왔던 것 같다.
* Feature를 넣고 빼는 작업이 용이한, 단순하게 시간 순서에만 구애받지 않는 기술 구현의 중요성에 대해 생각하게 되었다.
* 익숙하지 않은 팀이라면 DVCS+CI보다 centralized VCS를 통한 관리가 더 유용할 수도 있다.