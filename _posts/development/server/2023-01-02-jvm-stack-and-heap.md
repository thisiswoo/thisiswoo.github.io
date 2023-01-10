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
- C/C++는 컴파일 플랫폼과 타겟 플랫폼이 다를 경우, 프로그램이 동작하지 않는다. **플랫폼은 운영체제와 CPU 아키텍처의 조합이다.**(플랫폼 = 운영체제(MacOS) + CPU 아키텍처(Apple M1))
- C나 C++는 Compile하고 나면 그 기계 code가 타겟 플랫폼이 **dependent(의존)**하기 때문에 Compile하는 플랫폼이랑 다를 경우에는 동작할 수 없다.
- 왜냐하면 OS마다 지원하는 System Call Interface(시스템 호출))가 다르고, CPU 아키텍쳐 마다 지원하는 ISA(Instruction Set Architecture)가 다르다. 
  - **System Call Interface 란** : Application이 운영체제(OS)에 있는 **function(함수(기능))을 호출 하는 것**을 의미한다. 
  - **ISA(Instruction Set Architecture) 란** : 명령어 집합 구조이며, 마이크로프로세서가 인식해서 **기능을 이해하고 실행 할 수 있는 기계어 명령어**를 말한다.
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

- macOS에서 compile해서 나온 실행 파일(.exe)을 window로 실행하게 되면 실행되지 않는다.
- 이게 문제이다... 이걸 해결하기 위해선...

## 크로스 컴파일(Cross Compile)
- 타겟 플랫폼에 맞춰 컴파일하는 것을 '크로스 컴파일'이라 한다.

![cross_compile](/assets/img/development/server/2023-01-02/cross_compile.png){:.centered width="90%"}

- macOS에서 windows로 타겟을 잡고 컴파일을 할 수 있다.
- 그렇게 나온 타겟 프로그램은 windows에서 실행이 가능하다.
- 이런식으로 C/C++같은 언어들은 **`Cross Compile`**을 이용하여 타겟 프로그래밍을 작업 하였다.

## JVM(Java Virtual Machine)으로 문제 해결

![solving](/assets/img/development/server/2023-01-02/Java-virtual-machine-task-exchange-on-a-loosely-coupled-network.png){:.centered width="90%"}
<div style="text-align: center">[그림출처](https://www.researchgate.net/figure/Java-virtual-machine-task-exchange-on-a-loosely-coupled-network_fig3_334064226)</div>

- Java 소스 코드가 **`.jacac`**라는 Compile을 거치고 나면 **Java Bytecode**가 된다.
- 이 Java Bytecode는 JVM이 설치된 플랫폼이라면 어떤 플랫폼이던 상관 없이 잘 동작하게 된다.
- 물론, JVM이 플랫폼과 관련된 작업들을 대신 해주기 때문에 가능한 것이다.
- 만약 배포하게 되는 곳의 OS가 **linux면 linux용 JVM**을 **windows면 windows의 JVM을** 설치하게 된다면 macOS에서 build 하여 compile한 소스 코드들을 다른 OS에서 실행할 수 있게 JVM이 작업을 해준다.


## WORA
> “**Write Once, Run Anywhere(WORA)**” - Sun Mycrosystems

![wora](/assets/img/development/server/2023-01-02/write-once-run-anywhere-jvm.png){:.centered width="90%"}
<div style="text-align: center">[그림출처](https://miro.medium.com/max/1400/1*8unTYz6pOhwryEb5b1S2Sg.png)</div>

- 즉, 당신이 코딩한 Java 코드를 Compile해서 배포하면, 어떤 플랫폼이든 다시 Compile할 필요 없이 실행시킬 수 있다. 단, 실행하려면 해당 플랫폼에 맞는 JVM을 설치되어 있어야 한다.

## 굳이 JVM?
- C/C++도 Cross Compile해서 배포하면 되는데, 굳이 JVM을 사용해야 하는가? 굳이 JVM을 도입한 이유가 뭘까?
  - **Java**가 나오던 시기(1990년대)는 **네트워크가 발전하던 시기**이다.
  - 네트워크로 모든 것들이 연결 되고 다 기종의 디바이스에서 실행하기 위한 프로그램이 필요했다.
  - 그러나 C/C++은 당시에만 해도 Cross Compile을 해주는 번거러움이 발생했다.
    - 예를 들어 일반 운영체제 linux나 window가 아니라 Android, iOs, ubuntu, solaris 등등 등장하게 되어 Cross Compile에 **한계가 생기기 시작**하였다.
- **Java는 네트워크에 연결된 모든 디바이스에서 작동하는 것이 목적**이었다.
- 디바이스마다 운영체제나 하드웨어가 다르기 때문에, 자연스럽게 **플랫폼에 의존(dependent)하지 않도록 언어를 설계**했다. 그 결과가 Java Bytecode와 JVM이 등장하게 되었다.

## Java의 야심

![java's ambition](/assets/img/development/server/2023-01-02/java_ambition.png){:.centered width="90%"}
<div style="text-align: center">[그림출처](https://i.ytimg.com/vi/4Rk_zDimf2s/mqdefault.jpg)</div>

- Web Server에 **.class**파일이 있다.
  - **.class**파일 이란 : **Java Bytecode**를 담은 파일이다.
- 해당 **.class**파일을 네트워크를 통해서 전달해주면 Web Browser에 JVM이 설치 되어 있어서 실행만 하면 된다.
  - 이것이 정확히 **Javascript**가 이런 동작을 하고 있다.
  - **Javascript** 정적 파일을 Web Browser에 보내주게 되면 Web Browser에 설치되어 있는 Javascript Runtime이 즉석에서 인터프리팅(interpreting)해서 실행하게 된다.
    - **Compile** 언어 : Java, C, C++, ... -> **코드가 실행되기 전 Compiler를 거쳐서 기계어로 모두 변환되어 실행되는 프로그래밍 언어**
    - **Interpreter** 언어 : **코드를 바로 실행하는 프로그램 또는 환경(웹 브라우저)** -> Javascript(웹 브라우저), SQL(데이터베이스 언어), Python과 Ruby(자체 프로그래밍 언어)

## Java 코드가 실행되기까지

![compiler](/assets/img/development/server/2023-01-02/compiler.png){:.centered width="90%"}

- **모든 Compiler는 위 그림과 같다.**
- Source Code가 **Compiler의 여러 과정을 거쳐 Assembler**이 된다.
- **Assembler**은 **Assembly Language**를 기계어 형태의 오브젝트 코드로 해석해 주는 컴퓨터 언어 번역 프로그램을 통해 **기계어로 변환** 된다.
- **Compile**에도 Front-end, Back-end가 있다.
- Web에서는 Backend는 크게 바뀌지 않고, **Frontend가 Client 종류에 따라 바뀌게 된다.**
- Compile에서는 반대이다. 
- Compile에서는 Frontend는 바뀌지 않는다.
- 왜냐하면, Frontend가 하는게 개발자가 짠 Source Code를 분석해서 그 의미를 파악하는 것이다.
- **Abstract Syntax Tree가 추상적으로 표현**하고, **Intermediate Code Generator로 번역**하기 때문에 **Frontend는 플랫폼과 아무 관련이 없다.**
- 그런데 **Backend**는 계속 바뀔 수 밖에 없는게 **Intermediate Code Generator 영역의 중앙 계층 표현**을 Assembly Language로 바꿔야 한다.
- 그런데 이 **Assembly Language**라는 것은 운영체제(OS)나 기기에 dependent(의존)하기 때문이다.
- 그래서 **Compiler의 구조**에서 **Backend만 Windows용, Linux용, MacOS용, 등...**이 있는 것이고, 각 **OS는 Frontend만 공유**하는 것이다.
- Compiler는 이러한 **Layer Architecture**로 되어있다.
- C/C++ Compiler는 통째로 Compiler가 해준다.
- 근데 Java에서 Frontend는 **javac**가 해주고, Backend는 **JVM**이 해준다. 결국 하는 일을 분리해 준 것이다.
- 그래서 갖는 장점도 있다.
- C언어는 한 번에 Compile 하고 나면 더 이상 개입할 수 없다.
- 근데, 개발자들은 사전에 다 알 수가 없다. Runtime에서 어떤 일이 벌어질지...
- Runtime에서만 발생하는 굉장히 소중한 정보들이 있고, 그 정보들을 이용해서 최적화를 하는것이 **JIT Compiler**이다. 

# JVM 내부 구조

![jvm](/assets/img/development/server/2023-01-02/jvm.jpeg){:.centered width="90%"}
<div style="text-align: center">[그림출처](https://aljjabaegi.tistory.com/387)</div>

## Runtime Data Areas

![jvm](/assets/img/development/server/2023-01-02/java_jvm_runtime_data_area.png){:.centered width="90%"}
<div style="text-align: center">[그림출처](https://www.devkuma.com/docs/jvm/memory-structure/)</div>

- **JVM이 Java Bytecode를 실행하기 위해 사용하는 메모리 공간**을 **Runtime Data Area** 라고 한다. 즉, **JVM이 Java Bytecode를 실행하는 가상의 기계**이다.

## Per JVM

![per jvm](/assets/img/development/server/2023-01-02/per_jvm.png){:.centered width="90%"}
<div style="text-align: center">[그림출처](https://www.devkuma.com/docs/jvm/memory-structure/)</div>

- **Method영역**과 **Heap영역**은 **모든 Thread가 공유하는 데이터 영역**이다.
- 반면, **Stack영역**과 **PC Register, Native Method Stack 영역**은 **각 Thread마다 생성되는 개별 영역**이다.

## Method/Heap

![method_heap](/assets/img/development/server/2023-01-02/method_heap_area.png){:.centered width="90%"}

### Method Area
- **Class Loader**가 class파일을 읽어왔을때 **클래스에 있는 정보들을 Parsing** 해서 **Method영역에 저장**하는 곳.
- **변수, 메소드, 정적 변수, Bytecode**등을 **저장하는 곳.**

### Heap Area
- 프로그램을 실행하면서 생성한 **모든 객체 Instance를 Heap에 저장**하는 곳.

# Bytecode 실행 예제
