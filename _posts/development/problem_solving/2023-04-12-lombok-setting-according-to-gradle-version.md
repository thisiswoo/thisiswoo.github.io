---
layout: post
title: "[Gradle] Gradle 버전에 따른 Lombok 설정 방법"
subtitle: "Gradle 버전에 따른 Lombok 설정 방법"
category: development
tags: problem_solving gradle lombok 설정
image:
    path: /assets/img/development/problem_solving/2023-04-12/lombok_settings_according_to_gradle_version_cover.png
---

> “Gradle 버전에 따른 Lombok 설정 방법”

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# 문제
- 동욱 님의 책 [스프링 부트와 AWS로 혼자 구현하는 웹 서비스]{:target="_blank"} 책을 보며 테스트 하던 중 아래 사진과 같은 문제가 발생하였다.
![](/assets/img/development/problem_solving/2023-04-12/gradle_version_under5.png){:.centered width="100%"}

- `variable '변수명' not initialized in the default constructor` 라는 오류가 발생하였다.

## 문제 상황 재현 코드
### 테스트 코드
```java
// file: "HelloResponseDTOTest.java"
public class HelloResponseDTOTest {
    @Test
    public void 롬복_기능_테스트() {
        // given
        String name = "test";
        int amount = 1000;

        // when
        HelloResponseDTO dto = new HelloResponseDTO(name, amount);

        // then
        assertThat(dto.getName()).isEqualTo(name);
        assertThat(dto.getAmount()).isEqualTo(amount);
    }
}
```

- 코드에 이상은 없었다.
- 문제는 `gradle`의 버전에 따라 `lombok` 설정이 다른 것이었다. 

### 문제의 build.gradle 설정
```gradle
// file: "build.gradle"
dependencies {
   // Gradle 5.x 미만
   implementation 'org.projectlombok:lombok'
}
```
- 위 코드처럼 내가 설정을 해놓았다.
- 문제는 프로젝트의 `Gradle` 버전과 `lombok` 버전이 맞지 않아 생기는 오류라는 것을 확인하였다.
- 나의 `gradle` 버전을 확인하지 않고 설정 해버렸던 것이었다.

### 나의 gradle 버전 확인
![](/assets/img/development/problem_solving/2023-04-12/gradle_version.png){:.centered width="100%"}
```shell
.gradlew --version
```
- 나의 `Gradle Version`은 `7.6.1`
- 내가 `build.gradle` 설정에서 설정해 준 버전은 `Gradle 5.x 미만`의 설정이어서 `variable '변수명' not initialized in the default constructor`라는 오류가 발생했던 것이었다.

# 해결
## 해결 build.gradle 재설정
```gradle
// file: "build.gradle"
dependencies {
   // Gradle 5.x 이상
   compileOnly 'org.projectlombok:lombok'
   annotationProcessor 'org.projectlombok:lombok'
}
```

- 위 `5.x`대의 설정 방법으로 설정해주니 해결 되었다.
![](/assets/img/development/problem_solving/2023-04-12/gradle_version_upper5.png){:.centered width="100%"}

# 결론
- 테스트 작업 시 `variable '변수명' not initialized in the default constructor`라는 해당 오류 발생 시 해당 프로젝트의 `Gradle` 버전 확인
- `Gradle` 버전(`5.x 미만`, `5.x 이상`)에 따라 `lombok` 설정 후 테스트 재진행

# Reference
- [[스프링/Spring] Lombok: variable not initialized in the default constructor](https://deeplify.dev/back-end/spring/lombok-required-args-constructor-initialize-error){:target="_blank"}

<!-- Links -->
[스프링 부트와 AWS로 혼자 구현하는 웹 서비스]: https://product.kyobobook.co.kr/detail/S000001019679