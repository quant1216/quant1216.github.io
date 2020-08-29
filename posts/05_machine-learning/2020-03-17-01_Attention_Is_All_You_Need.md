---
title: "(Paper)Attention Is All You Need"
source: 
date: 2020-03-17 10:00:00
categories: machine-learning
---
<h1>요약</h1>
Bert Model의 기반이 되는 Transformer Model을 먼저 설명하고 추후 문서에서 Bert에 대해 다시 설명합니다.

<h3>Transformer Model</h3>

- 기존의 RNN, LSTM, GRU와 같은 sequential data를 다루는 기법의 문제점을 self-attention과 encoder-decoder를 사용하여 해결하는 모델

- 기존의 모델들의 문제점이란?
    - 해당 모델들은 recurrence를 통해서 sequential data를 인식하고 있음. 즉 ht = f(ht-1, xt) 와 같이 t-1 시점의 결과와 t 시점의 x를 함께 사용하여 t 시점의 결과를 예측하고 있음.

    - 첫 번째 문제점은, t 시점의 데이터를 알기 위해서 언제나 연속된 시간의 결과를 모두 학습해야 하지만 실제로 t 시점의 데이터는 t 이전의 시점 중 일부 또는 t 이후의 시점 중 일부가 필요할 뿐이다. 하지만 recurrence는 연속된 모든 데이터를 보기 때문에 불필요한 연산이 많이 요구된다.

    - 두 번째 문제점은, 연속된 데이터 계산 속에서 거리가 멀리 있는 단어의 경우 실질적으로는 그 단어가 중요하더라도 모델에 반영되는 중요도가 점차 희석된다. 이를 vanishing gradient 문제라고 하며 이를 보완하기 위해 RNN에서 LSTM이 고안되었지만 아직도 멀리 있는 단어는 충분히 고려할 수 없다는 문제점이 존재했다.

    - 세 번째 문제점은, parallel computation인데 sequential data를 사용할 경우 언제나 시간순서의 데이터를 모두 prediction 해야 하기 때문에 이에 dependency가 걸리고, 이는 parallel한 계산을 진행을 어렵게 하였다.

- 이를 해결하기 위한 모델이 transfomer 모델이고 해당 모델에 대한 설명을 self-attention, positional encoding, encoder-decoder 파트로 나누어 정리하였다.

<h3>Self-Attention(Attention)</h3>

- 이전 sequential 모델들에도 특정 시점의 데이터(ht*)에 weight(attention)를 주는 방식의 모델이 존재하였다. 하지만 이를 단어에 따른 서로 간의 attention을 구하는 self-attention 개념을 도입하였다.

- attention이란 예를 들어 나는 빨간 XX를 먹었어 에서 XX을 예측하고자 한다면 XX 이전의 나는 은 사실 큰 의미가 없고, 직전의 빨간 과 먹었어를 기준으로 사과가 가능성이 높다는 것을 알 수 있다. 즉 해당 데이터를 알기 위해 주위의 모든 단어를 알 필요가 없고 필요한 주위의 특정 단어에만 집중하면 된다는 것을 알 수 있고 이를 self-attention이라고 한다.

- 수식
    - figre
    - Q, K, V 는 하나의 word embedding에 대하여 query, key, value 로 나누어진 값을 의미한다.
        - query, key, value는 word embedding의 또 다른 관점에서의 embedding 형태라고 보면 된다.- 예시를 알기 쉽게 말하자면 사람의 몸이 있을 때, query=뼈, key=살, value=사람(뼈+살)으로 생각하면 쉽다.
        - 즉, 해당 뼈가 다른 살들과 얼마나 조화가 잘 되는지에 대한 score를 구하고 이 score를 value에 곱해주는 것으로 생각하면 된다.

    - Q, K가 유사할수록(해당 언어가 key word의 위치에 더 높은 값을 가지고 있을수록) 값이 커진다. 이 Q, K의 곱에 softmax를 적용하여 가치를 0-1의 weight 값으로 변경한 뒤, value를 곱하여 해당 word의 실질적인 가치를 평가한다.

    - 실제로는 하나의 word에 대하여 다른 모든 key-value pairs에 대해서 계산을 진행하기 때문에 이 과정에서 주위 단어와의 연결 관계를 정의할 수 있다. 아래 참고를 보면 그림으로 매우 설명을 잘 해놓았다.

    - 참고
        - https://towardsdatascience.com/illustrated-self-attention-2d627e33b20a

<h3>Multi-Head Attention</h3>

- 위의 attention에 대한 설명은 Scaled dot-product attention이고, 실질적으로 attention의 성능을 올리기 위해 Q, K, V에 원하는 수만큼 linear projection를 진행하여 각 성분을 분할하여 attention을 구한다. 즉 서로 다른 sub position에 신경쓰는 attention을 구하고 이를 합침으로써 더 높은 성능을 추구한다.
- figure2

<h3>Positional encoding</h3>

- Attention의 개념은 하나의 word와 다른 단어의 존재 자체에 연관성에 대해서 값을 찾지 단어의 순서에 대해서 신경을 쓰지 않는다.

- 이를 해결하기 위해 positional encoding을 도입한다.

- 수식


    - 위의 수식을 보면, 하나의 position word embedding이라도 embedding 각각에 다른 값을 넣어주는 것을 볼 수 있다. 현재 논문에서는 position을 trigonometry(삼각법)으로 상대적인 위치를 표현하는 것으로 보인다.

    -     - pos를 고정시키고 i의 영향을 고려해보자. 이는 일정 radian씩 증가하는 positional encoding을 만든다. 해당 encoding 값이 i에 따라 monotonic increasing 또는 monotonic decreasing 하지 않는 이유는 실질적으로 word embedding의 순서가 단어의 해석 순서와는 상관이 없으므로 해당 식을 사용함으로써 위치의 절대적인 표현 값을 나타내려는 것으로 고려된다.

    - i를 고정시키고 pos의 영향을 고려해보자. pos=1일 때를 기준으로 보면 pos=2는 pos=1일 때의 radian 변화 값보다 2배의 radian 값으로 변경된다는 것을 알 수 있다. 즉 radian 값의 차이로 위치의 상대적인 값을 표현하는 것으로 볼 수 있다.

    - 현재 positional encoding의 경우 sin, cos을 사용하기 때문에 언젠간 동일한 패턴이 반복되는 구간이 생길 것으로 보여지는데(2π의 배수와 접점이 생길 때) 실질적으로는 2π(무리수)와 pos/100002i/d_model(무리수) 이기 때문에 그러한 접점이 없거나, 실질적으로 저자가 고려하는 문자의 수 내에서는 이러한 문제가 발생하지 않기 때문에 가능한 것으로 여겨진다.

    - position 수식 결과 값(y axis=position, x axis=dimension)

    - 참고
        - https://timodenk.com/blog/linear-relationships-in-the-transformers-positional-encoding/

        - http://jalammar.github.io/illustrated-transformer/ -> positional embedding의 dimension에 따른 그림만 참고, 그 외 설명은 현재 오류가 있음.

<h3>Encoder-Decoder</h3>

- 전체 구조
    - figure04
    - 내부 구조

        - 위 그림의 왼쪽 부분을 Encoder라고 한다. 오른쪽 부분을 Decoder라고 한다. 현재 그림에서는 하나씩 묘사되어 있지만 실질적으로는 동일한 layer를 6번씩 통과하는 것으로 실질적인 순서는 아래와 같다.

        - Encoder, Decoder 각 내부를 보면 layer에 들어가기 전의 input 값을 결과에 Add&Norm 하는 과정인 residual connection이 존재하는데 이를 통하여 vanishing gradient 문제를 더 보완할 수 있다.

        - Attention을 지난 이후에는 Feed forward layer를 거치는데 이는 특별할 것이 없는 full-connected layer이다.

    - 세 가지 Attention의 차이점

        - Encoder의 Attention layer는 이전에 설명한 Attention의 개념을 그대로 사용하고 있다. 즉 해당 포지션의 단어에서 문장 내의 모든 단어에 대한 attention을 구한다.

        - Decoder의 Attention layer는 masked attention layer인데 이는 Decoder는 문장을 순서대로 출력해주어야 하기 때문에 문장 내의 모든 단어가 아니라 해당 포지션 이전의 단어에 대한 Attention 만을 학습하여 Decoding을 진행한다.

        - Encoder와 Decoder가 만나는 부분의 Attention layer가 매우 핵심인 layer인데, Encoder로부터는 K, V의 값을 받고 Decoder로부터는 Q의 값을 받아서 Attention을 학습한다. Decoder로부터 얻게된(현재 position의 단어 이전의 정보만을 고려한) Q와 Encoder를 통해서 얻게된(문장의 전체를 고려한) K, V를 섞음으로써 Decoding할 단어에게 전체 문장에서의 K, V 값을 부여하고 다시 전체적인 Attention을 학습한다.