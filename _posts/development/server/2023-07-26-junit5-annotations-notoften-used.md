---
layout: post
title: "[JUnit5] JUnit5 어노테이션2"
subtitle: "JUnit @Annotations"
category: development
tags: server JUnit5 Annotations
image:
    path: /assets/img/development/server/2023-07-26/junit5_annotation_ver2_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# JUnit5 Annotations ver2

## @Disabled
- **테스트를 하고 싶지 않은** 클래스나 메서드에 붙이는 어노테이션
- `JUnit4`의 `@Ignore`과 유사

```java
class DisabledExampleTest {
    
    @Test
    @Disabled("문제가 해결될 때까지 테스트 중단")
    void 테스트1() {
        System.out.println("테스트1");
    }
    
    @Test
    void 테스트2() {
        System.out.println("테스트2");
    }
}
```

### @Disabled 결과
![](/assets/img/development/server/2023-07-26/annotations_disabled.png){:.centered width="80%"}
**@Disabled Annotation**
{:.figcaption}

## @Nested
- 정적(`static`)이 아닌 **중첩 클래스**를 나타내는 어노테이션
- `Java8` 부터 `Java15` 까지는 테스트 인스턴스 라이프사이클을 사용하지 않는 한 `@BeforeAll` 과 `@AfterAll`의 메서드를 `@Nested` 테스트 클래스안에서 직접 사용할 수 없다.
- 단, `Java16` 부터는 [테스트 인스턴스 라이프사이클]{:target="_blank"} 중에 하나를 사용하여 `@Nested` 테스트 클래스 에서 `@BeforeAll` 과 `@AfterAll`의 메서드를 정적(`static`) 으로 선언 할 수 있다.

```java
public class NestedExampleTest {

    @Nested
    class 중첩_클래스_1 {
        @Test
        public void 성공() { }

        @Test
        public void 실패() { }
    }

    @Nested
    class 중첩_클래스_2 {

        @Nested
        class 테스트1 {
            @Test
            public void 성공() { }

            @Test
            public void 실패() { }
        }

        @Nested
        class 테스트2 {
            @Test
            public void 성공() { }

            @Test
            public void 실패() { }
        }
    }
}
```

### @Nested 결과
![](/assets/img/development/server/2023-07-26/annotations_nested.png){:.centered width="80%"}
**@Nested Annotation**
{:.figcaption}

## @Tag
- 테스트 그룹을 만들고 원하는 테스트 그룹만 테스트를 실행할 수 있는 어노테이션.
- 클래스 또는 메서드 레벨에 사용되며, 해당 어노테이션은 클래스 수준에서는 상속 되지만, 메서드 수준에서는 상속되지 않는다.

```java
class TagExampleTest {

    @Test
    @Tag("top")
    void top_line() { }

    @Test
    @Tag("jungle")
    void jungle_line() { }

    @Test
    @Tag("mid")
    void mid_line() { }

    @Test
    @Tag("ad")
    void ad_line() { }

    @Test
    @Tag("support")
    void support_line() { }
}
```

### @Tag 결과
- 테스트 하기 전에 intelliJ IDE에서는 내가 원하는 `@Tag("xxx"")`에 지정해준 테스트만을 하기 위해선 설정을 해주어야 한다.
    

  ![](/assets/img/development/server/2023-07-26/settings_tag_annotation_by_intellij_1.png){:.centered width="80%"}
  `intelliJ IDE` 우측 상단 `Edit Configurations...` 클릭

  ![](/assets/img/development/server/2023-07-26/settings_tag_annotation_by_intellij_2.png){:.centered width="80%"}
  `Build and Run` 에서 `Method` 선택되어 있는 부분을 `Tags` 선

  ![](/assets/img/development/server/2023-07-26/settings_tag_annotation_by_intellij_3.png){:.centered width="80%"}
  `Tags` 선택 후 내가 테스트할 메서드 (`@Tag("키워드")`, 나의 예제에선 `@Tag("top")`)의 태그 이름을 넣어준다

  ![](/assets/img/development/server/2023-07-26/annotations_tag.png){:.centered width="80%"}
  **@Tag Annotation**
  {:.figcaption}


## @Timeout
## @ExtendWith
## @RegisterExtension
## @TempDir

## Dynamic Test
- 일반적으로 사용되는 `@Test`를 이용한 테스트는 **컴파일 시점에 완전히 지정**되는 **정적 테스트**. 이는 동적인 기능에 대한 기본 테스트 형태를 제공하지만, 그 표현이 **컴파일 시점에 제한된다는 한계**를 가지고 있다.
- 이에 비해 `Dynamic Test`는 `Runtime` 동안에 테스트가 **생성되고 수행**되기 때문에, **프로그램이 수행되는 도중에도 동작을 변경할 수 있는 특징**이 있다.

## @TestFactory
Continue with [[JUnit5] Dynamic Test @TestFactory](./2023-07-04-junit5-dynamictest-testfactory.md){:.heading.flip-title}
{:.read-more}

## @TestTemplate
- `@Test` 메서드와 달리 `@TestTemplate`은 그 자체가 테스트 케이스가 아니라 테스트 케이스에 대한 템플릿이다. 따라서 등록된 프로바이더가 반환하는 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계되었다.
- 하나 이상의 프로바이더와 함께 사용해야하며, 그렇지 않으면 실행이 실패한다.

- `@TestTemplate` 메서드는 일반 테스트 케이스가 아니라 테스트 케이스를 위한 템플릿이다.
- 따라서 등록된 프로바이더가 반환하는 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계되었다.
- 따라서 등록된 `TestTemplateInvocationContextProvider` 확장과 함께 사용해야 한다.
- `@TestTemplate` 메서드의 각 호출은 동일한 수명 주기 콜백 및 확장을 완벽하게 지원하면서 일반 `@Test` 메서드의 실행과 같이 동작한다.

### TestTemplateInvocationContextProvider

## @TestClassOrder
## @TestMethodOrder
## @TestInstance


Back to [[JUnit5] JUnit5](./2023-06-01-junit5.md){:.heading.flip-title}
{:.read-more}

# Reference



<!-- Links -->
[테스트 인스턴스 라이프사이클]: https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle