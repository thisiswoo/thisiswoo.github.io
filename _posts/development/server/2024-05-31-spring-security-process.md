---
layout: post
title: "[Spring] Spring Security 내부 동작 과정"
subtitle: "Spring Security internal operation process"
category: development
tags: server spring
image:
  path: /assets/img/development/server/2023-09-19_spring_security_overview/spring_security_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# Spring Security 내부 동작 과정
![](/assets/img/development/server/2024-05-31_spring_security_process/springsecurity_architechture.png){:.centered width="100%"}
Spring Security 내부 동작 과정을 요약한 이미지
{:.figcaption}


## Spring MVC 요청 과정
![](/assets/img/development/server/2024-05-31_spring_security_process/springmvc_architechture.png){:.centered width="100%"}
Spring MVC 요약한 이미지
{:.figcaption}

간략하게 Spring MVC 동작 과정을 살펴보저.
1. 사용자(client)가 웹 브라우저를 통해 특정 URL로 요청을 보낸다.
2. Spring의 모든 요청은 `FrontController` 역할을 하는 `DispatcherServlet`에 의해 처음 받아지게 된다.
3. 이때, `DispatcherServlet`은 `HandlerMapping`에 매핑 정보를 찾게 되는데, 요청 URL과 일치하는 핸들러를 찾는 역할을 수행한다.
4. 찾은 매핑 정보를 통해 `DispatcherServlet`은 `HandlerAdaptor`를 통해 각 핸들러 타입(`@Controller`의 `@RequestMapping`)을 연결해주는 역할을 하게 된다.
5. 그렇게 비지니스 로직을 거쳐 응답 받은 `DispatcherServlet`은 `ViewResolver` 를 통해 `Model` 객체를 반환하여 `View`에 전달하게 된다.
6. 사용자(`client`)는 응답받은 페이지와 데이터를 보게 된다.

[//]: # (이것이 `Spring MVC` 패턴의 기본적인 기능이다. 좀 더 자세한 내용은 아래의 포스트를 참고하길 바란다.)

[//]: # (Continue with [[Spring] Spring MVC]&#40;./2023-07-04-junit5-annotations-frequently-used.md&#41;{:.heading.flip-title})
[//]: # ({:.read-more})

## Spring Security 요청 과정
![](/assets/img/development/server/2024-05-31_spring_security_process/springsecurity_architechture.png){:.centered width="100%"}
Spring Security 내부 동작 과정을 요약한 이미지
{:.figcaption}

- 위 그림 처럼 일반적으로 `Client`가 `Server`로 요청을 보내면, **DispatcherServlet** 이라는 서블릿이 하나의 **HttpServletRequest** 를 받아서 요청을 처리하고 응답을 `Client`로 보낸다.
- 그런데, 하나 이상의 **Filter** 가 포함된다면, Client에서 보낸 요청이 서블릿으로 전달되기 전에 **Filter** 를 거치게 된다.
- `Client` 가 `Application` 에 하나의 요청을 보내면, **ServletContainer** 는 하나의 **필터 체인(FilterChain)** 을 생성한다.



# Reference
- [Security Filters](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-security-filters){:target="_blank"}
- [DelegatingFilterProxy](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-delegatingfilterproxy){:target="_blank"}
- [Spring Security Milestones](https://github.com/spring-projects/spring-security/milestones){:target="_blank"}
















