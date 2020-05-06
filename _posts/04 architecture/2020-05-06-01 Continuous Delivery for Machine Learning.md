---
title: "Continuous Delivery for Machine Learning"
source: https://martinfowler.com/articles/cd4ml.html
date: 2020-05-06 08:00:00
categories: data engineering
---
<h2>Simple translation</h2>
머신러닝 개발 과정은 기존의 소프트웨어 과정보다 복잡하다. 코드 그 자체, 모델, 데이터 이 세 가지 방향으로의 변화가 있을 수 있다. 기존의 Continuous Delivery를 뛰어넘어 ML을 위한 배포 방법을 CD4ML(Continuous Delivery for Machine Learning)이라 칭하도록 하겠다.

```Continuous Delivery for Machine Learning (CD4ML) is a software engineering approach in which a cross-functional team produces machine learning applications based on code, data, and models in small and safe increments that can be reproduced and reliably released at any time, in short adaptation cycles.```

* Software engineering approach: 팀이 높은 품질의 소프트웨어를 만들게 한다.
* Cross-functional team: 서로 다른 팀이 자신의 강점을 사용하여 협업할 수 있게 한다.
* Producing software based on code, data, and machine learning models: ML을 위한 서로 다른 툴과 워크플로우는 각각 버저닝되고 관리되도록 한다.
* Small and safe increments: 조금씩 개선되는 코드는 코드를 읽기 쉽게 하고, 결과의 변동을 통제하게 하며, 안정성을 높인다.
* Reproducible and reliable software release: ML 모델은 non-deterministic하고 재생산되지 않을 수 있지만 배포 과정은 신뢰할 수 있고 재생산 가능하며 최대한 자동화되어야 한다.
* Software release at any time: 언제든 배포될 수 있도록 준비되어야 한다.
* Short adaptation cycles : 개발 cycle을 짧게 하여 빠른 피드백을 받을 수 있게 한다.

<H3>Discoverable and Accessible Data</H3>
데이터 사이언티스가 쉽게 접근하고 사용할 수 있도록 해야한다. 데이터셋의 버전 관리도 필요한데 이 방법은 Data Versioning 포스트를 보도록 해라.

<H3>Reproducible Model Training</H3>
거대한 input data와 output model을 source control platform에서 관리를 할 수는 없다. 또한 모델 학습 파이프라인은 끊임없이 변화하기 때문에 재생산이 매우 어렵다. 이러한 두 가지 문제를 보완하기 위해서  DVC(Data Science Version Control)를 도입하였다. 

DVC의 장점은 다음과 같다.
* source control repository의 바깥에 있는 외부 저장소에 데이터를 가져오고 저장하기 위한 다양한 플러그인이 지원된다.
* 파일들의 버전을 관리해준다. 최신 데이터가 변하는 경우가 생겨도 버전을 이용하여 재학습이 가능하다.
* 의존성 그래프(dependency graph)와 ML 파이프라인에서 사용된 명령어들을 추적함으로써, 다른 환경에서도 동일한 과정을 진행할 수 있도록 해준다.
* Git branch와 통합될 수 있다. 이는 다수의 실험이 동시에 가능하도록 한다.

![](../../figs/04 architecture/2020-05-06-01 Continuous Delivery for Machine Learning/fig1.png)

위의 파이프라인을 DVC 코드로 아래와 같이 표현된다.

![](../../figs/04 architecture/2020-05-06-01 Continuous Delivery for Machine Learning/fig2.png)

각 run은 대응되는 파일을 만들고 이것들은 version conttrol이 된다. `dvc repro` 명령문을 사용하면 다른 사람들도 동일한 파이프라인을 다시 실행할 수 있도록 한다. 또한  `dvc push`와 `dvc pull` 를 이용하여 외부 쓰토리지에 배포하거나 가져올 수 있다. 다른 오픈소스 툴로 존재하는데 Pachyderm과 MLflow Projects가 존재한다. 각 특징이 있지만 DVC는 간단한 CLI를 지원하기에 DVC를 선택하였다.

<H3>Model Serving</H3>
모델을 배포하는 몇 가지 패턴이 있다.

* Embedded model: model artifact를 의존성으로 바라본다. 즉 application 내에서 함께 built 되고 packaged 된다. application artifact와 version을 application code와 model의 조합으로 사용한다.
* Model deployed as a separate service: model이 독립적인 서비스로 사용된다. 이는 모델이 독립적이라는 장점이 있지만 inference에서 사용되는 latency와 remote invocation(원격 요청)이 필요하다.
* Model published as data: 해당 케이스도 model은 독립적으로 사용된다. 하지만 application은 모델을 런타임에 사용하는 데이터로 생각한다. 해당 케이스는 모델의 새로운 버전이 만들어질 때마다 이를 subscribe 하고, 이전 버전의 모델로 예측을 하면서 새로운 버전의 모델을 소화할 수 있는 스트리밍/실시간 시나리오에서 사용된다. 소프트웨어 release pattern은 Blue Green Deployment 또는 Canary Releases 패턴을 사용할 수 있다.

우리는 embedded model을 사용했다. python으로 model을 만들고 이를 serialized object(pickle file)로 만든다. 이는 DVC에 의해 저장소로 보내진다. application을 만들 때, 해당 object를 Docker 컨테이너에 넣은 뒤 배포를 진행한다. 이 경우 Docker image로 version을 달고 producttion에 배포된다.

이 외에도 다른 방법들이 존재한다. MLeap은 Spark, scikit-learn, Tensorflow 모델을 지원하는 serialization을 제공한다. 이는 또한  PMML, PFA, ONNX와 같은 language-agnostic(언어에 상관없는) serialization 포맷도 제공한다. 이는 Model published as data 패턴에도 사용될 수 있다.

H2O는 모델을 JAR java 라이브러리의 POJO로 모델을 내보낸다. 이 접근의 장점은 데이터 사이언티스트에게 친근한 언어로 모델을 학습할 수 있다는 것과 모델을 다른 JVM에서 구동되는 컴파일된 바이너리로 내보낼 수 있다는 것이다. 즉 JVM을 사용하는 더 좋은 환경에서 inference에 속도를 낼 수도 있다.

Model을 service로 배포하는 패턴은 많은 회사들이 MLaaS로 제공하고 있다. 예를 들면 Azure Machine Learning, AWS Sagemaker, Google AI Platform이 있다. 또 다른 옵션으로 kubeflow를 쓰는 방안도 있다. 이는 ML workflow를 Kubernetes에 배포하기 위해 사용된다.

MLflow Models은 모델을 패키징하는 표준화된 방법을 제공하기 위해 노력하고 있다. 이는 다운스트림에서 MLaaS 또는 embedded model 패턴에서 사용할 수 있다. 하지만 현재 이 방법론은 명확한 승자가 없고 계속 발전 중이기에 많은 평가가 필요하다.

<H3>Testing and Quality in Machine Learning</H3>
사용자와 모델 사이에는 데이터 형식이라는 숨겨진 계약이 있다. 만약 이 계약이 갑자기 바뀐다면 이는 integration issue를 만들고 application을 망가뜨릴 것이다. 이를 해결하기 위해 testing의 개념이 필요하다.