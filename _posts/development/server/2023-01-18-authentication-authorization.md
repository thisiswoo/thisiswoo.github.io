---
layout: post
title: "[Security] 인증과 인가"
subtitle: "인증과 인가"
category: development
tags: server 인증 인가
image:
  path: /assets/img/development/server/2023-01-18/authentication_authorization_cover.png
---

> “해당 포스팅은 **우아한Tech**의 [**[10분 테코톡] 🎡토니의 인증과 인가**](https://www.youtube.com/watch?v=y0xMXlOAfss)의 영상을 기반으로 포스팅하였습니다. 또한 설명이 너무 잘되어었어 복습하고자 글로 남깁니다. 영상을 제작해주신 **우아한Tech**와 **토니**님에게 감사드립니다.”

<span style="font-size:30px;">\#**목차**</span>

* this unordered seed list will be replaced by the toc
{:toc}

# 실 생활에서의 인증과 인가

## 인증(Authentication)이란?

![authentication](/assets/img/development/server/2023-01-18/authentication.png){:.centered width="90%"}

- **(지문 또는 얼굴 등) 입증 가능한 정보**로 인증이 필요한 곳에 <span style="color:#ff8080">**입증하는 과정**</span>이다. 즉, **식별 가능한 정보로 서비스에 등록된 유저의 신원을 입증하는 과정**
- 쉽게 말해, **아이폰**으로 **계좌이체**를 하려고 할 때, 어플에 접근하여 **로그인 하는 과정**이 <span style="color:#ff8080">**인증**</span>이다.

## 인가(Authorization)란?

![authorization](/assets/img/development/server/2023-01-18/authorization.png){:.centered width="90%"}

- **입증(인증)** 된 신원의 **권한에 대한 허가**를 나타내는 것이 <span style="color:#ff8080">**인가**</span>이다. 즉, **인증된 사용자에 대한 자원 접근 권환 확인**이다.
- 쉽게 말해, 은행 어플에 접근하여 **로그인(인증)** 한 후, 계좌이체에서 마지막 송금 과정시 **OTP, 생체인증** 등 **권한을 통한 작업**이 <span style="color:#ff8080">**인가**</span>이다.


# Web에서의 인증과 인가

![web_authentication_authorization](/assets/img/development/server/2023-01-18/web_authentication_authorization.png){:.centered width="90%"}
> “**Web**에서 사용자가 게시판에 글 작성하는 서비스라고 생각해보자.”

- 사용자가 사이트에 **회원가입**과 **로그인**을 하는 과정이 <span style="color:#ff8080">**인증**</span>이다.
- **인증**이 됐으면 게시판에 게시글을 쓸 수 있는 **권한**이 생겼다. 이 과정이 <span style="color:#ff8080">**인가**</span>이다. 다른 사람이 게시판에 작성한 게시글 또한 읽을 수 있다.
- 하지만, 다른 사람이 작성한 게시글을 **수정**할 수 없다. 왜냐하면, 다른 사람이 작성한 게시글을 수정 할 <span style="color:#ff8080">**권한(인가)**</span>이 없기 때문이다. <span style="color:#ff8080">**인가**</span>가 **적용된 개념**이다.

## 1.Request Header 활용

![request_header](/assets/img/development/server/2023-01-18/request_header.png){:.centered width="90%"}
> “Client와 Server 사이에 <span style="color:#ff8080">**HTTP**</span>로 **통신**하게 된다. 가상의 사이트에 회원가입이 되어있는 상태라고 **가정**해보자.”

1. DB에 ID, PW 정보가 있다. 사용자가 login URL로 접근하게 되면 login 요청을 보낼 수 있게 된다.
2. 만약, 로그인 API가 구축되어 있는 상태라고 **가정** 했을 때, URL 앞에 ID, PW를 넣어주고 요청하게 되면 login이 된다.

* **2번의 처리 과정을 간단하게 알아보자**.

![browser_encoding](/assets/img/development/server/2023-01-18/encoding.png){:.centered width="90%"}
> “브라우저 요청 처리 방법은 요청 URL을 **Base64**로 **encoder**를 이용하여 **encoding** 하게 된다.”

1. URL의 **`user:1q2w3e!`** 부분을 <span style="color:#ff8080">**`parsing`**</span> 하여 
2. **`Base64`**를 통해 <span style="color:#ff8080">**`encoding`**</span>을 하고 
3. 그렇게 변환된 <span style="color:#ff8080">**`문자열`**</span>을 같게되고
4. **요청 헤더**의 <span style="color:#ff8080">**`Authorization`**</span>에 넣어서 보내주는 **개념**이다.

![request_header_login](/assets/img/development/server/2023-01-18/request_header_login.png){:.centered width="90%"}
> “**여기까지가 기본적으로 로그인한 상태의 로직**이다.”

1. Client가 encoding 한 요청 헤더를 Server에 요청하게 된다.
2. Server가 DB checking을 하고 
3. DB에 값이 있으면 Client에 <span style="color:#ff8080">**`OK`**</span> 사인을 주게 된다.

### Request Header만 활용할 시 문제점

- 어떤 처리를 할 때(ex) 등록, 수정, 삭제 등...) **매번 인증**을 해야 하는 번거로움이 발생하게 된다.
- 이걸 해결하기 위해 **Browser의 Storage**를 활용하게 된다.

## Browser 활용하기

- **Browser의 Storage** 종류
1. Local Storage
2. Cookie Storage
3. Session Storage

### Cookie Storage

![cookie_storage](/assets/img/development/server/2023-01-18/browser.png){:.centered width="90%"}
> “**Cookie Storage의 로직**”

1. Cookie에 사용자의 ID, PW를 넣는고 인증이 필요한 요청을 할 때 Cookie도 같이 Server에 보내준다.
2. Server는 DB에 쿠키에 담은 정보를 확인 요청하게 된다.
3. DB는 Server가 요청한 DB 값이 있는지 확인 후 callback 해준다.
4. Server는 다시 client에 callback 해준다. 그리고 사용자는 원하는 자원을 얻을 수 있다.

- **사용자 입장에서는 굉장히 편리한 방법이다**.

![hacking](/assets/img/development/server/2023-01-18/hacking.png){:.centered width="90%"}

- <span style="color:#ff8080">**`하지만`**</span>, 해커의 입장에서도 굉장히 편리한 방법이다.
1. 보안에 취약하게 노출되어 있어 **사용자 정보를 쉽게 탈취**할 수 있게 된다.
2. **Client**가 **Server**보다 **`상대적으로` 보안이 취약하다는 단점**이 있다.

* 이러한 두 가지 단점을 해결하기 위해(**보안 향상**) **Session을 활용**하여 **Server**에 도움을 요청하게 된다.

### Session Storage

![session_storage](/assets/img/development/server/2023-01-18/session_storage.png){:.centered width="90%"}
> “**Session Storage의 로직**. **`1번`**부터 **`3번`**의 로직은 **`Cookie Storage`**와 같다.”

1. Cookie에 사용자의 ID, PW를 넣는고 인증이 필요한 요청을 할 때 Cookie도 같이 Server에 보내준다.
2. Server는 DB에 쿠키에 담은 정보를 확인 요청하게 된다.
3. DB는 Server가 요청한 DB 값이 있는지 확인 후 callback 해준다.
4. Session은 **인증된 사용자의 식별자**와, **랜덤 한 문자열**로 <span style="color:#ff8080">**`SESSIONID를`**</span>를 만들어서
5. **응답(Response Header)로 넘겨주고**
6. **Client가 저장할 수 있도록 하는 것**이다.

#### 장점

1. Client의 보안이 낮은 데이터를 갖고 있게 되지 않게 된다. 그렇게 되면 해커가 정보를 가져가게 되더라도 크게 위험하지 않게 된다.
2. **SESSION**이 <span style="color:#ff8080">**`만료기간`**</span>을 설정할 수 있어 해커가 가져가게 되더라도 기한이 만료되면 **유효하지 않게 되는 장점**이 있다. 
3. **Session의 관리**를 **Server 자체**에서 하고 있어 탈취된 Session을 Server에서 삭제하게 되면 더 이상 **Session을 사용하지 못하게** 된다.

#### 단점

![session_load_balancer](/assets/img/development/server/2023-01-18/session_load_balancer.png){:.centered width="90%"}
> “중간에 **`Load Balancer`**가 여러 대의 서버에 요청하게 된다.”

1. 서비스가 잘 돼서 서버를 여러 개 두게 되면 **로드 밸런서**도 생기게 된다.
    - **`Load Balancer`** 란? : **서버에 가해지는 트래픽을 여러 대의 서버에서 균등하게 분산시켜주는 역할**.
2. 한 번 인증돼서 **`SESSIONID`**를 받게 되면 다음 요청 땐 **`Session`**으로만 이용해서 요청하게 된다.
3. 그 말은 이젠 DB까지 확인하지 않고 **Server에서 Session을 확인하여 처리**할 수 있게 됐다는 뜻이다. 

![session_load_balancer2](/assets/img/development/server/2023-01-18/session_load_balancer2.png){:.centered width="90%"}
> “이 상황에서 user가 두 번째 인증 요청을 보내게 된다.”

1. 1번째 인증을 완료하고 2번째 인증이 필요해 다시 요청을 보내게 된다.
2. 그런데, 로드 밸런서가 SESSIONID 값이 저장되어 있는 3번째 서버가 아닌 2번째 서버에 요청을 보내게 된다.
3. 2번째 서버에는 SESSIONID 값이 없어 오류가 발생하게 된다.
4. 이 문제의 원인은 **Server 하나하나 자체에서 Session을 관리**하고 있기 때문에 <span style="color:#ff8080">**`문제가 발생`**</span>하게 된다.

#### 문제 해결 - 세션 DB

![session_storage_db](/assets/img/development/server/2023-01-18/session_storage_db.png){:.centered width="90%"}
> “**Session Storage(세션 DB)**를 통해 SESSIONID 요청을 **한 DB(Session Storage)**에 요청하여 해결. 그런데...”

#### 또 다른 문제 발생 - 세션 DB의 문제

![session_storage_problem](/assets/img/development/server/2023-01-18/session_storage_problem.png){:.centered width="90%"}

- Client가 많아져서 요청이 많아지게 되면 Session Storage에 <span style="color:#ff8080">**`과부하`**</span>가 생겨 DB가 터지게 된다.
- 사용자를 위해 Cookie나 Browser의 힘을 빌려서 계속 로그인까지 하게 해줬는데 이젠 보안상의 문제가 생겨 Server 쪽에 와서 인증/인가를 해줬더니 문제가 계속 발생하게 된다.

### Stateful/Stateless

![stateful_stateless.png](/assets/img/development/server/2023-01-18/stateful_stateless.png){:.centered width="90%"}

- **`Client`**, **`Server`**, **`Session 저장소` 3가지 모두 사용자의 상태를 관리**할 수 있게 하였다. 그랬더니 문제가 발생하였다.
- 그 이유는 **`Client`**와 **`Server`**가 서로 통신할 때 사용하는 **`http`**와 Server 자체가 지향하는 **`REST API`**가 <span style="color:#ff8080">**`무상태성(Stateless)을 기초`**</span>로 하기 때문이다.
- 그런데, 실제로 인증과 인가를 구현할 때에는 사용자의 정보, 상태를 **`Client`**, **`Server`**, **`Session 저장소`** 3군데 모두 가지고 있었다.
- **그 말은 상태성을 갖고 있다는 이야기다**.
- **두 패러다임**이 <span style="color:#ff8080">**`충돌`**</span>되고 있다. 두 패러다임의 충돌을 해소해 보자.
- **`Client`**, **`Server`**, **`Session 저장소`** 모두 상태를 맡겨보았으니 나머지 <span style="color:#ff8080">**`통신(정보의 흐름)`**</span>에 맡겨보자.
- 요청과 응답 안에 사용자의 상태를 담아보자. 그걸로만 사용자의 인증과 인가를 처리하자.
- 그것이 바로 <span style="color:#ff8080">**`TOKEN`**</span>을 활용한 **인증**과 **인가**이다.

## JWT

![jwt](/assets/img/development/server/2023-01-18/jwt.png){:.centered width="90%"}
> “**`Secret key`**를 사용해서 **JWT**를 **만들어 내고 인증 과정을 거친다**.”

- **JWT** 자체는 해독하기 무척 쉽기 때문에 **JWT** 내에는 <span style="color:#ff8080">**`민감한 정보(PW)`**</span>를 대부분 담지 않는다.
- 그리고 <span style="color:#ff8080">**`Secret key`**</span> 중요한 만큼 노출되면 **JWT** 자체도 끝이 난다.
- 그래서 토큰을 사용하기 위해서는 **`Secret key`** <span style="color:#ff8080">**`서버 내부에 잘 관리`**</span>해야 한다.

### JWT 활용하기

![jwt_work_flow](/assets/img/development/server/2023-01-18/jwt_work_flow.png){:.centered width="90%"}
> “**`JWT`** 로직”

1. 요청을 보낸다.
2. Server에서 DB로 ID, PW를 체크한다.
3. 체크가 완료되면 
4. **`Secret key`**를 이용해서 **토큰**을 만들어 낸다.
    - 실제로는 더 길 수 도 있다.
5. **`Secret key`**를 이용 만든 토큰을 
6. **`header`**에 담아서 **Client**에 보내준다.
7. 다음부터 사용자는 해당 **`JWT`**로 **요청과 응답을 받는 형태**가 된다.

### JWT 좀 더 알아보기

![token](/assets/img/development/server/2023-01-18/token.png){:.centered width="90%"}
> “**`token`**”

1. **Client**로부터 **서버로 요청**이 왔다.
2. <span style="color:#ff8080">**`유효성 검사`**</span>를 **서버**에 있는 **`Secret key`**로 진행하게 된다.
3. 거기서 **유효하지 않**게 되면 버리게 되고, **유효**하게 되면 다음 단계인 <span style="color:#ff8080">**`사용자 정보를 파악`**</span>하게 된다.
4. **decoding 하기 쉽기** 때문에 사용자 정보 중에 **이름**으로 어떤 사용자인지 찾아낸다.
5. <span style="color:#ff8080">**`만료 시기`**</span>로 토큰의 만료 시기를 활용할 수 있다.
6. <span style="color:#ff8080">**`권한`**</span>으로 사용자의 권한(사용자/어드민 등)을 확인할 수 있다.
7. **주의**, <span style="color:#ff8080">**`비밀번호는 담으면 안 된다`**</span>. 비밀번호를 담게 되면 **디코딩을 쉽게** 할 수 있다는 점 때문에 **노출되기 쉽기 때문**이다.
8. **`Secret key`**를 통해서 유효성 검사를 통과한 토근은 이미 <span style="color:#ff8080">**`인증`**</span>을 받은 토큰이다.

### JWT 장점

![jwt_strengths](/assets/img/development/server/2023-01-18/jwt_strengths.png){:.centered width="90%"}
> “**`jwt의 장점`**”

1. Session Storage같은 경우 Session DB와 연관성이 있었는데, 이제는 **로드 밸런서**가 요청하는 곳에 **각자 서버**가 가진 **`Secret key`**로 해독해서 **인증을 진행**하면 된고 **요청을 반환**하면 된다는 장점이 있다.
2. <span style="color:#ff8080">**`확장성`**</span>이 좋아 **서버가 많아져도 똑같이 진행**할 수 있다.

### Token 단점

![jwt_weaknesses](/assets/img/development/server/2023-01-18/jwt_weaknesses.png){:.centered width="90%"}

1. <span style="color:#ff8080">**`해킹`**</span>을 당할 수 있다.
2. **Access Token**이 탈취 당하면 해당 해커는 사용자와 똑같은 <span style="color:#ff8080">**`지위`**</span>를 갖게 된다.

### Token 만료기한

1. **Token 만료기한**을 설정해 놓으면 만료기간 이후 **해커**뿐만 아니라 **사용자**도 사용할 수 없게 된다.
2. 그래서 사용자 입장에선 굉장히 **불편**할 수 있다.

### Refresh Token

![refresh_token](/assets/img/development/server/2023-01-18/refresh_token.png){:.centered width="90%"}
> “**`Refresh Token`**은 위 문제를 해결하고자 나온 Token 방식이다.”

1. 요청을 보낸다.
2. **`Secret key`**를 통해 **Token**을 만들어 낸다. 이때 **`Access Token`**과 <span style="color:#ff8080">**`Refresh Token`**</span>을 한 번에 만들어 낸다.
3. 그리고 **`Access Token`**은 저장하지 않고, **`Refresh Token`**만 따로 <span style="color:#ff8080">**`저장소`**</span>에 저장하게 된다.
4. **`Access Token`**과 **`Refresh Token`**을 한 번에 <span style="color:#ff8080">**`응답 헤더(Response Header)`**</span>로 보내게 된다.
5. **Client**는 둘 다 저장하게 된다.
6. **2번**에서 만든 **`Access Token`**은 **Server**에 저장하지 않는다.
7. 다음부터는 **Client**가 **`Access Token`**을 이용하여 <span style="color:#ff8080">**`요청`**</span>을 보내게 된다.

### Access Token 만료

![access_token_expire](/assets/img/development/server/2023-01-18/access_token_expire.png){:.centered width="90%"}
> “**`Access Token`**이 **만료**되면...<br/>
사용자는 **`Access Token`**이 **만료**되었다는 사실을 모르고, <span style="color:#ff8080">**`알 필요도 없다`**</span>.”

1. 사용자는 똑같이 요청을 보내게 된다.
2. 만료된 **access token** 이면 만료되었다고 Client에 알려준다.
3. 그럼 **Client**는 다시 **`Access Token`**과 **`Refresh Token`**을 한 번에 **Server**에 요청하게 되어 **저장소**에 있는 **`Refresh Token`**을 비교하고 확인되면
4. **`Secret key`**를 이용하여 <span style="color:#ff8080">**`갱신`**</span>된 **`Access Token`**을 다시 발급해 주고 준다.
5. **사용자**는 갱신된 **`Access Token`**을 사용하여 다음 요청들을 보내게 된다.
6. 만약, **`Refresh Token`**이 만료(<span style="color:#ff8080">**`7일 이전( 예) 2023.01.25 ~ 만료기한 2023.02.01)`**</span>) 되었다면, **`Secret key`**를 통해 **`Access Token`**과 **`Refresh Token`**을 다시 발급하여 위와 같은 로직을 수행하게 된다.

### Token 핵심

#### Token 장점

1. **토큰으로 상태 관리를 하기에 따로 세션을 둘 필요가 없다.**
2. <span style="color:#ff8080">**`효율성`**</span>이 **좋아**지고, **DB에 요청하여 직접 확인하지 않아도 되기 때문에 속도가 빠르다.**

#### Token 단점

1. **토큰 관리**를 해야 하며, 결국 토큰도 <span style="color:#ff8080">**`탈취`**</span>당할 수 있다.
2. **보안**에 있어 <span style="color:#ff8080">**`꾸준히 신경`**</span> 써야 한다.

# 마치며 - 더 공부해야 할 것들

## 보안(Security)

- **Token**
    - **JWT**
- **OAuth**
    - **OAuth1.0**
    - **OAuth2.0**
- **인증 방법**
    - **인증 서버**
- **Option**
    - **HttpOnly** : 서버에서 클라이언트로 정보를 보낼때 보낸 정보에 달아주는 **옵션**. **Storage**에 저장된 정보에 함부로 접근할 수 없게 만드는 **옵션**.
- **Sliding Session** : **Refresh Token**과 **상호 보안**적인 것.
- **Refresh Token** : **Sliding Session**과 **상호 보안**적인 것.
- **SSL/TLS1.3** : 접근하기 쉽고 가성비 좋은 보안 방법. => **HTTPS(가장 추천하는 방식)**
- **Haking 방법**

Continue with [[Security] OAuth](2023-01-26-oauth.md){:.heading.flip-title}
{:.read-more}