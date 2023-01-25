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

- **(지문 또는 얼굴 등) 입증 가능한 정보**로 회사 또는 **인증이 필요한 곳에 입증하는 과정**
- **식별 가능한 정보로 서비스에 등록된 유저의 신원을 입증하는 과정**
- 인증을 통해 인증 허가된 곳(예) 회사 503호)에 출입하여 일도 하고, 회의도 하고, 다용도실에서 커피도 마시고 할 수 있다.
- **<span style="color:#ff8080">하지만</span>**, 인증이 허가된 곳만 가능하고, 인증이 허가되지 않은 다른 층이나 다른 사무실에 들어가지 못하게 된다.
- **<span style="color:#ff8080">이유는</span>**, 인증 허가된 곳(예) 회사 503호) 외에 다른 곳은 나의 정보로 인증 등록이 되어있지 않기 때문이다.

## 인가(Authorization)란?

![authorization](/assets/img/development/server/2023-01-18/authorization.png){:.centered width="90%"}
> “예를 들어보자.”

- 롯데타워 시그니엘에 사는 집을 방문할 땐 롯데 시그니엘 측에 인증(출입증)과 인가(시그니엘 입주민)가 된 사람들만 출입할 수 있는 것이 **인가**이다.
- 인증이(출입증) 되었더라도 인가(시그니엘 입주 기간)가 만료되었으면 출입이 불가능하다. 
- **입증(인증)** 된 신원의 **권한에 대한 허가**를 나타내는 것이 **인가**이다.
- **<span style="color:#ff8080">즉</span>**, **인증된 사용자에 대한 자원 접근 권환 확인**

# Web에서의 인증과 인가

![web_authentication_authorization](/assets/img/development/server/2023-01-18/web_authentication_authorization.png){:.centered width="90%"}

> “**Web**으로 예를 들어보자. 사용자가 게시판에 글 작성하는 서비스라 생각하자.”

- 사용자가 해당 사이트에 게시글을 작성하려고 하면 우선, **회원가입**과 **로그인**을 해야 한다.
- 이 과정이 **인증**이다.
- 또한 **인증**이 되었으면 게시판에 게시글을 쓸 수 있는 **권한**이 생겼다.
- 이 과정이 **인가**이다.
- 다른 사람이 게시판에 작성한 게시글 또한 읽을 수 있다.
- **<span style="color:#ff8080">단</span>**, 다른 사람이 작성한 게시글을 **수정**할 수 없다.
- **<span style="color:#ff8080">왜냐하면</span>**, 다른 사람이 작성한 게시글을 수정할 **<span style="color:#ff8080">권한(인가)</span>**이 없기 때문이다.
- **인가**가 적용된 개념이다.

## 1.Request Header 활용

![request_header](/assets/img/development/server/2023-01-18/request_header.png){:.centered width="90%"}

> “Client와 Server 사이에 **HTTP**로 **통신**하게 된다. 가상의 사이트에 회원가입이 되어있는 상태라고 가정해보자.”

1. 해당 사이트 이미 회원 가입이 되어있어 DB에 ID, PW 정보가 있다.
2. 사용자가 login URL로 접근하게 되면 login 요청을 보낼 수 있게 된다.
3. 만약, 로그인 API가 구축되어 있는 상태라고 했을 때, DB에 ID, PW를 해당 URL 앞에 달아주고 요청하게 되면 login이 된다.

* 3번은 브라우저가 처리하게 되는데 처리 방식을 간단하게 알아보자.

![browser_encoding](/assets/img/development/server/2023-01-18/encoding.png){:.centered width="90%"}

> “브라우저 요청 처리 방법은 요청 URL을 **Base64**로 **encoder**를 이용하여 **encoding** 하게 된다.”

1. URL의 **`user:1q2w3e!`** 부분을 **`parsing`** 하여 
2. **`encoding`**을 통해서 
3. 변환된 **`문자열`**을 같게된다.
4. 그다음 요청 헤더에 **Authorization에 넣어서 보내주는 개념**이다.

![request_header_login](/assets/img/development/server/2023-01-18/request_header_login.png){:.centered width="90%"}

> “**여기까지가 기본적으로 로그인한 상태의 로직**이다.”

1. Client가 encoding 한 요청 헤더를 Server에 요청하게 된다.
2. Server가 DB checking을 하고 
3. DB에 값이 있으면 OK 사인을 주게 된다.

### Request Header만 활용할 시 문제점

- 어떤 처리를 할 때(ex)등록, 수정, 삭제 등...) **매번 인증**을 해야하는 번거러움이 발생하게 된다.
- 이걸 해결하기 위해 **Browser의 Storage**를 활용하게 된다.

## Browser 활용하기

- **Browser의 Storage** 종류
1. Local Storage
2. Cookie Storage
3. Session Storage

### Cookie Storage

![cookie_storage](/assets/img/development/server/2023-01-18/browser.png){:.centered width="90%"}

> “**Cookie Storage의 로직**”

1. Cookie에 사용자의 ID, PW를 넣는고 인증이 필요한 요청을 할때 Cookie도 같이 Server에 보내준다.
2. Server는 DB에 쿠키에 담은 정보를 확인 요청하게 된다.
3. DB는 Server가 요청한 DB값이 있는지 확인 후 callback 해준다.
4. Server는 다시 client에 callback 해준다. 그리고 사용자는 원하는 자원을 얻을 수 있다.

- **사용자 입장에서는 굉장히 편리한 방법이다**.

![hacking](/assets/img/development/server/2023-01-18/hacking.png){:.centered width="90%"}

- **<span style="color:#ff8080">단</span>**, 해커의 입장에서도 굉장히 편리한 방법이다.
1. 보안에 취약하게 노출되어있어 **사용자 정보를 쉽게 탈취**할 수 있게 된다.
2. **Client**가 **Server**보다 **`상대적으로` 보안이 취약하다는 단점**이 있다.

* 이러한 두 가지 단점을 해결하기 위해(**보안 향상**) **Session을 활용**하여 **Server**에 도움을 요청하게 된다.

### Session Storage

![session_storage](/assets/img/development/server/2023-01-18/session_storage.png){:.centered width="90%"}

> “**Session Storage의 로직**. **`1번`** 부터 **`3번`**의 로직은 **`Cookie Storage`**와 같다.”

1. Cookie에 사용자의 ID, PW를 넣는고 인증이 필요한 요청을 할때 Cookie도 같이 Server에 보내준다.
2. Server는 DB에 쿠키에 담은 정보를 확인 요청하게 된다.
3. DB는 Server가 요청한 DB값이 있는지 확인 후 callback 해준다.
4. Session은 **인증된 사용자의 식별자**와, **랜덤한 문자열**로 **SESSIONID를 만들어**서
5. **응답(Response Header)로 넘겨주고**
6. **Client가 저장할 수 있도록 하는 것**이다.

#### 장점

1. Client의 보안이 낮은 데이터를 갖고 있게 되지 않게 된다. 그렇게 되면 해커가 정보를 가져가게 되더라도 크게 위험하지 않게 된다.
2. **`SESSION`**이 **만료기간**을 설정할 수 있어 해커가 가져가게 되더라도 기한이 만료되면 **유효하지 않게 되는 장점**이 있다. 
3. **Session의 관리**를 **Server 자체**에서 하고 있어 탈취된 Session을 Server에서 삭제하게 되면 더 이상 **Session을 사용하지 못하게** 된다.

#### 단점

![session_load_balancer](/assets/img/development/server/2023-01-18/session_load_balancer.png){:.centered width="90%"}

> “중간에 **`Load Balancer`**가 여러대의 서버에 요청하게 된다.”

1. 서비스가 잘 돼서 서버를 여러 개 두게 되면 **로드 밸런서**도 생기게 된다.
    - **`Load Balancer`** 란? : **서버에 가해지는 트래픽을 여러 대의 서버에서 균등하게 분산시켜주는 역할**.
2. 한 번 인증돼서 **`SESSIONID`**를 받게 되면 다음 요청 땐 **`Session`**으로만 이용해서 요청하게 된다.
3. 그 말은 이젠 DB까지 확인하지 않고 **Server에서 Session을 확인하여 처리**할 수 있게 됐다는 뜻이다. 

![session_load_balancer2](/assets/img/development/server/2023-01-18/session_load_balancer2.png){:.centered width="90%"}

> “이 상황에서 user가 두 번째 인증 요청을 보내게 된다.”

1. 1번째 인증을 완료하고 2번째 인증이 필요해 다시 요청을 보내게 된다.
2. 그런데, 로드 밸런서가 SESSIONID 값이 저장되어 있는 3번째 서버가 아닌 2번째 서버에 요청을 보내게 된다.
3. 2번째 서버에는 SESSIONID 값이 없어 오류가 발생하게 된다.
4. 이 문제의 원인은 Server 하나하나 자체에서 Session을 관리하고 있기 때문에 문제가 발생하게 된다.

#### 문제 해결 - 세션 DB

![session_storage_db](/assets/img/development/server/2023-01-18/session_storage_db.png){:.centered width="90%"}

> “**Session Storage(세션 DB)**를 통해 SESSIONID 요청을 **한 DB(Session Storage)**에 요청하여 해결. 그런데...”

#### 또 다른 문제 발생 - 세션 DB의 문제

![session_storage_problem](/assets/img/development/server/2023-01-18/session_storage_problem.png){:.centered width="90%"}

- Client가 많아져서 요청이 많아지게 되면 **Session Storage에 과부하가 생겨 DB가 터지게 된다**.
- 사용자를 위해 Cookie나 Browser의 힘을 빌려서 계속 로그인 까지 하게 해줬는데 이젠 보안상의 문제가 생겨 Server쪽에 와서 인증/인가를 해줬더니 문제가 계속 발생하게 된다.