---
title: "Why_Can’t_I_just_Use_the_ROC_Curve?"
source: https://towardsdatascience.com/sampling-techniques-for-extremely-imbalanced-data-281cc01da0a8
date: 2020-05-22 18:00:00
categories: machine-learning
---
<h1>단순 번역 및 요약</h1>
데이터 불균형은 다수의 데이터에 대한 심각한 편향을 야기한다. 이는 성능을 저하시키고 FN(False Negative)를 증가시킨다. 이 문제를 어떻게 완화시킬 수 있을까? 가장 흔한 방법은 데이터 샘플링이다. 다수의 데이터에 대한 under-sampling, 소수의 데이터에 대해 over-sampling, 또는 이 두개의 혼합 버전이 있다. 이는 성능을 향상시킬 것이다. 이 글에서 나는 불균형 데이터가 무엇인지 설명하고, 왜 ROC가 정확한 측정을 할 수 없는지, 그리고 이를 해결할 방법을 다룰 것이다. 각 방법에 대한 python code도 첨부하였다.

<h3>What is imbalanced data?</h3>
Target field의 데이터의 비율이 다른 경우를 의미한다. 예를 들어 fraud detection을 한다고 할 경우 fraud는 매우 소수이기에 이러한 데이터가 imbalanced data가 되는 것이다.

<h3>Why a ROC curve cannot measure well?</h3>

* Labels
![](../../figs/05_machine-learning/2020-05-22-01_Why_Can’t_I_just_Use_the_ROC_Curve/fig1.png)

* Metics
![](../../figs/05_machine-learning/2020-05-22-01_Why_Can’t_I_just_Use_the_ROC_Curve/fig2.png)

* ROC Curve
![](../../figs/05_machine-learning/2020-05-22-01_Why_Can’t_I_just_Use_the_ROC_Curve/fig3.png)

중요한 것은 ROC curve에 사용되는 지표를 보면 negative labels의 갯수에 영향을 받지 않는다. TPR은 negative와는 관련이 없고 FPR은 negative labels의 갯수에 영향을 받지만 동일한 성능을 가진 모델에 대해서 FP와 TN은 비슷한 비율로 증가할 것이기에 값의 큰 변화는 없다. 즉 imbalanced data를 평가할 때 이는 좋은 지표가 되지 않는다. Davis와 goadrich는 편향된 데이터를 다룰 때 Precision-Recall curves가 ROC Curves보다 더 많은 정보를 제공할 것이라고 합니다. Precision은 imbalanced 데이터에 직접적인 영향을 받기 때문입니다. 만약 imbalanced data를 가지고 다른 두 개의 모델을 비교한다면, AUPRC가 AUROC보다 더 정확합니다.

<h3>What are the remedies?</h3>


<h3>Reference</h3>
* https://ftp.cs.wisc.edu/machine-learning/shavlik-group/davis.icml06.pdf

<h1>내 의견</h1>
먼저 중요하게 생각해야 할 것은 AUROC, AUPRC 모두 두 개의 모델의 성능을 비교하기 위한 지표이다. 이 때 imbalanced data에 대해서는 AUPRC를 사용해야 한다. 간단하게 예를 들어보면 쉬울 것 같다. 처음 balanced된 데이터로 TP/TN/FP/FN를 임의로 할당해보자. 이후 imbalanced data를 가정하기 위해 negative 데이터만 추가로 들어온다고 했을 때, FP와 TN은 점점 커질 것이다. 이 때 AUROC는 TPR(=TP/(TP+FN)), FPR(=FP/(TN+FP))로 구성되어 있기 때문에 negative 데이터가 추가로 들어와도 TPR과 FPR의 값이 동일하다. 즉 balance의 영향을 받지 않는다. 하지만 AUPRC는 precision(=TP/(TP+FP))와 recall(=TP/(TP+FN))로 구성되어 있기 때문에 동일한 모델의 성능이라면 skewed가 될수록 precision 값이 낮아지게 된다. 즉 balance에 영향을 받는다. 두 개의 모델을 만들었다면 imbalanced data에서 AUPRC가 더 높은 모델을 선택해야할 것이다.