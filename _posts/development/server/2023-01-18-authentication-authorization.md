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

## 1.Request Header 활용

![web_authentication_authorization](/assets/img/development/server/2023-01-18/request_header.png){:.centered width="90%"}

> “Client와 Server 사이에 **HTTP**로 **통신**하게 된다. 가상의 사이트에 회원가입이 되어있는 상태라고 가정해보자.”

1. 해당 사이트 이미 회원 가입이 되어있어 DB에 ID,PW 정보가 있다.
2. 사용자가 login URL로 접근하게 되면 login 요청을 보낼 수 있게된다.
3. 만약, 로그인 API가 구축 되어있는 상태라고 했을때, DB에 ID, PW를 해당 URL 앞에 달아주고 요청하게 되면 login이 된다.

* 3번은 브라우저가 처리하게 되는데 처리 방식을 간단하게 알아보자.

![browser_encoding](/assets/img/development/server/2023-01-18/encoding.png){:.centered width="90%"}

* 브라우저 요청 처리 방법은 요청 URL을 Base64로 encoder를 이용하여 encoding 하게 된다.
1. URL의 **`user:1q2w3e!`** 부분을 parsing하여 encoding을 통해서 변환된 문자열을 같게된다.
2. 그다음 요청 헤더에 **Authorization에 넣어서 보내주는 개념**이다.

![request_header_login](/assets/img/development/server/2023-01-18/request_header_login.png){:.centered width="90%"}

1. Client가 encoding한 요청헤더를 Server에 요청하게 된다.
2. Server가 DB checking을 하여 DB에 값이 있으면 OK 사인을 주게 된다.

### Request Header만 활용할 시 문제점

- 어떤 처리를 할 때 **매번 인증**을 해야하는 번거러움이 발생하게 된다.
- 이걸 해결하기 위해 **Browser의 Storage**를 활용하게 된다.

## Browser 활용하기

- **Browser의 Storage** 
1. local storage
2. cookie storage
3. session storage

### cookie storage

1. cookie에 사용자의 ID, PW를 넣는다.
2. 사용자가 인증이 필요한 요청이 필요 할 때 cookie를 같이 넣어서 보내준다.

![request_header_login](/assets/img/development/server/2023-01-18/browser.png){:.centered width="90%"}


![request_header_login](/assets/img/development/server/2023-01-18/hacking.png){:.centered width="90%"}