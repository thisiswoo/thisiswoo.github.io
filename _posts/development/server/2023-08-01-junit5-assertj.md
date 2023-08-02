---
layout: post
title: "[JUnit5] JUnit5 AssertJ"
subtitle: "JUnit5 AssertJ"
category: development
tags: server JUnit5
image:
    path: /assets/img/development/server/2023-08-01/assertj_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# JUnit5 AssertJ

## AssertJ VS Assertions
### JUnit의 AssertJ 라이브러리를 사용 권장
> "`JUnit Jupiter`에서 제공하는 `Assertions` 기능만으로도 많은 테스트 시나리오에 충분하지만, 더 강력한 성능과 매처와 같은 추가 기능이 필요하거나 필요한 경우가 있습니다. **이러한 경우 JUnit 팀은 `AssertJ`, `Hamcrest`, `Truth` 등과 같은 타사 어설션 라이브러리를 사용할 것을 권장합니다.** 따라서 개발자는 원하는 `Assertions` 라이브러리를 자유롭게 사용할 수 있습니다. - JUnit5 Doc"

### AssertJ의 장점
- 메소드 체이닝을 지원하기 때문에 좀 더 깔끔하고 읽기 쉬운 테스트 코드를 작성할 수 있다.
- 개발자가 테스트를 하면서 필요하다고 상상할 수 있는 거의 모든 메소드를 제공한다.

### 라이브러리 의존성 설정
- SpringBoot를 사용하여 프로젝트를 생성하게되면 `spring-boot-starter-test` 라이브러리가 자동으로 들어가게 된다.

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter'
    // ...
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

![](/assets/img/development/server/2023-08-01/spring_starter_contains_assertj_dependencies.png){:.centered width="80%"}
**Dependencies**
{:.figcaption}

### AssertJ VS Assertions 차이점
#### junit.jupiter의 Assertions
```java
import static org.assertj.core.api.Assertions.assertThat;

class AssertionsTest {
    @Test
    void assertions_테스트() {
        String str1 = "문자열";
        String str2 = "문자열";
        System.out.println("str1 = " + str1.hashCode());
        System.out.println("str2 = " + str2.hashCode());

        Assertions.assertEquals(str1, str2);
    }
}
```

##### junit.jupiter의 Assertions 결과
![](/assets/img/development/server/2023-08-01/junit_assertions.png){:.centered width="80%"}
**JUnit Assertions**
{:.figcaption}

#### assertj.core의 Assertions
```java
import static org.assertj.core.api.Assertions.*;

public class AssertJTest {

    @Test
    void assertj_테스트() {
        String str1 = "문자열";
        String str2 = "문자열";
        System.out.println("str1 = " + str1.hashCode());
        System.out.println("str2 = " + str2.hashCode());

        assertThat(str1).isEqualTo(str2);
    }
}
```

##### assertj.core의 Assertions 결과
![](/assets/img/development/server/2023-08-01/assertj_assertions.png){:.centered width="80%"}
**AssertJ Assertions**
{:.figcaption}

- `actual(실제 값)` 을 `expexted(기대 값)`이 동일한지(`isEqualTo()`) 확인하는 메서드이다.
- 아직 까지는 크게 차이나보이진 않는다.
- 하지만 `Collection Test`를 보면 크게 다르다는걸 느끼게 될 것이다.  

#### Collection Test
```java
class CollectionTest {

    @Test
    public void junit_assertions_테스트() {
        ArrayList<Student> characters = new ArrayList<>();
        Student spring = new Student("Spring");
        Student jpa = new Student("Jpa");
        Student kubernetes = new Student("Kubernetes");
        characters.add(spring);
        characters.add(jpa);
        characters.add(kubernetes);

        List<Student> expected = new ArrayList<>();
        expected.add(spring);
        expected.add(jpa);
        List<Student> filteredList = characters.stream()
                .filter((character) -> character.name.contains("p"))
                .collect(Collectors.toList());

        Assertions.assertEquals(expected,filteredList);
    }

    static class Student {
        private String name;
        public Student(String name) { this.name = name; }
    }
}
```

```java
class CollectionTest {

    @Test
    public void assertj_테스트() {
        ArrayList<Student> characters = new ArrayList<>();
        Student spring = new Student("Spring");
        Student jpa = new Student("Jpa");
        Student kubernetes = new Student("Kubernetes");

        characters.add(spring);
        characters.add(jpa);
        characters.add(kubernetes);

        assertThat(characters)
                .filteredOn(character -> character.name.contains("p"))
                .containsOnly(spring, jpa);
    }

    static class Student {
        private String name;
        public Student(String name) { this.name = name; }
    }
}
```

- 위 두 테스트 코드를 보게 되면 차이가 많이난다.
- `AssertJ`를 통해 `filteredOn()` 메서드의 람다식을 이용하여 배열 `character.name.contains("p")`를 이용하여 `p`가 들어간 것들을 찾는다
- `containsOnly(spring, jpa)`를 이용하여 `spring`변수와 `jpa`변수에 `p`가 포함되어있으면 테스트 성공하게 된다.
- 이러한 기능들 때문에 `AssertJ` 라이브러리를 많이 사용하게 된다.

##### Collection Test 결과
![](/assets/img/development/server/2023-08-01/collection_test.png){:.centered width="80%"}
**Collection Test**
{:.figcaption}

### AssertJ 기본 검증 메서드

| 메서드            | 설명                                |
|----------------|-----------------------------------|
| `isEqualTo(값)` | 검증대상과 **동일한 값**인지 비교한다.           |
| `isSameAs(값)`  | 검증대상과 값을 **`==`** 비교한다.           |
| `isNotNull()`  | 검증대상이 **`Not Null`** 인지 확인한다.     |
| `isNull()`     | 검증대상이 **`Null`** 인지 확인한다.         |
| `isNotEmpty()` | 검증대상이 **`Not Empty`** 인지 확인한다.    |
| `isEmpty()`    | 검증대상이 **`Empty`** 인지 확인한다.        |
| `isIn()`       | 검증대상이 **값 목록에 포함**되어 있는지 검증한다.    |
| `isNotIn()`    | 검증대상이 **값 목록에 포함되어 있지 않는지** 검증한다. |

- `isIn()`과 `isNotIn()`의 값 목록은 `가변 인자(변하는 인자의 개수)` 로 주거나 `List`와 타입을 이용해서 전달한다.

#### AssertJ 기본 검증 메서드 결과
![](/assets/img/development/server/2023-08-01/collection_test.png){:.centered width="80%"}
**Collection Test**
{:.figcaption}

### AssertJ 문자열 검증 메서드

| 메서드            | 설명                               |
|----------------|----------------------------------|
| `contains(값)`  | 검증대상에 (값)이 **포함**되어있는지 확인한다.     |
| `startsWith(값)` | 검증대상의 **시작 값**이 (값)과 동일한지 비교한다.  |
| `endsWith(값)`   | 검증대상의 **마지막 값**이 (값)과 동일한지 비교한다. |

### AssertJ 숫자 검증 메서드
| 메서드                        | 설명                      |
|----------------------------|-------------------------|
| `isPositive()`             | 검증대상이 **양수**인지 확인한다.    |
| `isNegative()`             | 검증대상이 **음수**인지 확인한다.    |
| `isZero()`                 | 검증대상이 **`0`** 인지 확인한다.  |
| `isGraterThan(값)`          | 검증대상이 값을 **초과**한지 확인한다. |
| `isLessThan(값)`            | 검증대상이 값보다 **미만**인지 확인한다. |
| `isGraterThanOrEqualTo(값)` | 검증대상이 값 **이상**인지 확인한다.  |
| `isLessThanOrEqualTo(값)`   | 검증대상이 값 **이하**인지 확인한다.  |

### AssertJ filteredOn 메서드







Back to [[JUnit5] JUnit5](./2023-06-01-junit5.md#결과-출력){:.heading.flip-title}
{:.read-more}

# Reference
- [AssertJ](https://assertj.github.io/doc/){:target="_blank"}
- [JUnit5에서 타사 라이브러리(AssertJ, Hamcrest, Truth) 추천](https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions-third-party){:target="_blank"}
- [Collection Test](https://kkoon9.tistory.com/135){:target="_blank"}


<!-- Links -->
