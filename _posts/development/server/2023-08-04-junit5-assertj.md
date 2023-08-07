---
layout: post
title: "[JUnit5] JUnit5 AssertJ"
subtitle: "JUnit5 AssertJ"
category: development
tags: server JUnit5
image:
    path: /assets/img/development/server/2023-08-04/assertj_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# JUnit5 AssertJ
## AssertJ VS Assertions
### JUnit 팀의 AssertJ 라이브러리를 사용 권장
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

![](/assets/img/development/server/2023-08-04/spring_starter_contains_assertj_dependencies.png){:.centered width="80%"}
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
![](/assets/img/development/server/2023-08-04/junit_assertions.png){:.centered width="80%"}
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
![](/assets/img/development/server/2023-08-04/assertj_assertions.png){:.centered width="80%"}
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
![](/assets/img/development/server/2023-08-04/collection_test.png){:.centered width="80%"}
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

```java
import static org.assertj.core.api.Assertions.assertThat;

class JUnitTest {
    @Test
    void 기본_테스트_메서드() {
        // isEqualTo()
        String str1 = new String("hello");
        String str2 = new String("hello");
        assertThat(str1).isEqualTo(str2); // 비교 대상의 내용이 같은지 확인
        // isSameAs()
        String str3 = str1;
        assertThat(str1).isSameAs(str3); // 같은 객체를 참조하고 있는지 확인

        // isNotNull()
        assertThat(str1).isNotNull(); // 객체가 null이 아닌지 확인

        // isNull()
        String str4 = null;
        assertThat(str4).isNull(); // 객체가 null인지 확인

        // isNotEmpty() and isEmpty()
        List<String> emptyList = new ArrayList<>();
        List<String> nonEmptyList = Arrays.asList("apple", "banana");
        assertThat(emptyList).isEmpty(); // 비어있는지 확인
        assertThat(nonEmptyList).isNotEmpty(); // 비어있지 않은지 확인

        // isIn() and isNotIn()
        String fruit1 = "apple";
        String fruit2 = "orange";
        assertThat(fruit1).isIn(nonEmptyList); // "apple"이 리스트에 포함되어 있는지 확인
        assertThat(fruit2).isNotIn(nonEmptyList); // "orange"가 리스트에 포함되어 있지 않은지 확인
    }
}
```

#### AssertJ 기본 검증 메서드 결과
![](/assets/img/development/server/2023-08-04/basic_test.png){:.centered width="80%"}
**Basic Test**
{:.figcaption}

#### AssertJ isIn(), IsNotIn 메서드 결과
![](/assets/img/development/server/2023-08-04/isin_isnotin.png){:.centered width="80%"}
**isIn(), isNotIn Test**
{:.figcaption}

### AssertJ 문자열 검증 메서드

| 메서드                               | 설명                                            |
|-----------------------------------|-----------------------------------------------|
| `contains(값)`                     | 검증대상에 (값)이 **포함**되어있는지 확인한다.                  |
| `containsOnlyOnce(값)`             | 검증대상에 (값)이 딱 **한 번만 포함**되어있는지 확인한다.           |
| `containsOnlyDigits()`            | **숫자만 포함**하는지 검증한다.                           |
| `containsWhitespaces()`           | **공백 문자를 포함**하고 있는지 검증한다.                     |
| `containsOnlyWhitespaces()`       | **공백 문자만 포함**하는지 검증한다.                        |
| `doesNotContain(값)`               | 검증대상의 **공백 문자만 포함**하는지 검증한다.                  |
| `doesNotContainAnyWhitespaces()`  | 검증대상의 **공백 문자를 포함하고 있지 않은지**를 검증한다.           |
| `doesNotContainOnlyWhitespaces()` | 검증대상의 **공백 문자만 포함하고 있지 않은지**를 검증한다.           |
| `doesNotContainPattern(패턴)`       | 검증대상의 **정규 표현식에 일치하는 문자를 포함하고 있지 않은지**를 검증한다. |
| `startsWith(값)`                   | 검증대상의 **시작 값**이 (값)과 동일한지 비교한다.               |
| `endsWith(값)`                     | 검증대상의 **마지막 값**이 (값)과 동일한지 비교한다.              |
| `doesNotStartWith(값)`             | 검증대상의 (값)이 **지정한 문자열로 시작하지 않는지**를 검증한다.       |
| `doesNotEndWith(값)`               | 검증대상의 (값)이 **지정한 문자열로 끝나지 않는지**를 검증한다.        |

```java
import static org.assertj.core.api.Assertions.*;

public class AssertJTest {

    @Test
    void 문자열_테스트() {
        assertThat("Hello, world! Nice to meet you.") // 주어진 "Hello, world! Nice to meet you."라는 문자열은
                .isNotEmpty() // 비어있지 않고
                .contains("Nice") // "Nice"를 포함하고
                .contains("world") // "world"도 포함하고
                .doesNotContain("ZZZ") // "ZZZ"는 포함하지 않으며
                .startsWith("Hell") // "Hell"로 시작하고
                .endsWith("u.") // "u."로 끝나며
                .isEqualTo("Hello, world! Nice to meet you."); // "Hello, world! Nice to meet you."과 일치
    }
}
```

#### AssertJ 문자열 검증 메서드 결과
![](/assets/img/development/server/2023-08-04/string_test.png){:.centered width="80%"}
**String Test**
{:.figcaption}

### AssertJ 숫자 검증 메서드

| 메서드                        | 설명                             |
|----------------------------|--------------------------------|
| `isPositive()`             | 검증대상이 **양수**인지 확인한다.           |
| `isNotPositive()`          | 검증대상이 **양수가 아닌지** 확인한다.        |
| `isNegative()`             | 검증대상이 **음수**인지 확인한다.           |
| `isNotNegative()`          | 검증대상이 **음수가 아닌지** 확인한다.        |
| `isZero()`                 | 검증대상이 **`0`** 인지 확인한다.         |
| `isNotZero()`              | 검증대상이 **`0`** 이 아닌지 확인한다.      |
| `isOne()`                  | 검증대상이 **`1`** 인지 확인한다.         |
| `isGraterThan(값)`          | 검증대상이 값을 **초과**한지 확인한다.        |
| `isLessThan(값)`            | 검증대상이 값보다 **미만**인지 확인한다.       |
| `isGraterThanOrEqualTo(값)` | 검증대상이 값 **이상**인지 확인한다.         |
| `isLessThanOrEqualTo(값)`   | 검증대상이 값 **이하**인지 확인한다.         |
| `isBetween(값1, 값2)`        | `값1`과 `값2` **사이에 포함**되는지 검증한다. |

```java
import static org.assertj.core.api.Assertions.*;

public class AssertJTest {

    @Test
    void 숫자_테스트() {
        assertThat(3.14d) // 주어진 3.14라는 숫자는
                .isPositive() // 양수이고
                .isGreaterThan(3) // 3보다 크며
                .isLessThan(4) // 4보다 작습니다
                .isEqualTo(3, offset(1d)) // 오프셋 1 기준으로 3과 같고
                .isEqualTo(3.1, offset(0.1d)) // 오프셋 0.1 기준으로 3.1과 같으며
                .isEqualTo(3.14); // 오프셋 없이는 3.14와 같습니다
    }
}
```

#### AssertJ 숫자 검증 메서드 결과
![](/assets/img/development/server/2023-08-04/number_test.png){:.centered width="80%"}
**Number Test**
{:.figcaption}

### AssertJ 날짜 검증 메서드

| 메서드                       | 설명                                         |
|---------------------------|--------------------------------------------|
| `isBefore(비교 값)`          | **비교 값 보다 이전**인지 검증한다. `실제 값 < 비교 값`       |
| `isBeforeOrEqualTo(비교 값)` | **비교 값 보다 이전이거나 같은지** 검증한다. `실제 값 <= 비교 값` |
| `isAfter(비교 값)`           | **비교 값 보다 이후**인지 검증한다. `실제 값 > 비교 값`       |
| `isAfterOrEqualTo(비교 값)`  | **비교 값 보다 이후이거나 같은지** 검증한다. `실제 값 >= 비교 값` |

#### AssertJ 날짜 검증 메서드 결과
![](/assets/img/development/server/2023-08-04/compare_test.png){:.centered width="80%"}
**Compare Test**
{:.figcaption}

### as - Fail Message
- `as(String description, Object... args)`를 사용하여 테스트 코드의 실패 메시지를 설정할 수 있다.
- `as`는 검증 문보다 앞에 작성해야 하며, 그렇지 않을 경우 검증 문 이후 호출이 중단됨으로 무시된다.

```java
import static org.assertj.core.api.Assertions.*;

class FailMessageTest {
    
    @Test
    void fail_message_테스트() {
        String str = "JUnit";
        assertThat(str)
                // as는 검증 문보다 앞에 작성해야 하며, 그렇지 않을 경우 검증 문 이후 호출이 중단됨으로 무시된다.
                .as("기대값(Expected) AssertJ와 실제값(Actual) %s이 일치하지 않습니다.", str)
                .isEqualTo("AssertJ");
    }
}
```

#### as - Fail Message 결과
![](/assets/img/development/server/2023-08-04/fail_message.png){:.centered width="80%"}
**Fail Message Test**
{:.figcaption}

### filteredOn 메서드

```java
import static org.assertj.core.api.Assertions.*;

class FilteredOnTest {
    public Member member1, member2, member3;
    public List<Member> members;

    static class Member {
        private String name;
        private int age;
        private MemberRole role;

        public Member(String name, int age, MemberRole role) {
            this.name = name;
            this.age = age;
            this.role = role;
        }

        public String getName() { return name; }
        public int getAge() {return age; }
        public MemberRole getRole() { return role; }
    }

    enum MemberRole { ADMIN, BASIC, VIP }

    @BeforeEach
    public void createMember() {
        member1 = new Member("Kim", 20, MemberRole.ADMIN);
        member2 = new Member("Ahn", 20, MemberRole.BASIC);
        member3 = new Member("Park", 21, MemberRole.VIP);
        members = Lists.list(member1, member2, member3);
    }

    @Test
    public void 체이닝_람다를_사용한_필터_테스트() {
        assertThat(members) // members 컬렉션에서
                .filteredOn(member -> member.getAge() >= 20) // age >= 20 인 객체만 필터링
                .filteredOn(member -> !member.getName().equals("Kim")) // name이 "Kim"과 같지 않은 객체만 필터링
                .filteredOn("role", in(MemberRole.VIP)) // "role"이 MemberRole.VIP인 객체를 필터링
                .containsOnly(member3) // 그 객체가 오직 member3 이며,
                .isNotEmpty(); // 비어있지 않다.
    }
}
```
- 위 테스트 처럼 **체이닝과 람다**를 활용하여 더욱 편리하게 `filteredOn()` 메서드를 사용할 수 있다.

#### AssertJ filteredOn 메서드 결과
![](/assets/img/development/server/2023-08-04/filteredon_test.png){:.centered width="80%"}
**Fail Message Test**
{:.figcaption}

### Extracting 메서드
- `extracting()` 메서드의 파라미터로 `필드명` 을 입력하거나, `메서드 레퍼런스` 도 표현이 가능하다.

```java
import static org.assertj.core.api.Assertions.*;

class FilteredOnTest {
    public Member member1, member2, member3;
    public List<Member> members;

    class Member {
        private String name;
        private int age;
        private MemberRole role;

        public Member(String name, int age, MemberRole role) {
            this.name = name;
            this.age = age;
            this.role = role;
        }

        public String getName() { return name; }
        public int getAge() {return age; }
        public MemberRole getRole() { return role; }
    }

    enum MemberRole { ADMIN, BASIC, VIP }

    @BeforeEach
    public void createMember() {
        member1 = new Member("Kim", 20, MemberRole.ADMIN);
        member2 = new Member("Ahn", 20, MemberRole.BASIC);
        member3 = new Member("Park", 21, MemberRole.VIP);
        members = Lists.list(member1, member2, member3);
    }

    @Test
    public void 메서드_레퍼런스를_이용한_필터_테스트() {
        assertThat(members) // members 컬렉션에서
                .filteredOn(
                        member -> member.getAge() >= 20 // member 객체의 age가 >= 20 인 객체만 필터링
                                && !member.getName().equals("Kim") // member 객체의 name이 "Kim"과 같지 않은 객체만 필터링
                                && member.getRole().equals(MemberRole.VIP) // member 객체의 role이 MemberRole.VIP인 객체를 필터링
                )
                .extracting(
                        Member::getName, // member -> member.getName() => Member 클래스 객체의 getName() 메서드를 참조
                        Member::getAge, // member -> member.getAge()
                        Member::getRole // member -> member.getRole()
                )
                .containsExactly(tuple("Park", 21, MemberRole.VIP)); // containsExactly(): 순서 원소의 개수와 값이 일치해야 한다.
    }
}
```

- 위 `@BeforeEach`를 통해 `@Test` 어노테이션이 붙은 테스트 메서드를 실행하기 전 `createMember()` 메서드를 실행하여 `Member` 객체들을 생성한다.
- 이후 `extracting()` 메서드에서 `람다 표현식`을 **더 간단하게 표현**하는 방법인 **`메서드 레퍼런스`**를 이용하여 `Member` 객체의 메서드를 참조(`Method Reference`)를 이용하여 `.filteredOn()`을 구현한다.
- 마지막 `.containsExactly()` 메서드에 인자로 `tuple()`을 사용하여 각기 다른 인자값들을 받아 테스트 하게 되는데, **이때 `.containsExactly()`는 순서 원소의 개수와 값이 모두 일치해야 한다.**

#### Extracting 메서드 결과
![](/assets/img/development/server/2023-08-04/method_reference_test.png){:.centered width="80%"}
**Fail Message Test**
{:.figcaption}

### assertThatThrownBy() : 예외처리
#### assertThatThrownBy() : 예외처리 에러1

```java
import static org.assertj.core.api.Assertions.assertThatThrownBy;

class AssertThrowsTest {

    @Test
    void assertThrows_테스트() {
        // given
        String input = "abc";

        // when, then : 입력값 범위 밖일 경우 StringIndexOutOfBoundsException 발생
        assertThatThrownBy(() -> input.charAt(input.length()))
                .isInstanceOf(StringIndexOutOfBoundsException.class)
                .hasMessageContaining("String index out of length");
    }
}
```

- 에러 메세지의 값이 `"String index out of range"` 가 포함되어 있어야 하는 테스트이지만 `"String index out of length"` 여서 테스트가 실패하게 된다.

#### assertThatThrownBy() - 예외처리 에러1 결과
![](/assets/img/development/server/2023-08-04/assertthrows_error_1.png){:.centered width="80%"}
**AssertThatThrownBy Fail Test1**
{:.figcaption}

#### assertThatThrownBy() - 예외처리 에러2

```java
import static org.assertj.core.api.Assertions.assertThatThrownBy;

class AssertThrowsTest {

    @Test
    void assertThrows_테스트() {
        // given
        String input = "abc";

        // when, then : 입력값 범위 밖일 경우 StringIndexOutOfBoundsException 발생
        assertThatThrownBy(() -> input.charAt(input.length()))
                .isInstanceOf(StringIndexOutOfBoundsException.class)
                .hasMessageContaining(String.valueOf(4));
    }
}
```

- 에러 메세지의 값이 `"String index out of range: 3"` 가 포함되어 있어야 하는 테스트이지만 `"4"` 여서 테스트가 실패하게 된다.

#### assertThatThrownBy() - 예외처리 에러2 결과
![](/assets/img/development/server/2023-08-04/assertthrows_error_2.png){:.centered width="80%"}
**AssertThatThrownBy Fail Test2**
{:.figcaption}

#### assertThatThrownBy() - 예외처리 성공

```java
import static org.assertj.core.api.Assertions.assertThatThrownBy;

class AssertThrowsTest {

    @Test
    void assertThrows_테스트() {
        // given
        String input = "abc";

        // when, then : 입력값 범위 밖일 경우 StringIndexOutOfBoundsException 발생
        assertThatThrownBy(() -> input.charAt(input.length()))
                .isInstanceOf(StringIndexOutOfBoundsException.class)
                .hasMessageContaining("String index out of range")
                .hasMessageContaining(String.valueOf(input.length()));
    }
}
```

- 에러 메세지의 값이 `"String index out of range: 3"` 가 포함되어야하는 값에 각각 `"String index out of range"` 과 `"3"`이 포함되어 테스트가 성공하게 된다.

#### assertThatThrownBy() - 예외처리 결과
![](/assets/img/development/server/2023-08-04/filteredon_test.png){:.centered width="80%"}
**AssertThatThrownBy Test**
{:.figcaption}

Back to [[JUnit5] JUnit5](./2023-06-01-junit5.md#결과-출력){:.heading.flip-title}
{:.read-more}

# Reference
- [AssertJ](https://assertj.github.io/doc/){:target="_blank"}
- [JUnit5에서 타사 라이브러리(AssertJ, Hamcrest, Truth) 추천](https://junit.org/junit5/docs/current/user-guide/#writing-tests-assertions-third-party){:target="_blank"}
- [Collection Test](https://kkoon9.tistory.com/135){:target="_blank"}
- [AssertJ 소개](https://www.daleseo.com/assertj/){:target="_blank"}

<!-- Links -->