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
1. 사용자(client)가 웹 브라우저를 통해 특정 URL로 <span style="color:#ff8080">**요청**</span>을 보낸다.
2. Spring의 모든 요청은 `FrontController` 역할을 하는 `DispatcherServlet`에 의해 처음 받아지게 된다.
3. 이때, `DispatcherServlet`은 `HandlerMapping`에 매핑 정보를 찾게 되는데, 요청 URL과 일치하는 <span style="color:#ff8080">**핸들러**</span>를 찾는 역할을 수행한다.
4. 찾은 매핑 정보를 통해 `DispatcherServlet`은 `HandlerAdaptor`를 통해 각 핸들러 타입(`@Controller`의 `@RequestMapping`)을 <span style="color:#ff8080">**연결**</span>해주는 역할을 하게 된다.
5. 그렇게 비지니스 로직을 거쳐 응답 받은 `DispatcherServlet`은 `ViewResolver` 를 통해 `Model` 객체를 반환하여 `View`에 전달하게 된다.
6. 사용자(`client`)는 응답받은 페이지와 데이터를 보게 된다.

위 내용이 기본적인 Spring MVC의 요청 흐름이다.<br/>
그렇다면, 요청에 문제가 있거나, 권한이 없는 요청등이 중요한 데이터에 접근하려 한다면 어떻게 처리해야 할까?<br/>
이 문제를 Srping Security가 해결할 수 있게 되었다.<br/>  

[//]: # (Continue with [[Spring] Spring MVC]&#40;./2023-07-04-spring-mvc-pattern.md&#41;{:.heading.flip-title})
[//]: # ({:.read-more})

## Spring Securit 의 기본 동작 원리와 흐름
![](/assets/img/development/server/2024-05-31_spring_security_process/springsecurity_architechture.png){:.centered width="100%"}
Spring Security 내부 동작 과정을 요약한 이미지
{:.figcaption}

- `Spring Security`는 [Servlet Filter 기반](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-filters-review){:target="_blank"} 으로 동작하게 된다.
- 참고로 [Servlet과 Srping Context는 다르다](https://medium.com/@sigridjin/servletcontainer%EC%99%80-springcontainer%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4-%EB%8B%A4%EB%A5%B8%EA%B0%80-626d27a80fe5){:target="_blank"}.
  - `Servlet Filter` : <span style="color:#ff8080">**웹의 모든 요청**</span>을 <span style="color:#ff8080">**가로채어 먼저**</span> 처리하는 역할을 수행한다, 톰캣과 같은 <span style="color:#ff8080">**WAS에서 작동**</span>한다.
  - `Spring Context` : <span style="color:#ff8080">**스프링 IoC 컨테이너**</span>를 기반으로 구축되며, DI, AOP 등 다양한 기능을 제공한다.
- 선행된 요청들은 Servlet Filter의 과정을 모두 거치고 나서 Spring Container의 Context로 넘어오게 된다.


[//]: # (Continue with [[Spring] Spring IoC, DI, AOP, PSA]&#40;./2023-07-04-spring-ioc-di-aop_psa.md&#41;{:.heading.flip-title})
[//]: # ({:.read-more})


- 위 그림 처럼 일반적으로 `Client`가 `Server`로 요청을 보내면, **DispatcherServlet** 이라는 서블릿이 하나의 **HttpServletRequest** 를 받아서 요청을 처리하고 응답을 `Client`로 보낸다.
- 그런데, 하나 이상의 **Filter** 가 포함된다면, Client에서 보낸 요청이 서블릿으로 전달되기 전에 **Filter** 를 거치게 된다.
- `Client` 가 `Application` 에 하나의 요청을 보내면, **ServletContainer** 는 하나의 **필터 체인(FilterChain)** 을 생성한다.



# Reference
- [Security Filters](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-security-filters){:target="_blank"}
- [DelegatingFilterProxy](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-delegatingfilterproxy){:target="_blank"}
- [Spring Security Milestones](https://github.com/spring-projects/spring-security/milestones){:target="_blank"}
















