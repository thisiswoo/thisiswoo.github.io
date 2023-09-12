---
layout: post
title: "[JPA] QueryDSL에서 MySQL group_concat 함수 등록과 사용법"
subtitle: "JPA QueryDSL group_concat"
category: development
tags: server JPA
image:
  path: /assets/img/development/server/2023-09-04_jpa-groupconcat/jpa_group_concat_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# MySQL group_concat
- `JPA QueryDSL`에서 **`group_concat()`** 함수 사용법

## MySQL group_concat 함수 란?
- **`GROUP_CONCAT`** 은 `MySQL`에서 제공하는 [집계 함수] 중 하나로, **선택한 row(열)의 값을 연결하여 하나의 문자열로 반환** 한다. 
- 기본적으로 이 함수는 쉼표(,)를 사용하여 값들을 연결하지만, `SEPARATOR` 키워드를 사용하여 다른 문자 또는 문자열을 구분자로 지정할 수 있다.

## MySQL group_concat 기본 사용법
### GROUP_CONCAT() 문법
```text
GROUP_CONCAT(
    [DISTINCT] expression [,expression ...]
    [ORDER BY {unsigned_integer | col_name | expr}
    [ASC | DESC] [,col_name ...]]
    [SEPARATOR str_val]
)
```

- **`expression`** : GROUP_CONCAT 함수가 적용될 열 또는 식.
- **`ORDER BY`** : 결과를 정렬하는 데 사용되며, 필요에 따라 생략 가능.
- **`ASC | DESC`** : 정렬 순서를 지정하며, 필요에 따라 생략할 수 있다
- **`SEPARATOR`** : 값들을 연결할 때 사용할 구분자를 지정. 기본값은 쉼표(,).

```mysql
SELECT 
    student_name,
    GROUP_CONCAT(
        DISTINCT test_score ORDER BY test_score DESC SEPARATOR ' '
    )
FROM student
GROUP BY student_name;
```

### GROUP_CONCAT() 예제
```mysql
CREATE TABLE test (
    id INT,
    value VARCHAR(255)
);

INSERT INTO test (id, value) VALUES (1, 'Apple');
INSERT INTO test (id, value) VALUES (1, 'Banana');
INSERT INTO test (id, value) VALUES (2, 'Cherry');
INSERT INTO test (id, value) VALUES (2, 'Date');

SELECT id,
       GROUP_CONCAT(value ORDER BY value ASC SEPARATOR ', ')
FROM test
GROUP BY id;
```

### GROUP_CONCAT() 예제 결과
```shell
1  Apple,Banana
2  Cherry,Dates
```

## JPA QueryDSL에서 group_concat() 적용하기
### 문제 발생

> "**`Hibernate`** 는 특정 데이터베이스에 종속되지 않고 `객체지향`스럽게 사용할 수 있도록 `추상화`해주어 **특정 DB에 종속된 함수(예, `MySQL`의 `group_concat`)는 제공하지 않는다.**<br>때문에 `Hibernate`에서 제공하지 않고 특정 DB에서 제공하는 함수를 사용하려면 **`Dialect를 커스텀`** 하여 SQL 함수를 등록해주어야한다."

### CustomMySQLDialect 파일 생성
- `package com.xxxxx.xxxxx.xxx.config;` 에 `CustomMySQLDialect.class` 파일을 생성
  - 각 프로젝트에서 `config` 폴더에 생성하면 무난할듯 하다.

```java
package com.xxxxx.xxxxx.xxx.config;

import org.hibernate.dialect.MySQL57Dialect;
import org.hibernate.dialect.function.StandardSQLFunction;
import org.hibernate.type.StandardBasicTypes;

public class CustomMySQLDialect extends MySQL57Dialect {
    public CustomMySQLDialect() {
        super();
        // JPA QueryDSL에서 group_concat 함수를 사용할 수 있도록 등록
        registerFunction("group_concat", new StandardSQLFunction("group_concat", StandardBasicTypes.STRING));
    }
}
```

#### 주의!
![](/assets/img/development/server/2023-09-04_jpa-groupconcat/mysql_dialect_deprecated.png){:.centered width="80%"}

- 위 사진처럼 `MySQL5InnoDBDialect` 와 `MySQL57InnoDBDialect` 이 **`deprecated(사용하지 않는)`** 되어있다.
- 그래서 난 위 코드처럼 `MySQL57Dialect`로 extends 해주었다.

### application.yml 또는 application.properties 설정
#### application.yml 설정
```yaml
spring:
  jpa:
    database: mysql
    database-platform: com.xxxxx.xxxxx.xxx.config.CustomMySQLDialect # CustomMySQLDialect 파일 위치 설정해줘야 group_concat() 함수를 사용할 수 있다.
```

#### application.properties 설정
```properties
spring.jpa.database=mysql
# CustomMySQLDialect 파일 위치 설정해줘야 group_concat() 함수를 사용할 수 있다.
spring.jpa.database-platform=com.xxxxx.xxxxx.xxx.config.CustomMySQLDialect  
```

### group_concat 사용 전 SQL문
![](/assets/img/development/server/2023-09-04_jpa-groupconcat/group_concat_01.png){:.centered width="80%"}
- 위 쿼리문 처럼 `group_concat`을 하기 전 결과를 보게 되면 `출입이력_일련번호`와 `유저_일련번호`는 같지만 `문진표_답변` 컬럼의 데이터 값이 달라서 총 3개의 `row`가 나오게 된다. 
- 내가 원하는 건 `출입이력_일련번호`와 `유저_일련번호`, `문진표_답변`을 **`한 row`** 에 나오게 하고 싶다.
- 해결책은 아래와 같다.

### group_concat 사용 후 SQL문
![](/assets/img/development/server/2023-09-04_jpa-groupconcat/group_concat_02.png){:.centered width="80%"}
- 같은 컬럼이지만 데이터가 다른 필드에 **`group_concat()`** 을 사용하여 쿼리문을 작성하는 것이다.
- 위 같이 **`group_concat()`** 을 적용하게 되면 같은 컬럼이지만 다른 값으로 나왔던 데이터들이 **`한 row`** 에 출력되게 된다.
  - **`group_concat()`** 과 **`GROUP BY 컬럼명`** 을 이용하여 **`한 row`** 에 출력할 수 있다.

### QueryDSL에 group_concat() 적용
```java
@Repository
@RequiredArgsConstructor
public class QClassAccessCntrlHistRepository implements ExAccessCntrlHistRepository { 
    private final JPAQueryFactory jpaQueryFactory;

  @Override
  public List<ResAccessCntrlHistVO> getAccessCntrlList(ReqAccessCntrlHistSearchVO searchVO) {
    QAccessCntrlHist qAccessCntrlHist = QAccessCntrlHist.accessCntrlHist;
    QGateInfo qGateInfo = QGateInfo.gateInfo;
    QUsr qUsr = QUsr.usr;
    QQuestnAnsr qQuestnAnsr = QQuestnAnsr.questnAnsr1;

    Pageable pageable = PageRequest.of(searchVO.getStart(), searchVO.getLength()); // Pageable로 만들어주기 위함

    return jpaQueryFactory
            .select(
                    Projections.fields(
                            ResAccessCntrlHistVO.class,
                            qAccessCntrlHist.accessCntrlHistSn,
                            qAccessCntrlHist.accessCntrlRegDt,
                            qUsr.usrCameraId,
                            qGateInfo.gateNm,
                            qAccessCntrlHist.img,
                            qUsr.nm,
                            qUsr.mbtlno,
                            qUsr.usrTy,
                            qUsr.mngGradeTy,
                            qAccessCntrlHist.tempr,
                            qAccessCntrlHist.temprStts,
                            qAccessCntrlHist.accessCntrlTy,
                            Expressions.stringTemplate("group_concat({0})", qQuestnAnsr.questnAnsr).as("questnAnsr"),
                            qQuestnAnsr.nm.max().as("qNm"),
                            qQuestnAnsr.mbtlno.max().as("qMbtlno")
                    )
            )
            .from(qAccessCntrlHist)
            .leftJoin(qUsr)
            .on(qAccessCntrlHist.usrSn.eq(qUsr.usrSn))
            .leftJoin(qGateInfo)
            .on(qAccessCntrlHist.gateInfoSn.eq(qGateInfo.gateInfoSn))
            .leftJoin(qQuestnAnsr)
            .on(qAccessCntrlHist.questnAnsrId.eq(qQuestnAnsr.questnAnsrId))
            .where(
                    eqGateInfoSn(searchVO.getGateInfoSn()),
                    betWeenStrAccessCntrlRegDt(searchVO.getSchStrDt(), searchVO.getSchEndDt()),
                    eqAccessCntrlTy(searchVO.getAccessCntrlTy()),
                    likeSearchKeyword(searchVO.getSearchKeyword())
            )
            .groupBy(qAccessCntrlHist.accessCntrlHistSn, qUsr.nm, qQuestnAnsr.nm, qUsr.mbtlno, qQuestnAnsr.mbtlno)
            .orderBy(qAccessCntrlHist.accessCntrlRegDt.desc())
            .offset(pageable.getOffset())
            .limit(pageable.getPageSize())
            .fetch();
  }
}
```

- 위 `select`문에 `Projections.fields()`필드 마지막 **`Expressions.stringTemplate("group_concat({0})", qQuestnAnsr.questnAnsr).as("questnAnsr")`** 을 보면 `group_concat`을 통해 `QueryDSL` 에서도 `MySQL`의 **`group_concat()`** 함수를 사용할 수 있다.

### QueryDSL에 group_concat() 적용 쿼리문 확인
![](/assets/img/development/server/2023-09-04_jpa-groupconcat/querydsl_group_concat_01.png){:.centered width="80%"}


![](/assets/img/development/server/2023-09-04_jpa-groupconcat/querydsl_group_concat_02.png){:.centered width="80%"}
- 위 사진들 처럼 정상적으로 query문이 날라갔고, JPA QueryDSL 에 정상적으로 바인딩 된 것을 확인 할 수 있다.


![](/assets/img/development/server/2023-09-04_jpa-groupconcat/querydsl_group_concat_result.png){:.centered width="80%"}
- 정상적으로 같은 컬럼의 각기 다른 값들을 한 row에 문자열 방식으로 출력 되었다. 

## 정리
- **`Hibernate`** 는 특정 데이터베이스에 종속되지 않고 `객체지향`스럽게 사용할 수 있도록 `추상화`해주어 **특정 DB에 종속된 함수(예, `MySQL`의 `group_concat`)는 제공하지 않는다.**
- 때문에 JPA QueryDSL에서 특정 DB의 함수를 사용하려면 **`설정`** 해주어야 한다.
- **`group_concat()`** 을 사용할 땐 꼭 **`GROUP BY 컬럼명`** 을 이용하여 한 줄(row)에 출력할 수 있게 해야한다.

<!-- Link -->
[집계 함수]: https://dev.mysql.com/doc/refman/8.0/en/aggregate-functions.html