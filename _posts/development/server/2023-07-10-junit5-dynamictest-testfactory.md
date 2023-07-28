---
layout: post
title: "[JUnit5] @TestFactory"
subtitle: "JUnit Dynamic Test @TestFactory"
category: development
tags: server JUnit5 Annotations
image:
    path: /assets/img/development/server/2023-07-10/junit5_testfactory_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# JUnit5 Annotations
## Dynamic Test
> 'Dynamic Test`는 Runtime 시점에 생성'
- 일반적으로 사용되는 `@Test`를 이용한 테스트는 **컴파일 시점에 완전히 지정**되는 **정적 테스트**. 이는 동적인 기능에 대한 기본 테스트 형태를 제공하지만, 그 표현이 **컴파일 시점에 제한된다는 한계**를 가지고 있다.
- 이에 비해 `Dynamic Test`는 `Runtime` 동안에 테스트가 **생성되고 수행**되기 때문에, **프로그램이 수행되는 도중에도 동작을 변경할 수 있는 특징**이 있다.

### @TestFactory
- `@TestFactory`는 테스트 케이스를 **동적**으로 생성할 수 있다.
- `@Test` 메서드와 달리 `@TestFactory`는 테스트 케이스 자체가 아니라 `테스트 케이스`를 위한 `Factory`이다.
- `@TestFactory` 메서드는 `private` 또는 `static`이어서는 안도 된다.
- 또한 `Stream`, `Collection`, `Iterable`, `Iterator`, 또는 `DynamicNode 인스턴스`의 `배열`을 **반환**해야 한다.
- 컴파일 시점에 유효하지 않은 반환 유형을 감지할 수 없기 때문에, 다른 형태로 반환하면 `JUnitException`이 발생한다는 특징이 있다.

> "규모가 큰 프로젝트에서는 각각의 기능에 따라 담당자가 나누어져 있기 때문에 이러한 시나리오 테스트보다는 단위 테스트가 더 필요하고 적합할 것이다. 또한 시나리오 테스트를 하고싶어도 다른 사람이 짠 코드를 같이 테스트 해야하기 때문에 여러가지 걸림돌이 있을 것이다. 하지만 규모가 작은 프로젝트의 경우 개발 인원이 적고, 한 사람이 여러가지 기능을 맡을 것이기 때문에 각 기능을 쪼개서 테스트하는 단위 테스트보다 시나리오 테스트가 더 적합할 수 있다."

```java
public class TestFactoryExample {
    private final Calculator calculator = new Calculator();

    // JUnitException 발생
    @TestFactory // @TestFactory 어노테이션을 사용할 때,@TestFactory
    List<String> dynamicTestsWithInvalidReturnType() {
        return Arrays.asList("Hello");
    }
    
    @TestFactory // Collection<DynamicTest>를 반환하는 @TestFactory
    Collection<DynamicTest> dynamicTestsFromCollection() {
        return Arrays.asList(
                dynamicTest("1st dynamic test", () -> assertTrue(isPalindrome("madam"))),
                dynamicTest("2nd dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        );
    }

    @TestFactory // Iterable<DynamicTest>를 반환하고, Arrays.asList()를 사용하여 테스트 케이스를 생성
    Iterable<DynamicTest> dynamicTestsFromIterable() {
        return Arrays.asList(
                dynamicTest("3rd dynamic test", () -> assertTrue(isPalindrome("madam"))),
                dynamicTest("4th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        );
    }

    @TestFactory // Iterator<DynamicTest>를 반환하며, Arrays.asList().iterator()를 사용하여 테스트 케이스를 생성
    Iterator<DynamicTest> dynamicTestsFromIterator() {
        return Arrays.asList(
                dynamicTest("5th dynamic test", () -> assertTrue(isPalindrome("madam"))),
                dynamicTest("6th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        ).iterator();
    }

    @TestFactory // DynamicTest[]를 반환하고, 테스트 케이스를 배열로 생성
    DynamicTest[] dynamicTestsFromArray() {
        return new DynamicTest[] {
                dynamicTest("7th dynamic test", () -> assertTrue(isPalindrome("madam"))),
                dynamicTest("8th dynamic test", () -> assertEquals(4, calculator.multiply(2, 2)))
        };
    }

    @TestFactory // 특정 문자열에 대해 isPalindrome 검사를 수행하는 동적 테스트를 반환하는 Stream<DynamicTest>를 생성
    Stream<DynamicTest> dynamicTestsFromStream() {
        return Stream.of("racecar", "radar", "mom", "dad")
                .map(text -> dynamicTest(text, () -> assertTrue(isPalindrome(text))));
    }

    @TestFactory // 처음 10개의 짝수 정수에 대한 동적 테스트를 생성하는 Stream<DynamicTest>를 반환
    Stream<DynamicTest> dynamicTestsFromIntStream() {
        // 처음 10개의 짝수 정수에 대한 테스트를 생성
        return IntStream.iterate(0, n -> n + 2)
                .limit(10)
                .mapToObj(n -> dynamicTest("test" + n, () -> assertTrue(n % 2 == 0)));
    }

    @TestFactory // 무작위로 생성된 정수들에 대해 동적 테스트를 생성하는 Stream<DynamicTest>를 반환
    Stream<DynamicTest> generateRandomNumberOfTestsFromIterator() {
        // 0에서 100 사이의 임의의 양의 정수를 생성
        // 7로 균등하게 나누어지는 숫자를 만듦
        Iterator<Integer> inputGenerator = new Iterator<Integer>() {
            Random random = new Random();
            int current;

            @Override
            public boolean hasNext() {
                current = random.nextInt(100);
                return current % 7 != 0;
            }

            @Override
            public Integer next() {
                return current;
            }
        };

        // 입력:5, 입력:37, 입력:85 등과 같은 표시 이름을 생성
        Function<Integer, String> displayNameGenerator = (input) -> "input:" + input;

        // 현재 입력 값을 기준으로 테스트를 실행
        ThrowingConsumer<Integer> testExecutor = (input) -> assertTrue(input % 7 != 0);

        // 동적 테스트 스트림을 반환
        return DynamicTest.stream(inputGenerator, displayNameGenerator, testExecutor);
    }

    @TestFactory // 주어진 문자열이 회문인지 확인하는 동적 테스트를 반환하는 Stream<DynamicTest>를 생성
    Stream<DynamicTest> dynamicTestsFromStreamFactoryMethod() {
        // 확인 할 회문(e.g) 기러기, 토마토, 등등)을 Stream으로 저장 
        Stream<String> inputStream = Stream.of("racecar", "radar", "mom", "dad");

        // 다음과 같은 표시 이름 생성 : racecar는 회문(e.g) 기러기, 토마토, 등등)이다
        Function<String, String> displayNameGenerator = text -> text + " is a palindrome";

        // 현재 입력 값을 기준으로 테스트를 실행
        ThrowingConsumer<String> testExecutor = text -> assertTrue(isPalindrome(text));

        // 동적 테스트 스트림을 반환
        return DynamicTest.stream(inputStream, displayNameGenerator, testExecutor);
    }

    @TestFactory // 이름이 지정된 동적 테스트를 반환하는 Stream<DynamicTest>를 생성
    Stream<DynamicTest> dynamicTestsFromStreamFactoryMethodWithNames() {
        // 확인 할 회문(e.g) 기러기, 토마토, 등등)을 Stream으로 저장
        Stream<Named<String>> inputStream = Stream.of(
                named("racecar is a palindrome", "racecar"),
                named("radar is also a palindrome", "radar"),
                named("mom also seems to be a palindrome", "mom"),
                named("dad is yet another palindrome", "dad")
        );

        // 동적 테스트 스트림을 반환
        return DynamicTest.stream(inputStream, text -> assertTrue(isPalindrome(text)));
    }

    @TestFactory // DynamicNode를 반환하는 동적 테스트를 생성
    Stream<DynamicNode> dynamicTestsWithContainers() {
        // 여기서는 "A", "B", "C" 에 대한 동적 컨테이너를 생성하며, 이 컨테이너 안에는 문자열 관련 성질을 검사하는 다른 동적 테스트들이 포함
        return Stream.of("A", "B", "C")
                .map(input -> dynamicContainer("Container " + input, Stream.of(
                        dynamicTest("not null", () -> assertNotNull(input)),
                        dynamicContainer("properties", Stream.of(
                                dynamicTest("length > 0", () -> assertTrue(input.length() > 0)),
                                dynamicTest("not empty", () -> assertFalse(input.isEmpty()))
                        ))
                )));
    }

    @TestFactory // 단일 동적 테스트 DynamicNode를 반환하며, "pop" 문자열이 회문(e.g) 기러기, 토마토, 등등)인지 확인
    DynamicNode dynamicNodeSingleTest() {
        return dynamicTest("'pop' is a palindrome", () -> assertTrue(isPalindrome("pop")));
    }

    @TestFactory // 하나의 동적 컨테이너 DynamicNode를 반환하며, 다양한 문자열에 대해 회문 검사하는 동적 테스트들이 포함
    DynamicNode dynamicNodeSingleContainer() {
        return dynamicContainer("palindromes",
                Stream.of("racecar", "radar", "mom", "dad")
                        .map(text -> dynamicTest(text, () -> assertTrue(isPalindrome(text)))));
    }

    // 주어진 문자열이 회문인지 확인하는 로직을 구현한 것입니다. 단어가 거꾸로 읽어도 동일한 문자열인 경우, 회문으로 판단
    private static boolean isPalindrome(String str) {
        // 원래 str의 역순을 저장하기 위해 빈 문자열 초기화
        String rev = "";

        // 답변에 대한 새 boolean 변수 초기화
        boolean ans = false;
        // 역순으로 문자열 저장
        for (int i = str.length() - 1; i >= 0; i--) {
            rev = rev + str.charAt(i);
        }
        // 두 문자열이 같은지 확인
        if (str.equals(rev)) {
            ans = true;
        }
        return ans;
    } 
    
}
```

### @TestFactory 테스트 결과

![](/assets/img/development/server/2023-07-10/annotations_testfactory.png){:.centered width="80%"}
**@TestFactory 테스트**결과
{:.figcaption}

### dynamicTestsWithInvalidReturnType() Error 로그
```
org.junit.platform.commons.JUnitException: @TestFactory method [java.util.List<java.lang.String> com.example.junit5example.junit5test.annotations.dynamicTest.TestFactoryTest.dynamicTestsWithInvalidReturnType()] must return a single org.junit.jupiter.api.DynamicNode or a Stream, Collection, Iterable, Iterator, or array of org.junit.jupiter.api.DynamicNode.
```

#### Error Log 해설
- 위 코드에서 발생하는 `JUnitException`은 `@TestFactory` 어노테이션을 사용하여 작성된 메소드(`dynamicTestsWithInvalidReturnType()`)의 리턴 타입이 유효하지 않기 때문이다.
- `@TestFactory` 어노테이션을 사용할 때, 메소드의 리턴 타입은 반드시 `Collection`, `Iterable`, `Iterator`, `Stream` 중 하나의 인터페이스를 구현해야 하며, 원소는 `DynamicNode` 타입이어야 한다.
- 즉, 리턴 타입이 `Collection`, `Iterable`, `Iterator`, `Stream` 중 하나가 아니라 `List<String>` 타입으로 리턴되어 `JUnitException`이 발생한 것이다.

Back to [[JUnit5] JUnit5 어노테이션](./2023-07-04-junit5-annotations-frequently-used.md){:.heading.flip-title}
{:.read-more}

# Reference
[Dynamic Test](https://junit.org/junit5/docs/current/user-guide/#writing-tests-dynamic-tests){:target="_blank"}