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
![](/assets/img/development/server/2024-04-30_spring_security_process/springsecurity_architechture.png){:.centered width="100%"}
Spring Security 내부 동작 과정을 요약한 이미지
{:.figcaption}

## Spring 요청 과정
- 일반적으로 Client가 Server로 요청을 보내면, **DispatcherServlet** 이라는 서블릿이 하나의 **HttpServletRequest** 를 받아서 요청을 처리하고 응답을 Client로 보낸다.
- 그런데, 하나 이상의 **Filter** 가 포함된다면, Client에서 보낸 요청이 서블릿으로 전달되기 전에 **Filter** 를 거치게 된다.
- Client 가 Application 에 하나의 요청을 보내면, **ServletContainer** 는 하나의 **필터 체인(FilterChain)** 을 생성한다.
- 



# Reference
- [Security Filters](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-security-filters){:target="_blank"}
- [DelegatingFilterProxy](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-delegatingfilterproxy){:target="_blank"}
- [Spring Security Milestones](https://github.com/spring-projects/spring-security/milestones){:target="_blank"}
















