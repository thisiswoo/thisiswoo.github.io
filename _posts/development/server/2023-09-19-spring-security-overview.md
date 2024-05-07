---
layout: post
title: "[Spring] Spring Security 개요"
subtitle: "Spring Security Overview"
category: development
tags: server spring
image:
  path: /assets/img/development/server/2023-09-19_spring_security_overview/spring_security_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# Spring Security 개요
- 해당 포스트에서는 Spring Security의 핵심적인 인증(Authentication), 인가(Authorization), 보안과 관련된 개념을 다룬다.

## Spring Security 란?
**Spring Security**는 Java 웹 어플리케이션의 **인증**과 **인가(권한 부여)** 를 제공하며, 웹 공격에 대한 **방어 기능**을 제공 해주는 스프링의 하위 **프레임워크**이다. - [spring.io](https://spring.io/projects/spring-security){:target="_blank"}

## 인증과 권한(인가)이란?
- 인증과 권한(인가)에 대한 자세한 내용은 아래 링크를 통해 확인 할 수 있다.

Continue with [[Security] 인증과 인가](./2023-01-18-authentication-authorization.md){:.heading.flip-title}
{:.read-more}

간단히 인증과 인가(권한 부여)에 대한 뜻을 알아보자.

- **인증 - Authentication**
  - **(지문 또는 얼굴 등) 입증 가능한 정보**로 사용자의 신원을 **확인하는 과정**이다. 
  - 즉, **식별 가능한 정보로 서비스에 등록된 유저의 신원을 입증하는 과정**

- **인가(권한 부여) - Authorization**
  - **입증(인증) 된 신원**의 **권한에 대한 허가**를 나타내는 것이 **인가**이다. 
  - 즉, **인증된 사용자에 대한 특정 리소스에 접근할 권한이 있는지 확인하는 과정**
  
간단한 표로 인증과 인가의 차이를 알아보자.

| -      | 인증(Authentication)             | 인가(Authorization)               |
|--------|--------------------------------|---------------------------------|
| 기능     | 사용자에 대한 자격 증명 확인               | 사용자에 대한 권한 부여 또는 거부             |
| 작동     | 비밀번호, 생체 인식, 일회용 핀 또는 앱을 통해 작동 | 개발,보안팀에서 관리하는 설정을 통해            |
| 데이터 이동 | ID 토큰을 통해 데이터 이동               | 액세스 토큰(Access Token)을 통해 데이터 이동 |

## Spring Security에서의 인증, 인가 그리고 보호의 기능
### 인증(Authentication)
- **스프링 시큐리티**의 인증은 **양식((e.g)로그인 양식) 기반 인증**, **HTTP 기본 인증**, **세션 기반 인증**, **JWT(Json Web Token) 기반 인증**, **[OAuth 2.0](./2023-01-26-oauth.md)**, **[OpenID Connect](./2023-02-08-openid.md)** 등을 비롯한 다양한 **인증 메커니즘을 제공**한다.

### 인가(권한 부여(Authorization))
- **스프링 시큐리티**의 인가는 **애플리케이션에서 어떤 사용자가 특정 리소스에 액세스(접근) 할 수 있는지 세밀 하게 제어**할 수 있다.
- 이는 **역할 기반 액세스 제어(`RBAC`)**, **표현식 기반 액세스 제어(`EL 기반`)** 와 같은 다양한 기술을 사용하여 수행할 수 있다.

### 웹 공격으로부터의 보호
- **사이트 간 스크립팅(`XSS`)**, **사이트 간 요청 위조(`CSRF`)**, **세션 고정**과 같은 **웹 사이트 공격에 대한 기본 보호 기능을 제공**한다.

## Spring Security의 핵심 역할
### 인증(Authentication) : 사용자의 신원을 확인하는 프로세스
- **양식((e.g)로그인 양식) 기반 인증**
  - **가장 일반적인 인증 유형**으로, 사용자가 **로그인 양식**에 사용자 이름(`username`)과 비밀번호(`password`)를 입력하는 방식이다.
- **HTTP 기본 인증**
  - 이 인증 유형은 사용자가 **모든 요청(`Request`)** 의 **HTTP 헤더에 자격 증명을 제공**해야 한다.
  - 웹 서버와 클라이언트 간의 간단한 인증 방식이며, 이 방식은 `HTTP 프로토콜`에 **내장된 메커니즘**으로, **사용자 이름과 비밀번호를 Base64로 인코딩한 후 HTTP 헤더에 추가하여 서버에 전송**하게된다.
- **OAuth 2.0**
  - **OAuth 2.0**은 인증에 널리 사용되는 개방형 표준이다.
  - **Spring Security**는 OAuth 2.0을 지원하므로 애플리케이션을 다른 OAuth 2.0 호환 시스템과 쉽게 통합할 수 있다.
    - e.g) **`Google`**, **`Naver`**, **`Kakao`** 로그인 등등...

Continue with [[Security] OAuth 2.0](./2023-01-26-oauth.md){:.heading.flip-title}
{:.read-more}

- **OpenID Connect**
  - **OpenID Connect** 는 **OAuth 2.0를 기반으로 구축된 ID 계층**이다.
  - 사용자 프로필 정보 및 싱글 사인온(`SSO`)과 같은 추가 기능을 제공한다.
    > **SSO(Single Sign On) 이란?**<br>한 번의 로그인으로 여러 가지 다른 사이트들을 자동적으로 접속하여 이용하는 방법을 말한다.

Continue with [[Security] OpenID Connect](./2023-02-08-openid.md){:.heading.flip-title}
{:.read-more}

### 인가(권한 부여(Authorization)): 사용자가 특정 리소스에 액세스(접근)할 수 있는지 여부를 결정하는 프로세스
- **역할 기반 액세스 제어(`RBAC(Role-Based Access Control)`)**
  - **사용자의 역할에 따라 시스템 리소스에 대한 접근을 제어하는 방식**이며, 이는 사용자가 아닌 **"역할"에 권한을 부여** 하고, **사용자는 해당 역할을 통해 권한을 얻게 된다.**
  - 예를 들어, **'관리자'**, **'사용자'**, **'게스트'** 등의 **역할**을 **미리 정의**하고, 이 역할에 따라 **특정 리소스에 대한 접근 권한을 부여**한다. 
  - `RBAC`는 간단하고 이해하기 쉬운 모델로, 대부분의 시스템에서 널리 사용되고 있습니다.

- **표현식 기반 액세스 제어(`EL(Expression Language) 기반`)**
  - **EL 기반 액세스 제어** 는 **보안 결정**을 위해 **보안 표현식을 사용하는 방식**이다. 
  - 이는 **사용자의 역할**, **액세스하려는 리소스**, **애플리케이션의 현재 상태**와 같은 **여러 요소에 따라 달라지는 권한 부여 규칙을 구현하는 데 유용**할 수 있다.
  - 또한, **사용자의 역할 뿐만 아니라 다양한 조건(로그인 상태, IP 주소, 시간 등)을 사용하여 접근 권한을 판단**한다. 
  - 예를 들어, <b>`.hasRole('ROLE_ADMIN')`</b>이나 <b>`.hasIpAddress('192.168.1.0/24')`</b>와 같은 **표현식을 사용**할 수 있다.

### 웹 공격으로부터의 보호
**Spring Security는 다양한 웹 공격으로부터 애플리케이션을 보호한다.**

- **CSRF(Cross-Site Request Forgery) 공격 방어**
  - <b>`CSRF(Cross-Site Request Forgery)`</b>는 **사용자가 자신의 의지와는 무관하게 공격자가 의도한 행동을 하도록 만드는 공격**이다. 
  - **스프링 시큐리티**는 **CSRF 토큰**을 사용하여 이러한 공격을 **방어**하게 되는데, 기능적으로 보면 **CSRF 토큰**은 **서버에서 생성하여 클라이언트에게 전달하고, 클라이언트는 이후 요청에 이 토큰을 포함하여 서버로 전송**한다. 
  - **서버는 이 토큰을 검증하여 유효한 요청인지를 판단**한다.

- **세션 고정(Session Fixation) 공격 방어**
  - **세션 고정 공격**은 **공격자가 특정 세션 ID를 미리 알아내고 이를 이용하여 다른 사용자의 세션을 탈취하려는 공격**이다. 
  - **스프링 시큐리티**는 **로그인 시 새로운 세션을 생성**하여 **이전 세션의 정보를 새 세션으로 복사하는 방식**으로 **세션 고정 공격을 방어**한다.

- **클릭재킹(Clickjacking) 방어**
  - **클릭재킹**은 **사용자가 의도한 곳이 아니라 다른 곳을 클릭하도록 만드는 공격**이다. 
  - **스프링 시큐리티**는 <b>`X-Frame-Options 헤더`</b> 를 이용하여 이러한 공격을 **방어**한다. 
  - **이 헤더를 통해 브라우저는 현재 페이지가 `iframe` 내에서 렌더링되는 것을 방지**할 수 있다.
    > **iframe이란?**<br/>현재 HTML 페이지 안에 다른 HTML 페이지를 포함시키는 **중첩된 브라우저**이다.<br/><b>`iframe`</b> 요소를 이용하면 해당 웹 페이지 안에 **어떠한 제한 없이 다른 페이지를 불러와서 삽입** 할 수 있다.

- **XSS(Cross-Site Scripting) 방어**
  - **XSS(Cross-Site Scripting)**는 **공격자가 웹 페이지에 악성 스크립트를 삽입하여 다른 사용자의 데이터를 탈취하거나 조작하는 공격**이다. 
  - **스프링 시큐리티**는 **사용자의 입력을 필터링하거나 이스케이프하여 이러한 공격을 방어**한다.
    > **이스케이프 처리란?**<br/><b>`& => &amp;`</b>, <b>`' => &#x27;`</b>, <b>`" => &quot;`</b>, <b>`< => &lt;`</b>, <b>`> => &gt;`</b>, <b>`/ => &#x2F;`</b><br/>즉, 코드로써 작성될 수 있는 기호들을 **이스케이프** 처리한 텍스트로 작성하는 것이다.

- **SQL Injection 방지**
  - **SQL 인젝션**은 **공격자가 애플리케이션의 입력값을 조작하여 의도치 않은 SQL 쿼리를 실행하는 공격**이다. 
  - <b>스프링 시큐리티 자체는 SQL 인젝션을 방지하는 기능을 제공하지 않지만, 스프링 프레임워크의 일부인 `JdbcTemplate` 또는 `JPA`, `Hibernate`와 같은 `ORM(Object Relational Mapping)` 도구를 사용하면 SQL 인젝션을 방지</b>할 수 있다. 
  - **이들 도구는 파라미터 바인딩을 통해 쿼리를 안전하게 만들어 준다.**
    > **파라미터 바인딩이란?**<br/>
      - **이름 기반 바인딩** : **`=:연산자`를 사용, 메서드 체이닝을 사용**
        ```java
        Query query = em.createQuery("select m from Member m where m.username =: username").setParameter("username", userName);    
        ```
    
      - **위치 기반 바인딩** : **`=?연산자`를 사용, 메서드 체이닝을 사용**
        ```java
        Query query = em.createQuery("select m from Member m where m.username =? 1").setParameter(1, userName);    
        ```

- **권한 우회 공격 방어**
  - **권한 우회 공격**은 **공격자가 본인의 권한 이외의 리소스에 접근하려는 시도**이다. 
  - **스프링 시큐리티**는 **URL 레벨이나 메소드 레벨에서 접근 제어를 설정하여 이러한 공격을 방어**할 수 있다. 
  - 예를 들어, **특정 URL은 관리자 권한을 가진 사용자만 접근할 수 있도록 설정할 수 있다.** 
  - 또한, <b>표현식 기반 EL`(Expression Language)`의 접근 제어를 통해 더욱 세밀한 권한 설정이 가능</b>하다.

## 정리
**스프링 시큐리티**는 이런 방식으로 **인증**, **인가 과정을 관리**하고, **애플리케이션을 다양한 웹 공격으로부터 보호**한다. 
**이를 통해 개발자는 보안과 관련된 부분에 대해 신경 쓰지 않고, 비즈니스 로직에 집중할 수 있게 해준다.**

[//]: # (Continue with [[Spring] Spring Security 내부 동작 과정]&#40;./2024-05-31-spring-security-process.md&#41;{:.heading.flip-title})
[//]: # ({:.read-more})

# Reference
- [docs.spring.io](https://docs.spring.io/spring-security/reference/index.html){:target="_blank"}
- [Authentication and Authorization](https://hyperskill.org/learn/step/14219){:target="_blank"}

<!-- Link -->