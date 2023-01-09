---
layout: post
title: "[Java] JVM Stack & Heap"
subtitle: "Java Virtual Machine의 Stack & Heap"
category: development
tags: server java JVM Stack Heap
image:
  path: /assets/img/development/server/2023-01-02/jvm.jpeg
---

> “해당 포스팅은 **우아한Tech**의 [**[10분 테코톡] 🎅무민의 JVM Stack & Heap**](https://www.youtube.com/watch?v=UzaGOXKVhwU&list=LL&index=9&t=632s)의 영상을 기반으로 포스팅하였습니다. 또한 설명이 너무 잘되어었어 복습하고자 글로 남깁니다. 영상을 제작해주신 **우아한Tech**와 **무민**님에게 감사드립니다.”

<span style="font-size:30px;">\#**목차**</span>

* this unordered seed list will be replaced by the toc
{:toc}

# JVM(Java Virtual Machine)이란?

## 태초에 문제가 있었다...
- C/C++는 컴파일 플랫폼과 타겟 플랫폼이 다를 경우, 프로그램이 동작하지 않는다. **플랫폼은 운영체제와 CPU 아키텍처의 조합니다.**(플랫폼 = 운영체제(MacOS) + CPU 아키텍처(Apple M1))
- C나 C++는 Compile하고 나면 그 기계 code가 타겟 플랫폼이 dependent(의존)하기 때문에 Compile하는 플랫폼이랑 다를 경우에는 동작할 수 없다.
- 왜냐하면 OS마다 지원하는 System Call Interface(시스템 호출))가 다르고, CPU 아키텍쳐 마다 지원하는 ISA(Instruction Set Architecture)가 다르다. 
  - **System Call Interface 란** : Application이 운영체제(OS)에 있는 **function(함수(기능))을 호출 하는 것**을 의미한다. 
  - **ISA(Instruction Set Architecture) 란** : 명령어 집합 구조이며, 마이크로프로세서가 인식해서 **기능을 이해하고 실행 할 수 있는 기계어 명렁어**를 말한다.
<!-- 그래서 Bytecode Verifier(바이트코드 검증기)가 다르기 때문에 코드가 동작하지 않을 수도 있다. -->

## 개발할 때는 문제가 없다
- 동일한 플랫폼에서 컴파일과 실행을 같이 한다면, 프로그램은 아무 문제없이 동작한다.

![same_os](/assets/img/development/server/2023-01-02/same_os.png){:.centered width="90%"}

- macOS에서 compile하고 macOS에서 실행하게 되면 아무런 문제가 없다.
- 보통 local에서 test 작업을 할 때 많이 사용하는 방법.
- C/C++도 이와 같은 방법은 잘 작동한다.
- 문제는...

## 배포할 때 문제가 발생한다
- 플랫폼이 달라질 경우, 타겟 플랫폼에서 프로그램이 동작하지 않는다.

![other_os](/assets/img/development/server/2023-01-02/other_os.png){:.centered width="90%"}

- macOS에서 compile해서 나온 실행 파일을 window로 실행하게 되면 실행되지 않는다.
- 이게 문제이다... 이걸 해결하기 위해선...

## 크로스 컴파일(Cross Compile)
- 타겟 플랫폼에 맞춰 컴파일하는 것을 '크로스 컴파일'이라 한다.

![cross_compile](/assets/img/development/server/2023-01-02/cross_compile.png){:.centered width="90%"}

- macOS에서 windows로 타겟을 잡고 컴파일을 할 수 있다.
- 그렇게 나온 타겟 프로그램은 windows에서 실행이 가능하다.
- 이런식으로 C/C++같은 언어들은 **`Cross Compile`**을 이용하여 타겟 프로그래밍을 작업 하였다.

## JVM((Java Virtual Machine)으로 문제 해결
- Java 소스 코드가 **`.jacac`**라는 Compile을 거치고 나면 **Java Bytecode**가 된다.
- 이 Java Bytecode는 JVM이 설치된 플랫폼이라면 어떤 플랫폼이던 상관 없이 잘 동작하게 된다.
- 물론, JVM이 플랫폼과 관련된 작업들을 대신 해주기 때문에 가능한 것이다.
- 만약 배포하게 되는 곳의 OS가 **linux면 linux용 JVM**을 **windows면 windows의 JVM을** 설치하게 된다면 macOS에서 build 하여 compile한 소스 코드들을 다른 OS에서 실행할 수 있게 JVM이 작업을 해준다.

![solving](/assets/img/development/server/2023-01-02/solving_jvm.png){:.centered width="90%"}

# WORA
> “**Write Once, Run Anywhere(WORA)**” - Sun Mycrosystems

![wora](/assets/img/development/server/2023-01-02/write-once-run-anywhere-jvm.png){:.centered width="90%"}

- 즉, 당신이 코딩한 Java 코드를 Compile해서 배포하면, 어떤 플랫폼이든 다시 Compile할 필요 없이 실행시킬 수 있다. 단, 실행하려면 해당 플랫폼에 맞는 JVM을 설치되어 있어야 한다.


# Bytecode 실행 예제
