---
layout: post
title: "[JUnit5] JUnit5 어노테이션 ver1"
subtitle: "JUnit @Annotations"
category: development
tags: server JUnit5 Annotations
image:
    path: /assets/img/development/server/2023-07-04/junit5_annotation_ver1_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# JUnit5 Annotations ver1

## @Test

- 테스트 메서드라는 것을 나타내는 어노테이션
- `JUnit4`와 다르게 어떠한 속성도 선언하지 않는다.
- 또한, 클래스와 메서드는 더 이상 가시성(`visibility`)이 `public`으로 선언되지 않아도 동작한다.(물론 기존과 같이 `public`, `protected`등으로 선언해도 동일하게 동작한다.)

```java
class JUnitTest() {
    // JUnit4
    @Test(exptected = Exception.class)
    public void 상품주문() throws Exception {
        // ...
    }
    
    // JUnit5
    @Test
    void 상품주문() {
        // ...
    }
}
```

### @Test 결과
![](/assets/img/development/server/2023-07-04/annotations_test.png){:.centered width="80%"}
**@Test Annotation**
{:.figcaption}

## @DisplayName
- **`Test Results`**에 나오는 테스트 클래스, 메소드 이름을 정할 수 있는 어노테이션.

| 파라미터명   | 타입       | 설명                               |
|---------|----------|----------------------------------|
| `value` | `String` | 테스트명 (한글, 특수문자, 이모지 등) 다양한 문자 가능 |

```java
public class DisplayNameTest {
    
    @Test
    void 기본() {
    }
    
    @DisplayName("한글 메서드")
    @Test
    void 한글() {
    }
    
    @DisplayName("english")
    @Test
    void english() {
    }
    
    @DisplayName("특수문자 \uD83D\uDE00")
    @Test
    void 특수문자() {
    }
}
```

### @DisplayName 결과
![](/assets/img/development/server/2023-07-04/annotations_displayname.png){:.centered width="80%"}
**@DisplayName Annotation**
{:.figcaption}

## @DisplayNameGeneration
- `@DisplayName` 처럼 별도의 이름을 주는 것이 아닌 **코딩한 클래스**, **메소드 이름**을 이용해 **변형**시키는 어노테이션

| 파라미터명   | 타입                                      | 설명                                 |
|---------|-----------------------------------------|------------------------------------|
| `value` | `Class<? extends DisplayNameGenerator>` | 정의된 `DisplayNameGenerator` 중 하나 사용 |

- 위 파라미터 값은 `DisplayNameGenerator` 클래스에 사용할 수 있는 방법이 내부 클래스로 정의되어있다.

| 클래스명                  | 설명                                                                                            |
|-----------------------|-----------------------------------------------------------------------------------------------|
| `Standard`            | 기존 클래스, 메소드 명을 사용 **(`기본값`)**                                                                 |
| `Simple`              | 괄호`()`를 **제외**                                                                                |
| `ReplaceUnderscores`  | _(`underscore`) 를 **공백**으로 바꿈                                                                 |
| `IndicativeSentences` | 클래스명 + 구분자(", ") + 메소드명으로 바꿈.<br> (`@IndicativeSentencesGeneration`를 이용해서 구분자를 커스텀하게 변경할 수 있다 |


```java
public class DisplayNameGenerationTest {
    @Nested
    @DisplayNameGeneration(DisplayNameGenerator.IndicativeSentences.class) // inner class, 메서드명 출력
    class IndicativeSentences {
        @Test
        void test_display_name_generation() {
        }
    }

    @Nested
    @DisplayNameGeneration(DisplayNameGenerator.ReplaceUnderscores.class) // _(underscore) 를 공백으로 바꿈
    class ReplaceUnderscores {
        @Test
        void test_display_name_generation() {
        }
    }

    @Nested
    @DisplayNameGeneration(DisplayNameGenerator.Simple.class) // 괄호()를 제외 
    class Simple {
        @Test
        void test_display_name_generation() {
        }
    }

    @Nested
    @DisplayNameGeneration(DisplayNameGenerator.Standard.class) // 기본값. 메서드 이름 그대로 출력
    class Standard {
        @Test
        void test_display_name_generation() {
        }
    }
}
```

### @DisplayNameGeneration 결과
![](/assets/img/development/server/2023-07-04/annotations_displaynamegeneration.png){:.centered width="80%"}
**@DisplayNameGeneration Annotation**
{:.figcaption}

## @ParameterizedTest
- 파라미터를 넣어서 데스트를 **반복적**으로 실행할 수 있게 해주는 어노테이션.
- 반드시 파라미터를 넘겨야하며, **`@ValueSource`** 혹은 **`@CsvSource`** 등을 붙여서 파라미터를 같이 던져주어야한다.

### @ValueSource
- 여러 번의 파라미터를 던질 수 있도록 하는 어노테이션 중 하나이다. 단 **하나**의 타입만 던질 수 있으며 사용법은 아래와 같다.
- `@ValueSource(ints = {10, 20, 30})` **[O]** , `@ValueSource(ints = {10, 20, 30}, strings = {"문자1", "문자2", "문자3")` **[X]**
- **단 하나의 타입의 파라미터만 가능**하므로 두 번째 케이스는 에러가 날 것이다.

```java
class ParameterizedTestTest {

    private Set<Integer> numbers;

    @BeforeEach
    void setup() {
        numbers = new HashSet<>();
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);
    }

    @DisplayName("일반_반복테스트")
    @Test
    void 일반_반복테스트() {
        assertThat(numbers.contains(1)).isTrue();
        assertThat(numbers.contains(2)).isTrue();
        assertThat(numbers.contains(3)).isTrue();
        for (int num : numbers) {
            System.out.println("일반 반복 테스트 : " + num);
        }
    }

    @DisplayName("반복_테스트")
    @ParameterizedTest
    @ValueSource(ints = {1, 2, 3})
    void 반복_테스트(int intArg) {
        assertThat(numbers.contains(intArg)).isTrue();
    }
}
```

#### 일반 반복 테스트 결과

![](/assets/img/development/server/2023-07-04/annotations_parameterizetest_normal.png){:.centered width="80%"}
**일반 반복 테스트**
{:.figcaption}

#### @ValueSource 반복 테스트 결과

![](/assets/img/development/server/2023-07-04/annotations_parameterizetest_valuesource.png){:.centered width="80%"}
**@ValueSource 반복 테스트**
{:.figcaption}

### @CsvSource
- `,`로 분리된 문자열을 테스트 메서드의 파라미터로 넣어주는 어노테이션이다.
- `delimiterString`이라는 옵션으로 문자열을 분리할 다른 구분자를 고를 수 있다.
- `' '`로 넣는 것은 `empty`가 들어오고 아에 비워두면 `null`로 들어오게 된다.

```java
public class CvsSourceTest {
    @ParameterizedTest
    @CsvSource({ "apple, banana" })
    void cvsSourceTest1(String fruit1, String fruit2) {
    }

    @ParameterizedTest
    @CsvSource({ "apple, 'lemon, lime'" })
    void cvsSourceTest2(String fruit1, String fruit2) {
    }

    @ParameterizedTest
    @CsvSource({ "apple, ''" })
    void cvsSourceTest3(String fruit1, String fruit2) {
    }

    @ParameterizedTest
    @CsvSource({ "apple, " })
    void cvsSourceTest4(String fruit1, String fruit2) {
    }

    @ParameterizedTest
    @CsvSource(value = { "apple, banana, NIL" }, nullValues = "NIL")
    void cvsSourceTest5(String fruit1, String fruit2) {
    }
}
```

#### @CsvSource 테스트 결과

![](/assets/img/development/server/2023-07-04/annotations_csvsource.png){:.centered width="80%"}
**@CsvSource 테스트**
{:.figcaption}


## @RepeatedTest
- `@RepeatedTest`메서드를 어노테이션을 지정하면 `value`값에 지정된 횟수만큼 테스트가 **반복**된다.

```java
public class RepeatTest {
    @RepeatedTest(5)
    @DisplayName("반복 테스트1")
    void repeatedTest1() {
    }

    @RepeatedTest(value = 5, name = "반복테스트2 중 {currentRepetition} of {totalRepetitions}")
    @DisplayName("반복 테스트2")
    void repeatedTest2() {
    }
}
```

### @RepeatedTest 테스트 결과

![](/assets/img/development/server/2023-07-04/annotations_repeattest.png){:.centered width="80%"}
**@RepeatedTest 테스트**
{:.figcaption}

## @BeforeEach
- `@BeforeEach` 어노테이션이 달린 메서드가 현재 클래스의 **각각** `@Test`, `@RepeatedTest`, `@ParameterizedTest` 또는 `@TestFactory` 어노테이션이 달린 메서드 *전에* 실행된다.

## @AfterEach
- `@AfterEach` 어노테이션이 달린 메서드가 현재 클래스의 **각각** `@Test`, `@RepeatedTest`, `@ParameterizedTest` 또는 `@TestFactory` 어노테이션이 달린 메서드 *이후*에 실행된다. 

## @BeforeAll
- `@BeforeAll` 어노테이션이 달린 메서드가 현재 클래스의 **모든** `@Test`, `@RepeatedTest`, `@ParameterizedTest` 및 `@TestFactory` 어노테이션이 달린 메서드보다 *먼저* 실행된다.

## @AfterAll
- `@AfterAll` 어노테이션이 달린 메서드가 현재 클래스의 **모든** `@Test`, `@RepeatedTest`, `@ParameterizedTest`, `@TestFactory` 어노테이션이 달린 메서드 *다음에* 실행된다.

## @BeforeAll vs @BeforeEach 와 @AfterAll vs @AfterEach

- 두 개 어노테이션 모두 하나 혹은 여러 개의 테스트 조건을 `setup`할 때 사용하지만 하나의 테스트 클래스 안에 여러 개의 테스트가 있는 경우 `@BeforeEach`와 `@AfterEach`는 여러번 실행되지만, `@BeforeAll`와 `@AfterAll`은 딱 한 번만 실행된다.
- `@BeforeEach`와 `@AfterEach`는 `@Test` 어노테이션이 붙은 메서드가 실행이 되기 전(`@BeforeEach`)과 후(`@AfterEach`) 실행이 된다.
- `@BeforeAll`과 `@AfterAll`은 해당 클래스 내에 처음 시작(`@BeforeAll`)과 맨 마지막 끝(`@AfterAll`)에 실행이 된다.

### JUnit5 LifeCycle

```java
class JUnit5LifeCycleTest {

    @BeforeAll
    static void setup() {
        System.out.println("@BeforeAll - executes once before all test methods in this class");
    }
  
    @BeforeEach
    void init() {
        System.out.println("@BeforeEach - executes before each test method in this class");
    }
  
    @AfterEach
    void tearDown() {
        System.out.println("@AfterEach - executed after each test method.");
    }
  
    @AfterAll
    static void tearDownAll() {
        System.out.println("@AfterAll - executed after all test methods.");
    }
  
    @Test
    void successTest1() {
        System.out.println("executes successTest1");
        assumeTrue("abc".contains("a"));
    }
  
    @Test
    void successTest2() {
        System.out.println("executes successTest2");
        assumeTrue("abc".contains("b"));
    }
    
    // 출력 ---------------------------------------------------------
    /*
      @BeforeAll - executes once before all test methods in this class
      @BeforeEach - executes before each test method in this class
      executes successTest1
      @AfterEach - executed after each test method.
      @BeforeEach - executes before each test method in this class
      executes successTest2
      @AfterEach - executed after each test method.
      @AfterAll - executed after all test methods.
    */
}
```

#### 결과 출력

![](/assets/img/development/server/2023-07-04/junit_lifecycle_example.png){:.centered width="80%"}
**JUnit5 LifeCycle Example 1**
{:.figcaption}

Back to [[JUnit5] JUnit5](./2023-06-01-junit5.md){:.heading.flip-title}
{:.read-more}

# Reference
- [JUnit Annotations](https://junit.org/junit5/docs/current/user-guide/#writing-tests-annotations){:target="_blank"}
- [[Java] JUnit 5 사용법 (3) - Test Results 테스트명 바꾸기 (@DisplayName, @DisplayNameGeneration, @IndicativeSentencesGeneration)](https://effortguy.tistory.com/200){:target="_blank"}
