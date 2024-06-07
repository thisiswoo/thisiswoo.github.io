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

- Spring Security 내부 동작 과정을 알기 위해선 <span style="color:#ff8080">**Spring MVC의 기본 동작 원리**</span>를 알고 있어야 한다.
- 우선, 아래 Spring MVC 내부 동작 과정에 대해 간략하게 알아보자.

## Spring MVC 요청 과정
![](/assets/img/development/server/2024-05-31_spring_security_process/springmvc_architechture.png){:.centered width="100%"}
Spring MVC 요약한 이미지
{:.figcaption}

1. 사용자는 웹 브라우저를 통해 <span style="color:#ff8080">**특정 `URL`로 요청**</span>을 보낸다. 그 요청에는 **HTTP 메서드**, **헤더 정보**, **본문 데이터 등**이 포함될 수 있다.
2. 모든 요청은 Spring의 웹 서버에 도착하고, `FrontController` 역할을 하는 <span style="color:#ff8080">**`DispatcherServlet`**</span>에 의해 처리가 되는데, **요청 URL**, **HTTP 메서드**, **요청 헤더 정보 등**을 분석하여 요청을 처리할 수 있는 핸들러를 <span style="color:#ff8080">**`HandlerMapping`**</span>에 조회한다.
3. <span style="color:#ff8080">**`HandlerAdapter` 목록**</span>에서 요청에 맞는 <span style="color:#ff8080">**핸들러를 조회**</span>한다.
4. `DispatcherServlet`은 조회한 핸들러의 타입을 기반으로 <span style="color:#ff8080">**`HandlerAdapter`**</span>를 통해서 <span style="color:#ff8080">**핸들러를 실행**</span>한다.
5. **핸들러**는 일반적으로 `@Controller`나 `@RestController`와 같은 <span style="color:#ff8080">**애노테이션으로 정의**</span>되며, `@RequestMapping`, `@GetMapping`, `@PostMapping` 등의 애노테이션을 통해 비즈니스 로직을 거쳐 <span style="color:#ff8080">**`Model` 객체를 생성**</span>하여 `DispatcherServlet`에 반환한다.
6. `DispatcherServlet`은 <span style="color:#ff8080">**`ViewResolver`**</span>에게 `Model` 객체를 전달하고, 뷰 이름을 실제 뷰 페이지 경로로 변환하고, **[View Template Engine(Thymeleaf, JSP 등)](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-template-engines){:target="_blank"}** 을 이용하여 `Model` 객체의 데이터를 HTML로 렌더링 하는 <span style="color:#ff8080">**`View` 객체를 생성**</span>한다. 
7. 그렇게 렌더링 되어 생성된 <span style="color:#ff8080">**`HTML`**</span> 코드를, `DispatcherServlet`에 응답해 준다. 
8. `DispatcherServlet`은 `ViewResolver`에서 렌더링 된 HTML 코드를 사용자(`Client`)에게 <span style="color:#ff8080">**`HTTP 응답`**</span>으로 전송한다. 

[//]: # (Continue with [[Spring] Spring MVC]&#40;./2023-07-04-spring-mvc-pattern.md&#41;{:.heading.flip-title})
[//]: # ({:.read-more})

위 내용이 기본적인 Spring MVC의 요청 흐름이다.<br/>

**그렇다면, 요청에 문제가 있거나, 권한이 없는 요청 등이 중요한 데이터에 접근하려 한다면 어떻게 처리해야 할까?** 이 문제를 <span style="color:#ff8080">**Srping Security가 해결**</span>할 수 있게 되었다. 

## Spring Securit 의 기본 동작 원리와 흐름
![](/assets/img/development/server/2024-05-31_spring_security_process/springsecurity_architechture.png){:.centered width="100%"}
Spring Security 내부 동작 과정을 요약한 이미지
{:.figcaption}

위 그림은 `Spring MVC`에서 `Spring Security`를 적용했을 때의 그림이다.

1. 사용자는 웹 브라우저 또는 기타 애플리케이션을 통해 요청을 보낸다. 요청은 <span style="color:#ff8080">**웹 서버**</span>(예: `Nginx`, `Apache`)에 도달하고, 웹 서버는 요청을 `Servlet Container`(예: `Tomcat`)로 <span style="color:#ff8080">**전달**</span>한다.
2. `Servlet Container`는 전달 받은 요청을 `FilterChain` (`FilterChain`은 `Servlet Container`에 등록된 필터들의 목록)에 전달하고, 필터들은 순서대로 실행되며, <span style="color:#ff8080">**요청을 처리하고 다음 필터로 전달할지 여부를 결정**</span>한다.
3. `FilterChain`의 `DelegatingFilterProxy`는 `Spring Context`에서 [SecurityFilterChain](https://spring.io/blog/2022/02/21/spring-security-without-the-websecurityconfigureradapter#configuring-httpsecurity){:target="_blank"}의 `@Bean`을 조회한다.

    **Spring Security less than 5.7 version**
    ```java
    @Configuration
    public class SecurityConfiguration extends WebSecurityConfigurerAdapter {
    
        @Override
        protected void configure(HttpSecurity http) throws Exception {
            http
                .authorizeHttpRequests((authz) -> authz
                    .anyRequest().authenticated()
                )
                .httpBasic(withDefaults());
        }
    }
    ```
    
    **Spring Security 5.7 version or more**
    ```java
    @Configuration
    public class SecurityConfiguration {
    
        @Bean
        public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
            // 각종 설정 들을 구성한다.
            http
                .authorizeHttpRequests((authz) -> authz
                    .anyRequest().authenticated()
                )
                .httpBasic(withDefaults());
            return http.build();
        }
    }
    ```

      > **참고**<br/>`Spring Security 5.4`에서는 `SecurityFilterChain` <span style="color:#ff8080">**빈(@Bean)**</span>을 생성하여 `HttpSecurity`를 **구성**하는 기능을 도입하였고, `Spring Security 5.7.0-M2` 부터 `WebSecurityConfigurerAdapter`의 기능이 **Deprecated** 되었다.

   - <span style="color:#ff8080">**여기서 문제가 발생**</span>한다. 필터는 `WAS`내에서 `Spring Security`는 `Spring Context`에서 <span style="color:#ff8080">**각각 독립적인 구성요소**</span>로 운영 된다. 즉, <span style="color:#ff8080">**필터에서는 직접적으로 스프링 기능을 활용하기 어렵다.**</span>
   - 그렇다면, **어떻게 Spring Security를 사용할 수 있게 해결했을까?** 바로, [DelegatingFilterProxy](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-delegatingfilterproxy){:target="_blank"}가 `Spring Security`의 `FilterChainProxy`에 <span style="color:#ff8080">**위임(delegation)**</span>하는 전략을 취하게 되었다. 즉, <span style="color:#ff8080">**위임(delegating)**</span>을 통해 **문제를 해결**하였다.

4. 그렇게 조회한 `@Bean`의 `SecurityFilterChain`에 설정된 보안(`CSRF`, `XSS` 등), 인증(`Authentication`), 인가(`Authorization`) 등의 작업을 통해 원하는 접근 제어를 할 수 있다.
5. `SecurityFilterChain`에서 설정한 설정 값들이 모두 통과하게 되면 다음 필터의 단계로 넘어가게 되며, 모든 `FilterChain`의 필터들이 정상적으로 통과하게 되면 클라이언트의 요청은 `DispatcherServlet`으로 넘어가게 된다.

> **참고**<br/>`Spring Security`는 [Servlet Filter](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-filters-review){:target="_blank"} 기반으로 동작하며, [Servlet과 Srping Context는 다르다](https://medium.com/@sigridjin/servletcontainer%EC%99%80-springcontainer%EB%8A%94-%EB%AC%B4%EC%97%87%EC%9D%B4-%EB%8B%A4%EB%A5%B8%EA%B0%80-626d27a80fe5){:target="_blank"}.<br/>&nbsp;&nbsp;- `Servlet Filter` : <span style="color:#ff8080">**웹의 모든 요청**</span>을 <span style="color:#ff8080">**가로채어 먼저**</span> 처리하는 역할을 수행한다, 톰캣과 같은 <span style="color:#ff8080">**WAS에서 작동**</span>한다.<br/>&nbsp;&nbsp;- `Spring Context` : <span style="color:#ff8080">**스프링 IoC 컨테이너**</span>를 기반으로 구축되며, `DI`, `AOP` 등 다양한 기능을 제공한다.

### package org.springframework.security.web
```java
public interface SecurityFilterChain {
    boolean matches(HttpServletRequest request);

    List<Filter> getFilters();
}
```
- `SecurityFilterChain`의 구현체는 `matches()`를 통해 자신에게 온 요청인지 확인하고 매칭된다면 `getFilters()`를 통해 `FilterChainProxy`에게 필터 체인을 제공한다. 즉, 여러 개의 필터 체인을 가지고 있을 수 있으며 요청에 따라 다른 보안 로직을 처리할 수 있다는 뜻이다.


[//]: # (Continue with [[Spring] Spring IoC, DI, AOP, PSA]&#40;./2023-07-04-spring-ioc-di-aop_psa.md&#41;{:.heading.flip-title})
[//]: # ({:.read-more})

### 그렇다면 왜 서블릿 필터와 스프링 컨텍스트로 나누는 전략을 취했을까?
1. **분리된 책임** : **서블릿 필터(Servlet Filter)** 는 주로 **웹 요청과 응답에 대한 사전처리 및 사후 처리를 담당**한다. 반면, **스프링 컨텍스트(Spring Context)** 는 **애플리케이션의 비즈니스 로직, 빈 관리, 데이터 접근, 서비스 계층 등을 관리**하게 된다. 이러한 분리는 <span style="color:#ff8080">**관심사의 분리(Separation of concerns, SoC)**</span> 원칙을 따르며, 각각의 영역이 자신의 역할에 집중할 수 있도록 설계되었다. 그래서 개발자는 각 영역에 맞는 코드 작업과 테스트할 수 있다.
2. **유연성과 확장성** : **서블릿 필터(Servlet Filter)** 와 **스프링 컨텍스트(Spring Context)** 를 분리함으로써, **각각 독립적으로 발전하고 확장할 수 있는 유연성을 제공**한다. 예를 들어, 보안, 로깅, 인증 같은 기능을 필터에서 처리하고, 비즈니스 로직은 스프링 컨텍스트에서 처리할 수 있다. 이는 <span style="color:#ff8080">**코드의 재사용 성과 유지보수 성을 향상**</span>시키게 된다.
3. **기술적인 호환성** : 서블릿 API는 JavaEE 사양의 일부로, 다양한 웹 애플리케이션 서버(WAS)에서 지원된다. 스프링 프레임워크는 이러한 서블릿 API 위에서 동작하며, 스프링 기능을 제공한다. 이 구조는 스프링 기반 애플리케이션을 다양한 환경에서 실행할 수 있는 <span style="color:#ff8080">**기술적 호환성을 보장**</span>한다.
4. **보안의 강화** : 보안 관련 처리를 <span style="color:#ff8080">**서블릿 필터(Servlet Filter)** 단계에서 먼저 처리함으로써, 악의적인 요청이 **스프링 컨텍스트(Spring Context) 내부의 비즈니스 로직에 도달하기 전에 차단**</span>될 수 있다. 이는 애플리케이션의 전반적인 <span style="color:#ff8080">**보안 수준을 향상**</span>시키게 된다.

이러한 이유로, **서블릿 필터(Servlet Filter)** 와 스프링 **컨텍스트(Spring Context)** 를 분리하여 애플리케이션의 구조를 더욱 견고하고 안전하게 만들며, 개발자가 유연하고 효율적인 애플리케이션을 개발하고 관리할 수 있도록 돕는다.<br/>
물론, 이러한 장점만 있는 것은 아니다.<br/><br/>
가장 큰 <span style="color:#ff8080">**단점**</span>은 <span style="color:#ff8080">**복잡성이 증가**</span>하여 두 시스템을 별도로 관리해야 하기 때문에 개발 및 유지 관리에 더 많은 노력과 시간이 투자가 된다.<br/>
하지만, 앞서 언급한 장점들을 고려했을 때, 일반적으로 웹 애플리케이션 개발에서는 **서블릿 필터(Servlet Filter)** 와 **스프링 컨텍스트(Spring Context)** 를 나누는 전략을 사용하는 것이 유리하며, 대규모 서비스나 복잡한 웹 애플리케이션 개발 시에는 이러한 전략의 장점들이 뚜렷이 나타나게 된다.





# Reference
- [Security MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html){:target="_blank"}
- [Security Filters](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-security-filters){:target="_blank"}
- [DelegatingFilterProxy](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-delegatingfilterproxy){:target="_blank"}
- [Spring Security Milestones](https://github.com/spring-projects/spring-security/milestones){:target="_blank"}
- [Progrow](https://somuchthings.tistory.com/195){:target="_blank"}















