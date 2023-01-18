---
layout: post
title: "[Java] 인증(Authentication)과 인가(Authorization)"
subtitle: "인증과 인가"
category: development
tags: server 인증 Authentication 인가 Authorization
image:
  path: /assets/img/development/server/2023-01-18/authentication_authorization.png
---

> “해당 포스팅은 **우아한Tech**의 [**[10분 테코톡] 🎡토니의 인증과 인가**](https://www.youtube.com/watch?v=y0xMXlOAfss)의 영상을 기반으로 포스팅하였습니다. 또한 설명이 너무 잘되어었어 복습하고자 글로 남깁니다. 영상을 제작해주신 **우아한Tech**와 **토니**님에게 감사드립니다.”

<span style="font-size:30px;">\#**목차**</span>

* this unordered seed list will be replaced by the toc
{:toc}

# 실 생활에서의 인증과 인가

## 인증(Authentication)이란?

![authentication](/assets/img/development/server/2023-01-18/authentication.png){:.centered width="90%"}

- **(지문 또는 얼굴 등)입증 가능한 정보**로 회사 또는 **인증이 필요한 곳에 입증하는 과정**
- **식별 가능한 정보로 서비스에 등록된 유저의 신원을 입증하는 과정**
- 인증을 통해 인증 허가된 곳(예) 회사 503호)에 출입하여 일도 하고, 회의도 하고, 다용도실에서 커피도 마시고 할 수 있다.
- **<span style="color:#ff8080">하지만</span>**, 인증이 허가된 곳만 가능하고, 인증이 허가 되지 않은 다른 층이나 다른 사무실에 들어가지 못하게 된다.
- **<span style="color:#ff8080">이유는</span>**, 인증 허가된 곳(예) 회사 503호)외에 다른 곳은 나의 정보로 인증 등록이 되어있지 않기 때문이다.

## 인가(Authorization)란?

![authorization](/assets/img/development/server/2023-01-18/authorization.png){:.centered width="90%"}

- **입증(인증)**된 신원의 **권한에 대한 허가**를 나타내는 것이 **인가**이다.
- **<span style="color:#ff8080">즉</span>**, **인증된 사용자에 대한 자원 접근 권환 확인**

# Web에서의 인증과 인가

![web_authentication_authorization](/assets/img/development/server/2023-01-18/web_authentication_authorization.png){:.centered width="90%"}