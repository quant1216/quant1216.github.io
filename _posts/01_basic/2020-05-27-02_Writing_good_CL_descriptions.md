---
title: "Small CLs"
source: https://google.github.io/eng-practices/review/developer/small-cls.html
date: 2020-05-27 11:00:00
categories: basic
---

<h1>단순 번역 및 요약</h1>
CL description은 어떤 변경 사항이 생겼고, 왜 생겼는지에 대한 공개된 history입니다. 이 history은 version control history의 영구적인 부분이 될 것이고 수 년동안 수 백명의 사람이 읽을 수 있어야 합니다.

미래의 개발자들은 당신의 description을 기반으로 변경 사항을 찾을 것입니다. 미래의 누군가는 관련된 희미한 기억 속에서 세부 사항을 기억하지 못하기 때문에 CL을 찾는 과정을 진행할 것입니다. 만약 모든 중욯한 정보가 code에 있고 description에는 없다면 당신의 CL을 찾아내기까지 훨씬 힘든 시간이 될 것입니다.

<h3>First Line</h3>
 
* 무엇이 진행됬는지에 대한 간략한 요약
* 명령문으로 쓰이지만 완성된 문장
* 빈 줄 추가

CL description의 첫 번째 줄은 CL에서 무엇이 진행되었는지에 대한 구체적인 부분에 대한 요약입니다. 이 부분이 version control system history의 요약으로 보여지게 될 것입니다. 따라서 이 부분은 충분한 정보를 줄 수 있어야 합니다. 이는 전체 CL과 description을 모두 볼 필요가 없어도 어떤 부분이 변경되었고 어떻게 다른 CL과 다른지를 알 수 있을만큼 충분한 정보를 내포해야 합니다. 첫 번째 줄은 독자들이 code history를 쉽게 훑어볼 수 있도록 따로 분리되어 있어야 합니다.

첫 번째 줄은 짧고, 집약적이며, 요점을 유지해야 합니다. 읽는 이의 명확성과 이용성이 가장 중요한 가치로 고려되어야 합니다.

관습적으로 CL description의 첫 번째 줄은 명령조의 완전한 문장입니다. 예를 들면, "특정 RPC를 지우기 그리고 새로운 시스템으로 대체하기" 보다는 "특정 RPC를 지우고 이를 새로운 시스템으로 대체해라"를 사용합니다. description의 나머지 부분도 명령문으로 작성할 필요는 없습니다.

<h3>Body is Informative</h3>
Description의 나머지 부분들은 유용한 정보를 제공해야 합니다. 이것은 CL에서 해결한 문제에 대한 간략한 설명과 왜 이러한 해결법을 선택했는지에 대해 설명되어야 합니다. 만약 해당 해결법에 단점이 있다면 이또한 언급되어야 합니다. 만약 관련이 있다면, 배경 지식(버그 번호, 벤치마크 결과, 디자인 문서에 대한 링크)을 포함시키세요.

만약 외부 링크를 전달한다면, 이 링크가 미래의 다른 독자들은 볼 수 없을 수도 있다는 것을 명심해야 합니다. 따라서 가능한 충분한 정보를 description에 함께 담도록 하세요.

사소한 CL은 세부 사항에 대해 더 적은 주의를 기울일 것입니다. 이 때는 CL을 context에 넣도록 하세요.

<h3>Bad CL Descriptions</h3>
명확한 설명이 없는 description은 좋지 않습니다. 무엇이 버그인지? 무엇이 수정되었는지? 를 나타내야 합니다. 예를 들어

* "빌드 수정해라"
* "패치를 추가해라"
* "A에서 B로 코드 옮기기"
* "1단계"
* "Conversion 함수를 넣어라"
* "이상한 URL들을 없애라"

와 같은 CL description은 충분히 유용한 정보를 제공하지 않습니다.

<h3>Good CL Descriptions</h3>

* Functionality change
> rpc: RPC 서버의 메시지의 freelist 갯수 제한을 제거해라.
>
> 해당 서버는 매우 큰 메시지들을 가지고 있고 재사용함으로써 이득이 있다. 사이즈 제한을 늘리고 goroutine으로 freelist를 천천히 메모리 해제시켜라. 이는 최종적으로 쉬고 있는 서버는 모든 freelist를 메모리 해제할 것이다.

시작하는 몇개의 단어는 CL이 무엇을 하는지 알려준다. description은 어떤 문제가 해결되었고 이것이 왜 좋은 해결책인지, 그리고 구체적인 구동에 대해 설명하고 있다.

* Refactoring
> TimeKeepper의 TimeStr과 Now 함수를 활용하기 위해, TimeKeepper를 사용하는 함수를 생성해라.
>
> Now 메소드를 추가해라, 이는 borglet() getter 메소드를 제거할 것이다. 이 작업은 Borglet의 기능을 TimeKeeper로 대체할 것이다.
>
> Now 메소드를 사용하는 테스크를 만드는 작업은 Borglet으로부터의 의존성을 제거하는 시작이 될 것이다. 결국에는 Now를 구하기 것에 의존적인 다른 업무들도 TimeKeepper를 직접 이용하는 방향으로 바뀔 것입니다. 하지만 이 작업은 리펙토링을 염두에 둔 시작 단계입니다. Borglet을 리팩토링 하기 위한 큰 목표는 계속 될 것 입니다.

첫 번째 줄은 CL이 무엇을 하고 과거로부터 어떻게 바뀌었는지를 나타냅니다. 나머지 설명부는 구체적인 실행에 대해 말합니다. CL에 따르면 이는 이상적인 해결책은 아니고 가능한 방안일 뿐입니다. 또한 이 변경이 왜 필요한지에 대해 설명합니다.

* Small CL that needs some context
> status.py를 위해 python3 빌드 규칙을 만들어라
>
> 이 작업은 이미 python3를 쓰고 있는 소비자들이 자신의 규칙이 아닌 기존의 status 빌드 규칙을 따르도록 만듭니다. 이 작업은 새로운 유저가 python2 대신 python3를 만들게 할 것이며, 현재 동작하고 있는 자동화된 빌드 파일 리펙토링 도구를 매우 간편하게 만들 것입니다.

첫 줄은 실제로 무엇을 하였는지 나타냅니다. 나머지 부분은 왜 변화가 생겼는지 설명하고 reviewer들에게 많은 context를 전달합니다.

<h3>Generated CL descriptions</h3>
몇 CL은 툴에 의해 만들어집니다. 가능하다면 언제나, 그들의 description 또한 이 글의 조언을 따라야 합니다. 첫 번째 줄은 짧고, 집약적이며, 설명이 충분하며 나머지 부분은 reviewer와 미래의 코드를 찾아볼 사람들이 변경 사항의 영향에 대해서 알 수 있도록 상세한 정보를 포함해야 합니다.

<h3>Review the description before submitting the CL</h3>
Review가 진행되는 동안 CL에 큰 변경 사항이 있을 수 있습니다. CL을 제출하기 전에 CL description이 변경 사항을 잘 반영하고 있는지 점검하는 것은 매우 중요합니다.

<h1>나에게 하는 말</h1>

* CL description의 첫 번째 줄이 매우 중요하다.
* 관습적으로 명령문으로 작성한다.
* CL description을 쓰는 부분에 있어서 예시 파일들을 떠올리며 상세하게 적도록 노력하자.
