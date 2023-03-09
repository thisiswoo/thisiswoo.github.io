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
![Physical Layer](/assets/img/development/client/2023-03-02/physical_layer_main.png){:.centered width="100%"}

## 두 대의 컴퓨터가 통신하려면?
### 모든 파일과 프로그램은 0과 1의 나열
![Two Computer Communications](/assets/img/development/client/2023-03-02/2_computer_communications.png){:.centered width="100%"}
> “**두 대의 컴퓨터를 전선 하나로 연결한다고 가정**”

- 모든 파일과 프로그램은 0과 1의 나열이다.
- <span style="color:#6667AB">**`1`**</span>을 보낼 때는 <span style="color:#6667AB">**`+5V`**</span>의 전기를, <span style="color:#ff8080">**`0`**</span>을 보낼 때는 <span style="color:#ff8080">**`-5V`**</span>의 전기를 전선으로 보낸다고 가정해 보자.
- 이렇게 하면 <span style="color:#ff8080">**`0`**</span>과 <span style="color:#6667AB">**`1`**</span>의 전송이 가능할 것이다.
- 하지만, 위 방법은 문제가 있는데, 실제론 잘 작동하지 않는다.

### 실제론 잘 작동하지 않는 문제 발생
![Sin Graph](/assets/img/development/client/2023-03-02/sin_graph.png){:.centered width="100%"}
> “**전자기파를 표현하는 Sin 함수 그래프**”

- **주파수란?** 1초당 진동한 진동 횟수. 단위 : Hz(헤르츠)
- 1초당 진동한 진동 횟수를 `Hz(헤르츠)`라고 하고, 위 예시는 1초에 4번의 사이클이기 때문에 `4Hz(헤르츠)`가 된다. 
- 위 그래프는 파동이 진행되는 내내 주파수가 `4Hz(헤르츠)`이다.

### 만약, 일정하지 않은 주파수이면?
![Irregular Sin Graph](/assets/img/development/client/2023-03-02/irregular_sin_graph.png){:.centered width="100%"}
> “**이런 전자기파는 주파수 값이 숫자 하나로 고정되지 않고, 파동이 진행되는 동안 주파수 값이 계속 변하게 된다.**”

- 하지만, 위 그림처럼 <span style="color:#ff8080">**일정하지 않은 주파수**</span>이면 어떻게 되는 것일까?
- 위 전자기파의 주파수 최솟값이 `1Hz`, 최댓값이 `10Hz`라고 생각해 보자.
- 그런데, 모든 전선에는 저항이 있어 전선에 맞는 허용 범위의 전류와 전압이 있다. 
- 즉, 전선은 모든 전류, 그리고 모든 주파수를 다 통과시키지 못 하고 허용 범위가 생긴다.

### 모든 전선에는 저항이 있어 전선에 맞는 허용 범위의 전류와 전압이 존재
  ![Electric Wire](/assets/img/development/client/2023-03-02/electric_wire.png){:.centered width="100%"}
> “**KS C IEC - IEC(국제 전기 표준화 기구)를 KS(한국 산업 표준)에 맞게 표준화**”<br>
> “**2CX 2.5SQ MM - 2core(2가닥의 전선)와 각 전선의 굵기는 2.5SQ (MM) 2.5mm**”<br>
> “**300/500V - 허용 범위의 전압(Volt), 국내 가정용 220V 표준**”<br>
> “**KS C IEC 53 - 케이블의 종류(해당 케이블은 VCTF)**”

- 우리나라의 허용범위 주파수는 `60Hz`이다.
- 이유는 [우리나라 주파수는 왜 60Hz일까?](http://www.keaj.kr/news/articleView.html?idxno=3608){:target="_blank"} 자료를 참고해 주시길 바란다.

### 잘못된 데이터 수신
![Problem](/assets/img/development/client/2023-03-02/problem_1.png){:.centered width="100%"}
> “**Hz의 허용 범위가 다른 전선의 사용한 경우 잘못된 데이터를 수신할 수 있게 되는 문제점 발생**”<br>

- 보내는 쪽에서는 `1~10Hz`의 데이터 `안녕`을 보낸다.
- 하지만, 전선의 주파수 허용 범위가 `5~8Hz`여서 `1~4Hz`, `9~10Hz`의 데이터가 손상이 된다.
- 그래서 받은 데이터는 전혀 다른 데이터를 받게 되는 문제가 발생한다.
- 그런데, 앞에서 두 대의 컴퓨터가 통신하려면 `0`과 `1`을 주고받을 수 있으면 된다고 했는데, 그 방법은 두 대의 컴퓨터에 다음과 같은 전자기파를 주고받을 수 있으면 된다.

### 수직/수평선이 있는 전자기파
![Vertical Horizontal_Wave](/assets/img/development/client/2023-03-02/vertical_horizontal_wave.png){:.centered width="100%"}
> “**그런데, 수직선과 수평선이 있는 전자기파는 항상 0~무한대Hz의 주파수 범위를 갖는다???**”<br><br>
> “**수직선과 수평선이 있는 전자기파는 편파(polarized) 전자기파이며, 편파전자기파는 진폭이 한 방향으로 고정되어 있고, 이러한 편파 전자기파는 하나의 평면에서 전파가 진행된다.**”<br>
> “**따라서, 수직선과 수평선이 있는 전자기파의 주파수 범위는 항상 0 ~ 무한대(Hz)를 갖는다. 왜냐하면 편파 전자기파는 진폭이 한 방향으로 고정되어 있으며, 주파수는 전파의 진동수를 나타내기 때문이다.**”<br><br>
> “**따라서, 수직선과 수평선이 있는 전자기파는 특정한 주파수 범위에서만 발생하는 것이 아니라, 전체 주파수 범위에서 발생할 수 있다.**”<br>

- 따라서, 이러한 전기신로를 통과시킬 수 있는 전선은 없다.
- 그렇다면 `0`과 `1`의 신호를 어떻게 전송해야 할까?

### 디지털 신호를 아날로그 신호로
![Digital to Analog](/assets/img/development/client/2023-03-02/digital_to_analog.png){:.centered width="100%"}
> “**디지털 신호를 아날로그 신호로 바꿔서 전송해야 한다.**”<br>

#### 아날로그 신호의 특징
- 아날로그 신호는 끊김없이 연속된 신호로 이루어져있다.
- 신호의 세기를 아주 정밀하게 표현이 가능하다.
- 거리에 따라 신호의 세기가 약해지면서 결국 소멸되는 특징을 가지고 있다.

#### 디지털 신호의 특징
- 디지털 신호는 `1`로 표현하고 싶을 땐 특정 전압 이상을 가하고, `0`으로 표현하고 싶을 땐 특정 전압의 이하로 가해 표현한다.
- 즉, `1`을 `5V`라고 가정할 때 `1`로 표현하고 싶으면 `5V` 이상의 전압을 가하고, `0`으로 표현하고 싶을 땐 `5V` 이하의 전압을 가하게 된다.
- 디지털 신호 그래프의 `0`은 끊어진 것처럼 보이지만 사실 끊어진 게 아니라 특정 전압 이하이기 때문에 `0`으로 그래프상 표시한 것 뿐이다. 실제론 계속 전류는 흐른다.


[//]: # (Continue with [[Network] OSI 7 Layer - Data Link Layer]&#40;./2023-03-05-week-8th.md&#41;{:.heading.flip-title})

[//]: # ({:.read-more})