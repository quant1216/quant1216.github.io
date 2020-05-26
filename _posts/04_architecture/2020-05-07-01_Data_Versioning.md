---
title: "Data Versioning"
source: https://emilygorcenski.com/post/data-versioning/
date: 2020-05-07 08:00:00
categories: architecture
---
<h1>단순 번역 및 요약</h1>
Machine learniing system은 model, data, code 이 세 가지를 고려하여야 한다.

<h2>Putting Intelligence into the Stack</h2>

<h3>Understanding Models</h3>
Model이란 domain(the space of inputs)과 range(the space of possible outputs)으로 제한될 수 있다. 여기서의 모델은 하이퍼파라미터와 알고리즘의 결합을 의미한다(이것은 일반적인 파라미터와는 다르다. 파라미터는 데이터에 종속이 있는 것이다.). 

<h3>Understanding Data</h3>
데이터에 있어서 가장 신경써야할 두 가지 사항은 데이터의 형태와 분포이다. 형태(스키마)는 데이터가 있는 공간의 특성을 나타내고, 분포(value)는 데이터가 어떤 값을 가지고 있는지를 알려준다.

<h4>Understanding How Values Change</h4>
잘못된 데이터가 들어오거나 잘못된 모델이 만들어졌을 때, roll back 시킨 후 재학습하는 과정들을 지원하기 위해 데이터 값에 대한 versioning이 필요하다.

<h4>Understanding Schemas</h4>
스키마가 변경되는 경우를 지원하기 위해 스키마의 versioning이 필요하다.

<h3>Understanding Code</h3>
ML에게 코드라는 것은 시스템 실행 코드와 model 모델 개발 코드로 나눠질 수 있다.
코드를 버전관리 한다는 것은 매우 당연한 일이다. 하지만 한 가지 더 중요한 사안이 있는데 대부분의 ML 산출물들은 code dependency들을 갖고 있기 때문에 dependency 관리 또한 모델은 관리하는 것만큼 중요한 것으로 간주해야 한다.

<h2>Addressing Machine Learning Version Challenges</h2>
ML은 4가지의 방향에 따른 version control이 필요하다. 모델, 데이터 값, 데이터 스키마, 코드 그 자체.

<h3>Treat Models as Code</h3>
하나의 방법은 모델과 모델을 만드는 것 모두 code로 관리하는 것이다. 보통 python, R과 같은 언어로 모델을 만들기 때문이다. Tensorflow를 예로 든다면 model을 코드의 하나의 extension으로 관리할 수 있다.

<h4>Where this goes right</h4>
이 접근은 혁신적인 접근이다. 우리는 개발과 롤백(이전 모델로 되돌아가는 롤백을 포함하여)이 함께 개발될 것이다. 즉 논리적으로 분리된 개념이 함께 엮이게 된다. 데이터 사이언티스트들은 silo(중복 작업)을 하지 않겠지만, 서로 다른 팀이 완전히 통합될 것입니다.

<h4>Where this goes wrong</h4>
이 접근의 어려운 점은 모델링 방법이 ML 시스템의 다른 성분들과 어떻게 연결되는지 생각하기 어렵다는 것이다. 그리고 이는 결과적으로 당신의 코드를 당신의 데이터, 당신의 스키마에 상호 종속이 있게 만들 것이다. 이것은 큰 문제가 될 수도 있다.

<h4>What to look out for</h4>
당신이 모델과 코드가 상호 종속이 있다면, 이것은 작업을 매우 어렵게 만들 것이다. 예를 들어 데이터 사이언티스트가 실험을 하고자 할 때 개발과 관련된 부분을 작업할 필요가 있고, 당신은 다수의 모델을 다루기 위한 복잡한 feature-flagging 시스템이 필요할 것입니다. 모델이란 하이퍼파라미터와 알고리즘의 결합이기에 시간이 지남에 따라 옛날 모델로 돌아가게 되는 경우가 많을 것인데 이는 새롭게 개발만 되어지는 다른 코드들과는 다를 것입니다.

<h4>Recommendations</h4>
나는 모델을 코드로 다루기를 추천합니다. 우리가 infrastructure를 코드로 다루는 것과 동일한 이유때문입니다. 비록 몇 가지 문제가 있긴 하지만 이것은 두 개의 관련된 개념을 연결짓는 자연스러운 방법이고 내 경험상 부정적인 영향이 가장 적었습니다.

<h3>Link Schemas and Values</h3>
이 접근은 매우 자연스럽습니다. 데이터를 다룰 때, 우리는 데이터를 blob으로 저장합니다. 이 값은 schema가 어떤 모양이든 맞춰서 사용될 수 있습니다.

<h4>Where this goes right</h4>
이것은 아마 구동하기 가장 쉬운 방법일 것입니다. 우리는 트레이닝 시점에 간단하게 데이터 테이블을 dumping할 수 있습니다. 데이터 테이블은 어디에나 저장될 수 있고, 데이터마트/materialized viedw/stored query를 이용하는 다양한 기술들을 사용할 수 있습니다. 그리고 우리가 values와 schema를 연결한다면 우리는 데이터가 특정 시점에 무엇을 의미하는지 명확해집니다. 이것은 missing value를 채워놓을 필요가 없게 하고, 오래된 데이터를 새로운 스키마에 맞추기 위한 복잡한 어댑터/인터페이스를 필요없게 합니다. 이것은 새로운 field에 대해서 default 값을 추론해야 하는 문제도 제거해줍니다.

<h4>Where this goes wrong</h4>
이것은 단지 데이터를 dumping 하는 것보다 조금 더 복잡합니다. 이것은 스키마와 데이터를 연결시키기 위한 툴들의 종속성이 존재합니다. 데이터와 스키마가 연결되어 있기에, 스키마 변경은 옛날 스키마에서 새로운 스키마로 넘어오는 데이터에 문제를 일으킬 수 밖에 없습니다. 데이터 마이그레이션을 해본 사람이라면 이게 무슨 뜻인지 알 것 입니다.

<h4>Recommendations</h4>
데이터를 스키마와 연결시키는 것은 추천하지 않습니다. 당신의 use case를 고려해보는 게 좋습니다. 만약 대규모 마이그레이션 또는 빈번한 스키마 변경이 있다면 필요할 때만 스키마와 다른 것들을 연결시키는 것이 좋습니다.

<h3>Link Data and Code using Version Control for Data</h3>
DVC에서 사용되는 방식입니다. 데이터를 저장소에 넣으면서 source control system에 코드와 함께 commit 할 수 있도록 작은 stub를 만듭니다.

<h4>Where this goes right</h4>
이 기능을 제공하는 MLFlow, Pachyderm, dvc는 인기를 얻고 있습니다. 작업을 공유하고 반복하기 쉽게 만들며 training/validation 데이터가 동일할 수 있도록 보장해줍니다. 저장소는 저렴하고 존재하는 source control tools과의 통합도 매우 잘 됩니다. 

<h4>Where this goes wrong</h4>
많은 저장소를 사용할 것입니다. 우리는 잠재적으로 동일한 데이터를 여러번 복제하기 때문입니다. 이 부분을 다루는 기능은 현재 완벽하지 않습니다.

<h4>Recommendations</h4>
당신의 데이터 저장소에 대해 이 기술을 가능한 구동시켜보고 사용해보기를 추천합니다. HDFS를 이용한다고 하더라도, 파일 경로의 리스트를 version control할 수 있으며 이에 대한 참조를 유지할 수 있습니다. blob 데이터에 대해서는, 개발된 어떤 둘을 이용해서라도 해쉬되고 관리되는 데이터 셋을 만들 수 있습니다.

<h3>Unlink Data/Models from Code</h3>
내가 해본 다른 접근은 모델과 데이터를 코드와 분리하는 것이었다. 새로운 여러 모델들을 학습시키고 서비스가 필요로 할 때 실행시키는 것이었다.

<h4>Where this goes wrong</h4>
이건 continuous delivery가 아니다. 데이터 사이언티스트들에게 silo(중복 작업)을 야기했다.

<h4>Recommendations</h4>
추천하지 않는다. 이 방법은 여러 통합 이슈를 만들어내는 방법이다.

<h3>Restrict Modeling Approaches</h3>
당신은 기본적으로 모델링 방법을 선택하고 진행할 것이다. 당신은 하이퍼파라미터를 변경할 것이고 모델을 재학습한다. 하지만 모델의 알고리즘을 바꾸지는 않고 어쩌면 너의 코드를 구동하기도 할 것이다.

<h4>Where this goes right</h4>
이것은 생각보다 멍청한 행동이 아니다. 대부분 우리는 기본적인 접근 외의 것이 필요하지 않다. 하나의 기술에 특화됨으로써, 발생할 수 있는 모든 이슈에 대해 전문가가 되는 것이 가능하다. 또한 이 접근은 당신을 당신의 코드에 특화되게 만들 것이고 이것은 변형과 최적화를 훨씬 쉽게 만들 것이다.

<h4>Where this goes wrong</h4>
만약 너가 가지고 있는 유일한 방법이 random-forest 형태의 망치라면, 모든 문제는 random-forest 형태의 못이어야 한다. 모델은 방법, 도구, 그 외 여러가지들에 의한 종속성이 존재한다. 이러한 사실은 이후에 모델을 변경하기 어렵게 만든다. 이것은 모델과 코드를 연결할 때 처음에 시도할 가능한 해법의 매우 구체적인 방법이다.

<h4>Recommendations</h4>
이것은 당신이 거대한 크기의 문제를 가지고 있고 또한 다수의 모델을 규칙적으로 test와 deploy하기에 매우 좋은 방식이다. 또한 이것은 당신이 간단한 문제를 가지고 있으며 최신 기술을 사용하고 싶지 않을 때도 좋다. 더 넓게 보자면, 나는 대부분의 applicatioin이 약간 다양한 모델링 방식들에도 대응 가능해야 하고, 그러한 경계들 안에 존재해야 한다고 생각한다.

<h2>Conclusion</h2>
데이터 사이언스는 상품화하기 어렵다. 그 이유는 많은 부분들이 현재 진행형이기 때문이다. AI 시스템에게 version이란 4가지 방향이 존재한다. 따라서 AI 시스템의 continuous delivery는 특히나 어렵다. 이 문제는 다뤄질 수 있고 다양한 방법에 각각 장점과 단점이 있다는 것을 알아야 한다.

<h1>내 의견</h1>

* ML은 모델, 데이터 값, 데이터 스키마, 코드 4가지의 방향에 따른 version control이 필요하다. 
* 모델을 코드로 다루는 것을 추천한다.
* 데이터를 스키마와 연결시켜서 저장하는 것은 추천하지 않는다.
* Data와 code를 연결지어 version control 할 수 있는 툴을 이용하자.
* Data, model을 code와 분리하려고 하지마라. 중복 작업이 많이 발생하고 통합 작업도 어렵다.
* Modeling 방법을 제한하여 변수를 줄인 상태에서 얻을 수 있는 정보를 최대한 얻고 다음 단계로 나아가는 것을 추천한다.
