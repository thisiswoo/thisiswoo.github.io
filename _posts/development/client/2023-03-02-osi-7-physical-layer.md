---
layout: post
title: "[Network] OSI 7 Layer - Physical Layer"
subtitle: "Physical Layer"
category: development
tags: client network osi7layer
image:
    path: /assets/img/development/client/2023-03-02/osi_7_layer_physical_layer_cover.png
---

> “해당 포스팅은 **우아한Tech**의 [**[10분 테코톡] 🔮 히히의 OSI 7 Layer**](https://www.youtube.com/watch?v=1pfTxp25MA8&t=35s){:target="_blank"}을 참고하여 포스팅하였습니다. 영상을 제작해주신 **우아한Tech**와 **히히**님에게 감사드립니다.”

<span style="font-size:30px;">\#**목차**</span>

* this unordered seed list will be replaced by the toc
{:toc}

# Physical Layer
![Physical Layer](/assets/img/development/client/2023-03-02/physical_layer_main.png){:.centered width="80%"}

## 두 대의 컴퓨터가 통신하려면?
### 모든 파일과 프로그램은 0과 1의 나열
![Two Computer Communications](/assets/img/development/client/2023-03-02/2_computer_communications.png){:.centered width="80%"}
> “**두 대의 컴퓨터를 전선 하나로 연결한다고 가정**”

- 모든 파일과 프로그램은 0과 1의 나열이다.
- <span style="color:#6667AB">**`1`**</span>을 보낼 때는 <span style="color:#6667AB">**`+5V`**</span>의 전기를, <span style="color:#ff8080">**`0`**</span>을 보낼 때는 <span style="color:#ff8080">**`-5V`**</span>의 전기를 전선으로 보낸다고 가정해 보자.
- 이렇게 하면 <span style="color:#ff8080">**`0`**</span>과 <span style="color:#6667AB">**`1`**</span>의 전송이 가능할 것이다.
- 하지만, 위 방법은 문제가 있는데, 실제론 잘 작동하지 않는다.

### 실제론 잘 작동하지 않는 문제 발생
![Sin Graph](/assets/img/development/client/2023-03-02/sin_graph.png){:.centered width="80%"}
> “**전자기파를 표현하는 Sin 함수 그래프**”

- **주파수란?** 1초당 진동한 진동 횟수. 단위 : Hz(헤르츠)
- 1초당 진동한 진동 횟수를 `Hz(헤르츠)`라고 하고, 위 예시는 1초에 4번의 사이클이기 때문에 `4Hz(헤르츠)`가 된다. 
- 위 그래프는 파동이 진행되는 내내 주파수가 `4Hz(헤르츠)`이다.

### 만약, 일정하지 않은 주파수이면?
![Irregular Sin Graph](/assets/img/development/client/2023-03-02/irregular_sin_graph.png){:.centered width="80%"}
> “**이런 전자기파는 주파수 값이 숫자 하나로 고정되지 않고, 파동이 진행되는 동안 주파수 값이 계속 변하게 된다.**”

- 하지만, 위 그림처럼 <span style="color:#ff8080">**일정하지 않은 주파수**</span>이면 어떻게 되는 것일까?
- 위 전자기파의 주파수 최솟값이 `1Hz`, 최댓값이 `10Hz`라고 생각해 보자.
- 그런데, 모든 전선에는 저항이 있어 전선에 맞는 허용 범위의 전류와 전압이 있다. 
- 즉, 전선은 모든 전류, 그리고 모든 주파수를 다 통과시키지 못 하고 허용 범위가 생긴다.

### 모든 전선에는 저항이 있어 전선에 맞는 허용 범위의 전류와 전압이 존재
  ![Electric Wire](/assets/img/development/client/2023-03-02/electric_wire.png){:.centered width="80%"}
> “**KS C IEC - IEC(국제 전기 표준화 기구)를 KS(한국 산업 표준)에 맞게 표준화**”<br>
> “**2CX 2.5SQ MM - 2core(2가닥의 전선)와 각 전선의 굵기는 2.5SQ (MM) 2.5mm**”<br>
> “**300/500V - 허용 범위의 전압(Volt), 국내 가정용 220V 표준**”<br>
> “**KS C IEC 53 - 케이블의 종류(해당 케이블은 VCTF)**”

- 우리나라의 허용범위 주파수는 `60Hz`이다.
- 이유는 [우리나라 주파수는 왜 60Hz일까?](http://www.keaj.kr/news/articleView.html?idxno=3608){:target="_blank"} 자료를 참고해 주시길 바란다.

### 잘못된 데이터 수신
![Problem](/assets/img/development/client/2023-03-02/problem_1.png){:.centered width="80%"}
> “**Hz의 허용 범위가 다른 전선의 사용한 경우 잘못된 데이터를 수신할 수 있게 되는 문제점 발생**”<br>

- 보내는 쪽에서는 `1~10Hz`의 데이터 `안녕`을 보낸다.
- 하지만, 전선의 주파수 허용 범위가 `5~8Hz`여서 `1~4Hz`, `9~10Hz`의 데이터가 손상이 된다.
- 그래서 받은 데이터는 전혀 다른 데이터를 받게 되는 문제가 발생한다.
- 그런데, 앞에서 두 대의 컴퓨터가 통신하려면 `0`과 `1`을 주고받을 수 있으면 된다고 했는데, 그 방법은 두 대의 컴퓨터에 다음과 같은 전자기파를 주고받을 수 있으면 된다.

### 수직/수평선이 있는 전자기파
![Vertical Horizontal_Wave](/assets/img/development/client/2023-03-02/vertical_horizontal_wave.png){:.centered width="80%"}
> “**그런데, 수직선과 수평선이 있는 전자기파는 항상 0~무한대Hz의 주파수 범위를 갖는다???**”<br><br>
> “**수직선과 수평선이 있는 전자기파는 편파(polarized) 전자기파이며, 편파전자기파는 진폭이 한 방향으로 고정되어 있고, 이러한 편파 전자기파는 하나의 평면에서 전파가 진행된다.**”<br>
> “**따라서, 수직선과 수평선이 있는 전자기파의 주파수 범위는 항상 0 ~ 무한대(Hz)를 갖는다. 왜냐하면 편파 전자기파는 진폭이 한 방향으로 고정되어 있으며, 주파수는 전파의 진동수를 나타내기 때문이다.**”<br><br>
> “**따라서, 수직선과 수평선이 있는 전자기파는 특정한 주파수 범위에서만 발생하는 것이 아니라, 전체 주파수 범위에서 발생할 수 있다.**”<br>

- 따라서, 이러한 전기신로를 통과시킬 수 있는 전선은 없다.
- 그렇다면 `0`과 `1`의 신호를 어떻게 전송해야 할까?

### 디지털 신호를 아날로그 신호로
![Digital to Analog](/assets/img/development/client/2023-03-02/digital_to_analog.png){:.centered width="80%"}
> “**디지털 신호를 아날로그 신호로 바꿔서 전송해야 한다.**”<br>

#### 아날로그 신호의 특징
- 아날로그 신호는 끊김없이 연속된 신호로 이루어져있다.
- 신호의 세기를 아주 정밀하게 표현이 가능하다.
- 거리에 따라 신호의 세기가 약해지면서 결국 소멸되는 특징을 가지고 있다.

#### 디지털 신호의 특징
- 디지털 신호는 `1`로 표현하고 싶을 땐 특정 전압 이상을 가하고, `0`으로 표현하고 싶을 땐 특정 전압의 이하로 가해 표현한다.
- 즉, `1`을 `5V`라고 가정할 때 `1`로 표현하고 싶으면 `5V` 이상의 전압을 가하고, `0`으로 표현하고 싶을 땐 `5V` 이하의 전압을 가하게 된다.
- 디지털 신호 그래프의 `0`은 끊어진 것처럼 보이지만 사실 끊어진 게 아니라 특정 전압 이하이기 때문에 `0`으로 그래프상 표시한 것 뿐이다. 실제론 계속 전류는 흐른다.

![Data Collection Graph](/assets/img/development/client/2023-03-02/data_collection_graph.png){:.centered width="80%"}
- 위 그래프는 `1초` 동안 발생한 아날로그 신호이다.
- `200ms` 마다 데이터를 수집한다고 가정해보자. 참고로 `200ms`는 `1/5(0.2sec)`초 이다. 
- 즉, `1초`에 `5번` 아날로그 신호의 데이터를 분석해서 `digital`신호로 바꾸는 작업이다.
- `200ms`는 기존에 아날로그 신호와 외곡 차이가 심하게 난다.
- `100ms`는 `200ms`보다 조금 더 아날로그 신호에 가깝다. 
- `50ms(1초에 20번)` 데이터를 수집하게 되면 `200ms`보다 `4배` 정도 정밀도가 높아진다.
- 즉, 조금 더 촘촘히 많은 데이터를 수집할 수 록 `아날로그` 데이터에 굉장히 가까워 진다.
- 아무리 `아날로그 신호`를 `디지털 신호`로 바꾸어도 어쩔 수 없이 데이터 외곡은 생기기 마련이다.

#### 가정에서 많이 사용하는 CAT5.E Cable
![CAT.5E Cable](/assets/img/development/client/2023-03-02/cat5e_cable.png){:.centered width="80%"}

#### CAT.5E vs CAT.6 차이
![CAT.5E vs CAT.6 Cable](/assets/img/development/client/2023-03-02/cat5e_vs_cat6.png){:.centered width="80%"}
- 1초에 `100MHz`의 데이터 전송이 가능한 케이블이다.
- 위에서 예시로 든 `50ms(1초에 20번)`와 1초에 `100MHz(1초에 1,000,000번 - 100만 번)`의 데이터 수집량에 엄청난 차이를 느낄 수 있다.
    - 참고로, **“현재 통신사에서1기가 인터넷을 최대 속도로 서비스하고 있는 상황에서 케이블의 종류에 따른 속도의 차이는 없다”**
    - 다만, 차후 `2.5G`나 `5G`의 속도가 상용화 되면 지금의 `1Gbps`보다 빠른 속도를 제공받을 수 있다는 이야기.

## PHY 칩
![Phy 칩](/assets/img/development/client/2023-03-02/phy_chip.png){:.centered width="80%"}
- `Physical Layer`는 하드웨어적으로 구현되어 있다.
- `PHY 칩은`이 `아날로그 신호`를 `디지털 신호(decoding)`로,
- `디지털 신호`를 `아날로그 신호(encoding)`로 변환해주는 하드웨어이다.

## 그래서 결국 Physical Layer란?
- 물리적인 전선을 통해 `0`과 `1`의 나열된 신호를 통신하는 계층이다.
- `0`과 `1`의 나열된 `디지털 신호`를 `아날로그 신호`로 바꾸어 전선에 흘려 보낸다. -> **`encoding`**
- `아날로그 신호`가 들어오면 `0`과 `1`의 나열로 해석한다 -> **`decoding`**
- 물리적으로 연결된 두 대의 컴퓨터가 `0`과 `1`의 나열을 주고받을 수 있게 해주는 모듈(**`module`**)

## Reference
- [[10분 테코톡] 🔮 히히의 OSI 7 Layer](https://www.youtube.com/watch?v=1pfTxp25MA8&t=35s){:target="_blank"}
- [시골사는개발자 - 아날로그 데이터를 디지털 데이터로 변환하는 기본 원리](https://www.youtube.com/watch?v=bSbW6xqHxc0){:target="_blank"}
- [인터넷119 - CAT.5E와 CAT.6 UTP케이블의 차이](https://m.blog.naver.com/internet_119/221592301990){:target="_blank"}

[//]: # (Continue with [[Network] OSI 7 Layer - Data Link Layer]&#40;./2023-03-05-week-8th.md&#41;{:.heading.flip-title})

[//]: # ({:.read-more})