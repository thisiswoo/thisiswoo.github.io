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
- 모든 파일과 프로그램은 `0`과 `1`의 나열이다.
- <span style="color:#6667AB">**`1`**</span>을 보낼 때는 <span style="color:#6667AB">**`+5V`**</span>의 전기를 전선으로 보낸다고 가정해보자.
- <span style="color:#ff8080">**`0`**</span>을 보낼 때는 <span style="color:#ff8080">**`-5V`**</span>의 전기를 전선으로 보낸다고 가정해보자.
- 이렇게 하면 <span style="color:#ff8080">**`0`**</span>과 <span style="color:#6667AB">**`1`**</span>의 전송이 가능할 것이다.
![Two Computer Communications](/assets/img/development/client/2023-03-02/2_computer_communications.png){:.centered width="100%"}
> “**두 대의 컴퓨터를 전선 하나로 연결한다고 가정**”
- <span style="color:#ff8080">**하지만**</span>, 위 방법은 문제가 있는데, 실제론 잘 작동하지 않는다.
- 그 이유는 바로 **아날로그 전송 방식**이기 때문이다.
![Sin Graph](/assets/img/development/client/2023-03-02/sin_graph.png){:.centered width="100%"}
> “**전자기파를 표현하는 Sin 함수 그래프**”
- 1초당 진동한 진동 횟수를 `Hz(헤르츠)`라고 하고, 위 예시는 1초에 4번의 사이클이기 때문에 `4Hz`가 된다.
- 하지만, 위 그림처럼 <span style="color:#ff8080">**일정하지 않은 주파수**</span>이면 어떻게 되는 것일까?


[//]: # (Continue with [[Network] OSI 7 Layer - Data Link Layer]&#40;./2023-03-05-week-8th.md&#41;{:.heading.flip-title})

[//]: # ({:.read-more})