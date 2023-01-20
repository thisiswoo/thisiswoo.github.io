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
> “예를 들어보자.”

- 롯데타워 시그니엘에 사는 집을 방문 할 땐 롯데시그니엘 측에 인증(출입증)과 인가(시그니엘 입주민)가 된 사람들만 출입 할 수 있는 것이 **인가**이다.
- 인증이(출입증) 되었더라도 인가(시그니엘 입주 기간)가 만료되었으면 출입이 불가능 하다. 
- **입증(인증)**된 신원의 **권한에 대한 허가**를 나타내는 것이 **인가**이다.
- **<span style="color:#ff8080">즉</span>**, **인증된 사용자에 대한 자원 접근 권환 확인**

# Web에서의 인증과 인가

![web_authentication_authorization](/assets/img/development/server/2023-01-18/web_authentication_authorization.png){:.centered width="90%"}

> “**Web**으로 예를 들어보자. 사용자가 게시판에 글 작성하는 서비스라 생각하자.”

- 사용자가 해당 사이트에 게시글을 작성하려고 하면 우선, **회원가입**과 **로그인**을 해야한다.
- 이 과정이 **인증**이다.
- 또한 **인증**이 되었으면 게시판에 게시글을 쓸 수 있는 **권한**이 생겼다.
- 이 과정이 **인가** 이다.
- 다른 사람이 게시판에 작성한 게시글 또한 읽을 수 있다.
- **<span style="color:#ff8080">단</span>**, 다른 사람이 작성한 게시글을 **수정** 할 수 없다.
- **<span style="color:#ff8080">왜냐하면</span>**, 다른 사람이 작성한 게시글을 수정 할 **<span style="color:#ff8080">권한(인가)</span>**이 없기 때문이다.
- **인가**가 적용된 개념이다.

# Request Header 활용

![web_authentication_authorization](/assets/img/development/server/2023-01-18/request_header.png){:.centered width="90%"}

> “Client와 Server 사이에 **HTTP**로 **통신**하게 된다.”

1. 해당 사이트 이미 회원 가입이 되어있어 DB에 ID,PW 정보가  있다.
2. 사용자가 사이트에 접속하려 하고