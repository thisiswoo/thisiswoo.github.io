# 문제 해결

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

## QueryDSL에서 DTO의 필드 값이 Enum타입 일 때 조회 방법
### 문제 

- QueryDSL로 해방 필드 'samgyeopsal'을 조회하고 싶은데 어떻게 조회할까?

```java
public enum Menu {
    SAMGYEOPSAL (12000, "samgyeopsal"),
    SHUSI (15000, "shusi"),
}
```

```java
public class DTO {
    private String restaurant;
    @Enumerated(EnumType.STRING)
    private Menu menu;  
}
```


## DB String 값을 filed의 enum 타입으로 바꿀 순 없을까?
