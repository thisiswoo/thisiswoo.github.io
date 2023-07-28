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
  `Build and Run` 에서 `Method` 선택되어 있는 부분을 `Tags` 선택

  ![](/assets/img/development/server/2023-07-26/settings_tag_annotation_by_intellij_3.png){:.centered width="80%"}
  `Tags` 선택 후 내가 테스트할 메서드 (`@Tag("키워드")`, 나의 예제에선 `@Tag("top")`)의 태그 이름을 넣어준다

  ![](/assets/img/development/server/2023-07-26/annotations_tag.png){:.centered width="80%"}
  **@Tag Annotation**
  {:.figcaption}

### Custom @Tag
- 커스텀 태그는 직접 특정한 태그를 만들어 사용하는 방식. 이렇게 미리 커스텀 태그를 만들고, 여러번 사용하면 코드의 중복과 태그의 오타가 발생할 확률을 줄일 수 있다.

#### 1. Custom @Tag 만들기
```java
@Retention(RetentionPolicy.RUNTIME) // 해당 어노테이션이 붙은 메서드가 런타임 될 동안까지 실행
@Target(value = {ElementType.TYPE, ElementType.METHOD}) // 메서드에서 사용할 것이이라고 지정
@Tag("TopLine")
public @interface TopLine {
}
```

#### Custom @Tag TopLine 세팅
![](/assets/img/development/server/2023-07-26/settings_custom_tag_annotation_by_intellij_top.png){:.centered width="80%"}
**Custom @Tag Annotation Settings**
{:.figcaption}

#### Custom @Tag 결과 1
![](/assets/img/development/server/2023-07-26/annotations_custom_tag_top.png){:.centered width="80%"}
**Custom @Tag Annotation Result**
{:.figcaption}

#### 나머지

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(value = {ElementType.TYPE, ElementType.METHOD})
@Tag("JungleLine")
public @interface JungleLine { }

@Retention(RetentionPolicy.RUNTIME) 
@Target(value = {ElementType.TYPE, ElementType.METHOD})
@Tag("MidLine")
public @interface MidLine { }

@Retention(RetentionPolicy.RUNTIME)
@Target(value = {ElementType.TYPE, ElementType.METHOD})
@Tag("AdLine")
public @interface AdLine { }

@Retention(RetentionPolicy.RUNTIME)
@Target(value = {ElementType.TYPE, ElementType.METHOD})
@Tag("SupportLine")
public @interface SupportLine { }
```

#### 나머지 결과
![](/assets/img/development/server/2023-07-26/settings_custom_tag_annotation_by_intellij_jungle.png){:.centered width="80%"}
**Custom @Tag Annotation Settings**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/annotations_custom_tag_jungle.png){:.centered width="80%"}
**Custom @Tag Annotation Result**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/settings_custom_tag_annotation_by_intellij_mid.png){:.centered width="80%"}
**Custom @Tag Annotation Settings**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/annotations_custom_tag_mid.png){:.centered width="80%"}
**Custom @Tag Annotation Result**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/settings_custom_tag_annotation_by_intellij_ad.png){:.centered width="80%"}
**Custom @Tag Annotation Settings**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/annotations_custom_tag_ad.png){:.centered width="80%"}
**Custom @Tag Annotation Result**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/settings_custom_tag_annotation_by_intellij_support.png){:.centered width="80%"}
**Custom @Tag Annotation Settings**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/annotations_custom_tag_support.png){:.centered width="80%"}
**Custom @Tag Annotation Result**
{:.figcaption}

```java
class TagExampleTest {

    @Test
    @TopLine
    void top_line() { }

    @Test
    @JungleLine
    void jungle_line() { }

    @Test
    @MidLine
    void mid_line() { }

    @Test
    @AdLine
    void ad_line() { }

    @Test
    @SupportLine
    void support_line() { }
}
```

## @Timeout
- `@Timeout` 어노테이션을 사용하면 `@Test`, `@TestFactory`, `@TestTemplate` 또는 라이프사이클 메서드의 실행 시간이 지정된 기간을 **초과**하는 경우 **실패하도록 선언**할 수 있다. 
- 기간의 시간 단위는 기본값이 `second`(초)이지만 사용자가 따로 시간을 지정할 수 있다.

```java
class TimeoutExampleTest {

    @BeforeEach
    @Timeout(5)
    void setUp() {
        // 실행 시간이 5초를 초과하면 실패
    }

    @Test
    @Timeout(value = 500, unit = TimeUnit.MILLISECONDS)
    void failsIfExecutionTimeExceeds500Milliseconds() {
        // 실행 시간이 500밀리초(0.5초)를 초과하면 실패
    }

    @Test
    @Timeout(value = 500, unit = TimeUnit.MILLISECONDS, threadMode = Timeout.ThreadMode.SEPARATE_THREAD)
    void failsIfExecutionTimeExceeds500MillisecondsInSeparateThread() {
        // 실행 시간이 500밀리초(0.5초)를 초과하면 실패하고 테스트 코드가 별도의 스레드에서 실행된다.
    }
}
```

- 테스트 클래스 내의 모든 테스트 메서드와 모든 `@Nested` 클래스에 동일한 시간 제한을 적용하려면 클래스 수준에서 `@Timeout` 어노테이션을 선언하면 된다. 그러면 특정 메서드나 `@Nested` 클래스에 대한 `@Timeout` 어노테이션으로 재정의하지 않는 한 해당 클래스 및 해당 `@Nested` 클래스 내의 모든 테스트, 테스트 팩토리 및 테스트 템플릿 메서드에 적용된다. 클래스 수준에서 선언된 `@Timeout` 어노테이션은 라이프사이클 메서드에는 적용되지 않는다는 점에 유의.
- `TestFactory` 메서드에 `@Timeout`을 선언하면 팩토리 메서드가 지정된 기간 내에 반환되는지 확인하지만 팩토리에서 생성된 각 개별 `DynamicTest`의 실행 시간은 확인하지 않는다. 이를 위해 `assertTimeout()` 또는 `assertTimeoutPreemptively()`를 사용하면 된다.
- `Timeout`이 `@TestTemplate` 메서드에 있는 경우(예: `@RepeatedTest` 또는 `@ParameterizedTest`), 각 호출에 지정된 시간 제한이 적용된다.

### @Timeout 예제 코드
```java
class TimeoutExampleTest {

    @BeforeAll
    static void testBeforeAll() {
        System.out.println("BeforeAll");
    }
    
    @AfterAll
    static void testAfterAll() {
        System.out.println("AfterAll");
    }
    
    @Timeout(1) // 1초 지나면 테스트 실패
    @Test
    void timeoutException() throws InterruptedException {
        System.out.println("timeoutException");
        assertEquals("timeout", "timeout");
        Thread.sleep(2000); // 2초동안 스레드 sleep
    }
}
```

#### @Timeout 예제 코드 결과
![](/assets/img/development/server/2023-07-26/annotations_timeout.png){:.centered width="80%"}
**@Timeout Annotation Result**
{:.figcaption}

#### @Timeout 구성 파라미터 값 예시

| 매개변수 값  | @어노테이션 설정                                   |
|---------|---------------------------------------------|
| 42      | `@Timeout(42)`                              |
| 42 ns   | `@Timeout(value = 42, unit = NANOSECONDS)`  |
| 42 μs   | `@Timeout(value = 42, unit = MICROSECONDS)` |
| 42 ms   | `@Timeout(value = 42, unit = MILLISECONDS)` |
| 42 s    | `@Timeout(value = 42, unit = SECONDS)`      |
| 42 m    | `@Timeout(value = 42, unit = MINUTES)`      |
| 42 h    | `@Timeout(value = 42, unit = HOURS)`        |
| 42 d    | `@Timeout(value = 42, unit = DAYS)`         |

## @ExtendWith
- 단위 테스트에 공통적으로 사용할 확장 기능을 선언해주는 역할을 한다. 
- 인자로 확장할 `Extension`을 명시하면 된다. 
- `SpringExtension.class` 또는 `MockitoExtension.class`를 많이 사용한다. 
- `Spring Test Context` 프레임워크와 `Junit5`와 통합해 사용할 때는 `SpringExtension.class`를 사용한다. 
- `JUniit5`와 `Mockito`를 연동해 테스트를 진행할 경우에는 `MockitoExtension.class`를 사용한다. 

```java
@ExtendWith(SpringExtension.class)
class ExtendWithExampleTest { }

@ExtendWith(MockitoExtension.class)
class ExtendWithExampleTest { }
```

### @ExtendWith - 기본 확장 모델 1
```java
public class ExtensionExample implements BeforeEachCallback, AfterEachCallback {
    @Override
    public void beforeEach(final ExtensionContext context) throws Exception {
        System.out.println("ExtensionExample.beforeEach");
    }

    @Override
    public void afterEach(final ExtensionContext context) throws Exception {
        System.out.println("ExtensionExample.afterEach");
    }
}
```

- 여기서 `Extension` 자체는 마커 인터페이스이어서 메서드가 정의되어 있지 않다.

> 마커 인터페이스(marker interface)란, 일반적인 인터페이스와 동일하지만, 아무 메서드도 선언하지 않은 인터페이스이다. 자바의 대표적인 마커 인터페이스로는 `Serializable`, `Cloneable`, `EventListener`가 있다. 대부분의 경우 마커 인터페이스를 단순한 타입 체크를 하기 위해 사용한다.

- `Extension` 상속한 `BeforeEachCallback` 이나 `AfterEachCallback`와 같은 확장 포인트 마다 정의된 서브 인터페이스를 구현한다.
  - `BeforeEachCallback`는 각 테스트 전에 콜백되는 `beforeEach() 메소드가 정의된다.
  - `AfterEachCallback`는 각 테스트 후에 콜백되는 `afterEach()` 메소드가 정의된다.

- 확장 기능을 구현한 클래스를 실제로 테스트에서 사용하는 방법 중 하나로 `@ExtendWith` 어노테이션을 사용하는 방법이 있다.
- 확장 기능을 적용할 위치에 `@ExtendWith` 어노테이션을 설정하고, `value`에 적용할 확장 프로그램의 Class 객체를 지정한다.

```java
@ExtendWith(ExtensionExample.class)
class ExtensionExampleTest {
    @Test
    void test1() {
        System.out.println("  test1()");
    }

    @Test
    void test2() {
        System.out.println("  test2()");
    }
}
```

#### @ExtendWith - 기본 확장 모델 1 결과
![](/assets/img/development/server/2023-07-26/basic_extedwith_1.png){:.centered width="80%"}
**@ExtendWith Annotation Result**
{:.figcaption}

### @ExtendWith - 기본 확장 모델 2
```java
// @ExtendWith(ExtensionExample.class)
class ExtensionExampleTest {
    
    @Test
    @ExtendWith(ExtensionExample.class)
    void test1() {
        System.out.println("  test1()");
    }

    @Test
    void test2() {
        System.out.println("  test2()");
    }
}
```

#### @ExtendWith - 기본 확장 모델 2 결과
![](/assets/img/development/server/2023-07-26/basic_extedwith_2.png){:.centered width="80%"}
**@ExtendWith Annotation Result**
{:.figcaption}

## @RegisterExtension
### 확장 기능을 절차적으로 등록
- `@ExtendWith`을 사용하는 방법의 경우 확장 기능 클래스의 조정은 기본적으로 정적이다.
  - 확장 기능을 구현한 클래스의 인스턴스는 `Jupiter` 에 의해 안에서 생성된다.
  - 그러기 때문에, 확장 기능 클래스의 인스턴스에 대해서 세세한 조정은 기본적으로 할 수 없다.
- 반면에 `@RegisterExtension`을 사용하면 확장 클래스의 조정을 동적으로 지정할 수 있다.

```java
public class RegisterExtensionExample implements BeforeEachCallback, BeforeAllCallback {
    private final String name;

    public RegisterExtensionExample(final String name) { this.name = name; }

    @Override
    public void beforeAll(final ExtensionContext context) throws Exception {
        System.out.println("[" + this.name + "] beforeAll()");
    }

    @Override
    public void beforeEach(final ExtensionContext context) throws Exception {
        System.out.println("[" + this.name + "] beforeEach()");
    }
}
```

```java
public class RegisterExtensionExampleTest {

    @RegisterExtension
    static RegisterExtensionExample classField = new RegisterExtensionExample("classField");

    @RegisterExtension
    RegisterExtensionExample instanceField = new RegisterExtensionExample("instanceField");

    @BeforeAll
    static void beforeAll() { System.out.println("beforeAll()"); }

    @BeforeEach
    void beforeEach() { System.out.println("beforeEach()"); }

    @Test
    void test1() { System.out.println("test1()"); }
}
```

- 사용할 확장 클래스의 인스턴스를 확장을 사용하려는 테스트 클래스의 필드(`static` or `인스턴스`)로 선언한다.
  - 이 필드를 `@RegisterExtension`으로 어노테이션을 지정하면 해당 필드에 설정된 인스턴스를 확장으로 등록할 수 있다.
  - 필드에 대한 인스턴스 설정은 모든 프로그램에서 작성할 수 있으므로 자유롭게 조정된 인스턴스를 사용할 수 있다.
- `static`으로 선언한 필드를 사용한 경우는 모든 확장 프로그램을 사용할 수 있다.
- 인스턴스 필드를 사용하는 경우 `BeforeAllCallback`와 같은 클래스 수준 확장 기능과 `TestInstancePostProcessor`와 같은 인스턴스 수준 확장 기능을 사용할 수 없다. 
  - 구현하더라도 무시된다.
  - `BeforeEachCallback`와 같은 메소드 레벨 확장 기능을 사용할 수 있다.

### @RegisterExtension 결과
![](/assets/img/development/server/2023-07-26/register_extension.png){:.centered width="80%"}
**@ExtendWith Annotation Result**
{:.figcaption}

## @TempDir
- `TempDirectory extension`은 테스트클래스 안에 있는 독립적인 테스트 또는 모든 테스트에 대해 임시 디렉토리를 생성하고 정리를 할 때 사용한다. 
- 이 기능을 사용하려면 접근 제어자가 `private`이 아닌 `java.nio.file.Path` 나 `java.io.File` 필드에 `@TempDir` 어노테이션을 붙이거나, 파라미터에 붙여준다.

```java
class TempDirExampleTest {
    @TempDir
    File tempFolderFile;

    @Test
    void test1() {
        System.out.println(tempFolderFile.getAbsolutePath());
    }

    @Test
    void test2(@TempDir Path tempFolderPath) {
        System.out.println(tempFolderFile.getAbsolutePath());
    }
}
```

### @TempDir 결과
![](/assets/img/development/server/2023-07-26/temp_dir.png){:.centered width="80%"}
**@ExtendWith Annotation Result**
{:.figcaption}


## Dynamic Test
- 일반적으로 사용되는 `@Test`를 이용한 테스트는 **컴파일 시점에 완전히 지정**되는 **정적 테스트**. 이는 동적인 기능에 대한 기본 테스트 형태를 제공하지만, 그 표현이 **컴파일 시점에 제한된다는 한계**를 가지고 있다.
- 이에 비해 `Dynamic Test`는 `Runtime` 동안에 테스트가 **생성되고 수행**되기 때문에, **프로그램이 수행되는 도중에도 동작을 변경할 수 있는 특징**이 있다.

## @TestFactory
Continue with [[JUnit5] Dynamic Test @TestFactory](./2023-07-10-junit5-dynamictest-testfactory.md){:.heading.flip-title}
{:.read-more}

## @TestTemplate
- `@Test` 메서드와 달리 `@TestTemplate`은 그 자체가 테스트 케이스가 아니라 테스트 케이스에 대한 템플릿이다. 따라서 등록된 프로바이더가 반환하는 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계되었다.
- 하나 이상의 프로바이더와 함께 사용해야하며, 그렇지 않으면 실행이 실패한다.

- `@TestTemplate` 메서드는 일반 테스트 케이스가 아니라 테스트 케이스를 위한 템플릿이다.
- 따라서 등록된 프로바이더가 반환하는 호출 컨텍스트의 수에 따라 여러 번 호출되도록 설계되었다.
- 따라서 등록된 `TestTemplateInvocationContextProvider` 확장과 함께 사용해야 한다.
- `@TestTemplate` 메서드의 각 호출은 동일한 수명 주기 콜백 및 확장을 완벽하게 지원하면서 일반 `@Test` 메서드의 실행과 같이 동작한다.

```java
public class TestTemplateTest {
    final List<String> fruits = Arrays.asList("apple", "banana", "lemon");

    @TestTemplate
    @ExtendWith(MyTestTemplateInvocationContextProvider.class)
    void testTemplate(String fruit) {
        assertTrue(fruits.contains(fruit));
    }

    public class MyTestTemplateInvocationContextProvider implements TestTemplateInvocationContextProvider {
        @Override
        public boolean supportsTestTemplate(ExtensionContext context) { return true; }

        @Override
        public Stream<TestTemplateInvocationContext> provideTestTemplateInvocationContexts(ExtensionContext context) {
            return Stream.of(invocationContext("apple"), invocationContext("banana"));
        }

        private TestTemplateInvocationContext invocationContext(String parameter) {
            return new TestTemplateInvocationContext() {
                @Override
                public String getDisplayName(int invocationIndex) { return parameter; }

                @Override
                public List<Extension> getAdditionalExtensions() {
                    return Collections.singletonList(new ParameterResolver() {
                        @Override
                        public boolean supportsParameter(ParameterContext parameterContext, ExtensionContext extensionContext) {
                            return parameterContext.getParameter().getType().equals(String.class);
                        }

                        @Override
                        public Object resolveParameter(ParameterContext parameterContext, ExtensionContext extensionContext) {
                            return parameter;
                        }
                    });
                }
            };
        }
    }

    @Test
    void testTemplateTest() {
        testTemplate("apple");
    }
}
```

### @TestTemplate 결과
![](/assets/img/development/server/2023-07-26/test_template_1.png){:.centered width="80%"}
**@TestTemplate Annotation Result**
{:.figcaption}

## @TestClassOrder
- 테스트 클래스에서 **`@Nested` 테스트 클래스에 대한 테스트 클래스 실행 순서를 구성**하는 데 사용된다.

```java
@TestClassOrder(ClassOrderer.OrderAnnotation.class)
class TestClassOrderExampleTest {
    @Nested
    @Order(1)
    class PrimaryTests {
        @Test
        void test1() {}
    }

    @Nested
    @Order(2)
    class SecondaryTests {
        @Test
        void test2() {}
    }
}
```

### ## @TestClassOrder 결과
![](/assets/img/development/server/2023-07-26/testclassorder_1.png){:.centered width="80%"}
**@TestClassOrder Annotation Result**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/testclassorder_2.png){:.centered width="80%"}
**@TestClassOrder Annotation Result**
{:.figcaption}

## @TestMethodOrder
- 테스트 클래스에 대한 **테스트 메서드 실행 순서를 구성**하는 데 사용된다.

```java
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
class TestMethodOrderExampleTest {
    @Test
    @Order(1)
    void test1() {}

    @Test
    @Order(2)
    void test2() {}

    @Test
    @Order(3)
    void test3() {}
}
```

```java
@TestMethodOrder(MethodOrderer.OrderAnnotation.class)
class TestMethodOrderExampleTest {
    @Test
    @Order(2)
    void test1() {}

    @Test
    @Order(3)
    void test2() {}

    @Test
    @Order(1)
    void test3() {}
}
```

### ## @TestMethodOrder 결과
![](/assets/img/development/server/2023-07-26/testmethodorder_1.png){:.centered width="80%"}
**@TestMethodOrder Annotation Result**
{:.figcaption}

![](/assets/img/development/server/2023-07-26/testmethodorder_2.png){:.centered width="80%"}
**@TestMethodOrder Annotation Result**
{:.figcaption}

## @TestInstance
- `@TestInstance`는 어노테이션이 지정된 테스트 클래스 또는 테스트 인터페이스에 대한 테스트 인스턴스의 라이프사이클을 구성하는데 사용된다.
- 테스트 클래스에 `@TestInstance`가 명시적으로 지정되지 않은 경우, 기본적으로 **`PER_METHOD`** 를 사용한다.

| enum                   | 설명                                                                                 |
|------------------------|------------------------------------------------------------------------------------|
| `Lifecycle.PER_METHOD` | 각 `@Test 메서드`, `@TestFactory 메서드` 또는 `@TestTemplate 메서드`에 대해 **새 테스트 인스턴스가 생성**된다. |
| `Lifecycle.PER_CLASS`  | 테스트가 아니라 테스트 클래스 별로 테스트 인스턴스가 생성된다.                                                |

```java
class TestInstanceExampleTest {
    int number = 0;

    @Test
    void add1() {
        number++;
        System.out.println("add1() number = " + number);
        System.out.println(this);
    }

    @Test
    void add2() {
        number += 2;
        System.out.println("add2() number = " + number);
        System.out.println(this);
    }
}
```

### @TestInstance(Lifecycle.PER_METHOD) 결과
![](/assets/img/development/server/2023-07-26/test_instance_1.png){:.centered width="80%"}
**@TestInstance(Lifecycle.PER_METHOD) Annotation Result**
{:.figcaption}

- 위 결과와 같이 `@TestInstance`를 선언하지 않으면 `@TestInstance(Lifecycle.PER_METHOD)`로 지정되어 각 메서드가 다른 인스턴스인것을 확인할 수 있다.
- 또한 각기 다른 인스턴스이기 때문에 number의 값이 1과 2가 나온것을 확인 할 수 있다.
- 위와 다르게 같은 인스턴스를 사용하게 하려면 `@TestInstance(Lifecycle.PER_CLASS)` 를 지정해주면 된다.


```java
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class TestInstanceExampleTest {
    int number = 0;

    @Test
    void add1() {
        number++;
        System.out.println("add1() number = " + number);
        System.out.println(this);
    }

    @Test
    void add2() {
        number += 2;
        System.out.println("add2() number = " + number);
        System.out.println(this);
    }
}
```

### @TestInstance(Lifecycle.PER_CLASS) 결과
![](/assets/img/development/server/2023-07-26/test_instance_2.png){:.centered width="80%"}
**@TestInstance(Lifecycle.PER_CLASS) Annotation Result**
{:.figcaption}

### Lifecycle.PER_CLASS 는 언제 사용할까?
- 이 어노테이션은 모든 테스트 전에 클래스를 인스턴스화하는 데 비용이 많이 드는 경우에 유용하다. 예를 들어 데이터베이스 연결을 설정하거나 대용량 파일을 로드할 수 있다.
- 상태 공유는 일반적으로 단위 테스트에서는 안티 패턴이지만 통합 테스트에서는 유용할 수 있다. 
- 클래스별 수명 주기는 의도적으로 상태를 공유하는 순차 테스트를 지원하며, 이는 특히 테스트 중인 시스템을 올바른 상태로 만드는 속도가 느린 경우, 이후 테스트에서 이전 테스트의 단계를 반복해야 하는 것을 피하기 위해 필요할 수 있다.

Back to [[JUnit5] JUnit5 어노테이션 ver1](./2023-07-04-junit5-annotations-frequently-used.md){:.heading.flip-title}
{:.read-more}

Back to [[JUnit5] JUnit5](./2023-06-01-junit5.md){:.heading.flip-title}
{:.read-more}

# Reference
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/){:target="_blank"}
- [JUnit5 확장 모델](https://www.devkuma.com/docs/junit5/extension/){:target="_blank"}
- [JUnit5 - @TestInstance에 대해 알아보자](https://devs0n.tistory.com/40){:target="_blank"}

<!-- Links -->
[테스트 인스턴스 라이프사이클]: https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle
[Uses of @TestInstance(PER_CLASS)]: https://www.baeldung.com/junit-testinstance-annotation#test-instance-uses