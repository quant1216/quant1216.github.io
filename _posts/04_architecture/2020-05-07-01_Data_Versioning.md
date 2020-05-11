---
title: "Data Versioning"
source: https://emilygorcenski.com/post/data-versioning/
date: 2020-05-07 08:00:00
categories: architecture
---
<h1>Simple translation</h1>
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
