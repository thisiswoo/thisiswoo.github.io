---
layout: post
title: "[Java] Java 란?"
subtitle: "what is Java"
category: development
tags: server java
image:
  path: /assets/img/development/server/2022-12-25/java_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

## Java 란?

> “Java는 웹 애플리케이션 코딩에 널리 사용되는 프로그래밍 언어이다. 20년 넘게 개발자들 사이에서 많은 인기를 끌은 결과, 오늘날 수백만 개의 Java 애플리케이션이 사용되고 있다. Java는 그 자체로 플랫폼으로 사용할 수 있는 다중 플랫폼, **객체 지향** 및 네트워크 중심 언어이다. 모바일 앱 및 엔터프라이즈 소프트웨어에서 빅 데이터 애플리케이션 및 서버 측 기술에 이르기까지 모든 것을 코딩하기 위한 빠르고 안전하며 안정적인 프로그래밍 언어이다.” - [AWS](https://aws.amazon.com/ko/what-is/java/)

## Java의 특징

### 객체지향 언어(Object Oriented Programming - OOP)

![OOP](/assets/img/development/server/2022-12-25/OOP.jpg){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://www.dreamstime.com/object-oriented-programming-oop-paradigm-explanation-outline-diagram-object-oriented-programming-language-oop-paradigm-image239724045)</div>

> “프로그램을 수많은 '객체(object)'라는 기본 단위로 나누고 이들의 상호작용으로 서술하는 방식이다. 객체란 하나의 역할을 수행하는 '메소드와 변수(데이터)'의 묶음으로 봐야 한다. 또한 접근제한자를 통한 객체를 나누어 사용하게 된다. **`public`**, **`protected`**, **`default`**, **`private`**” - [OOP](https://namu.wiki/w/%EA%B0%9D%EC%B2%B4%20%EC%A7%80%ED%96%A5%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)

<!-- Continue with [[Java] 객체지향이란?](vue-lifecycle){:.heading.flip-title}
{:.read-more} -->

### 높은 이식성

> “서로 다른 실행 환경을 가진 시스템간에 프로그램을 옮겨 실행할 수 있다. 또한 Java는 JRE(자바 실행 환경)이 설치 되어 있는 모든 운영체제에서 실행 가능하다.” - [이식성](https://helloworld-88.tistory.com/3)

### 인터프리터 언어

> “프로그래밍한 소스파일(.java)을 컴파일 하여 2진 파일(.class)로 만든 다음 JRE가 class파일을 인터프리터 하여 실행한다.” - [인터프리터](https://helloworld-88.tistory.com/3)

### 메모리 자동관리(Garbage Collection)

> “**Java는 객체 생성시 자동으로 메모리 영역을 찾아서 할당하고, 사용이 완료되면 `Garbage Collector`를 실행 시켜 자동으로 사용하지 않는 객체를 제거하여 메모리 관리**를 해준다.” - [가비지 컬렉션](https://helloworld-88.tistory.com/3)

<!-- Continue with [[Java] 가비지 컬렉션 이란?](vue-lifecycle){:.heading.flip-title}
{:.read-more} -->

### 멀티 스레드

> “운영체제에 따라 멀티 스레드를 구현하는 방법이 다르지만, 자바는 스레드 생성 및 제어와 관련된 라이브러리 API를 제공하므로 운영체제에 상관없이 멀티 스레드를 쉽게 구현할 수 있다.” - [멀티 스레드](https://helloworld-88.tistory.com/3)

<!-- Continue with [[Java] 멀티 스레드란?](vue-lifecycle){:.heading.flip-title}
{:.read-more} -->

### 동적 로딩

> “실행 시 모든 클래스가 로딩되는 것이 아니라 필요한 시점에 클래스를 로딩하여 사용할 수 있다.” - [동적 로딩](https://helloworld-88.tistory.com/3)

### 분산환경 지원

> “Java에는 TCP/IP 라이브러리가 기본적으로 포함되어 있고, http와 http프로토콜을 기본적으로 지원한다.” - [분산환경 지원](https://helloworld-88.tistory.com/3)

### 오픈소스 라이브러리가 풍부

> “Java에 대한 자료가 풍부하다.”

## Java 사용 분야

### 1.게임 개발

![Minecraft](/assets/img/development/server/2022-12-25/Minecraft_cover.png){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://en.wikipedia.org/wiki/Minecraft)</div>

**<center>Minecraft</center>**

> “많은 인기 모바일, 컴퓨터 및 비디오 게임이 Java로 작성된다. 기계 학습이나 가상 현실과 같은 첨단 기술이 통합된 최신 게임도 Java 기술로 작성된다. **가장 대표적인 게임이 [Minecraft](https://en.wikipedia.org/wiki/Minecraft)**이다.”

### 2.클라우드 컴퓨팅(Cloud Computing)

> “Java는 **WORA(Write Once and Run Anywhere - 한 번 작성하면 어느 곳에서 나 실행)**라고도 하며 **분산 클라우드 기반 애플리케이션에 적합**하다. 클라우드 공급자는 다양한 기본 플랫폼에서 프로그램을 실행하기 위해 Java 언어를 선택한다.” - [AWS](https://aws.amazon.com/ko/what-is/java/)

### 3.빅 데이터(Big Data)

![pl-ranking](/assets/img/development/server/2022-12-25/2022-pl-rangking.png){:.centered width="70%"}
<div style="text-align: center">[그림출처](https://www.tiobe.com/tiobe-index/)</div>

**<center>PL-Ranking</center>**

> “Java는 복잡한 데이터 집합과 방대한 양의 실시간 데이터를 처리할 수 있는 데이터 처리 엔진에 사용된다.” - [AWS](https://aws.amazon.com/ko/what-is/java/)

* 빅 데이터 관련 프로그래밍 언어는 **`Python`**이 랭킹이 높지만 **`Java`** 또한 빅 데이터뿐만 아니라 여러 분야에 많이 사용되어 순위가 높다는 걸 알 수 있다.

### 4.인공지능(Artificial intelligence)

> “Java는 기계 학습 라이브러리의 발전소 역할을 한다. **Java는 자연어 처리 및 딥 러닝과 같은 인공 지능 애플리케이션 개발에 적합한 안정성과 속도를 갖추고 있다.**” - [AWS](https://aws.amazon.com/ko/what-is/java/)

* 인공지능 또한 **`Python`**이 랭킹이 높지만 **`Java`** 역시 여러 분야에서 많이 사용가능한 언어이다.

### 5.사물 인터넷(Internet of things - IoT)

> “Java는 인터넷에 독립적으로 연결할 수 있는 엣지 디바이스의 센서 및 하드웨어 프로그래밍에 사용되었다.” - [AWS](https://aws.amazon.com/ko/what-is/java/)

# 결론!

- **Java는 객체지향 언어로 높은 이식성과 메모리 자동관리, 멀티 스레드, 분산환경, 풍푸반 오픈소스 라이브러리로 개발자들에게 많은 사랑을 받고있는 언어**이다.
- 게임분야 뿐만 아니라 클라우드 컴퓨팅, 빅데이터, 인공지능, 사물 인터넷등에 사용되고 있다.
