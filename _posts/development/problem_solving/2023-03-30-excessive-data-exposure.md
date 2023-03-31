---
layout: post
title: "[성능개선] 불필요한 정보 노출로 인한 성능 저하"
subtitle: "불필요한 정보 노출로 인한 성능 저하"
category: development
tags: problem_solving 성능개선
image:
    path: /assets/img/development/problem_solving/2023-03-26/problem_solving_cover.png
---

<span style="font-size:30px;">\#**목차**</span>
* this unordered seed list will be replaced by the toc
{:toc}

# 문제
- `jsp`프로젝트에서 `vue`로 마이너그레이션 중 리스트에 과도한 정보를 `response`해주는 경향 발생.
- 해당 문제는 `리스트`에 불필요한 정보들이 너무 많이 `client`에 응답을 해주는 것을 발견하였다.

## 문제 상황 재현 코드
```java
// file: "재현 코드 = 리스트 가져오는 서비스 재현 코드.java"
@Service
@RequiredArgsConstrutor
public class ExampleService {
    private final ExUser3JoinTableRepository  exUser3JoinTableRepository; 
    
    public List<ExUser3JoinTable> getList(ReqDTO reqDto) {
        List<ExUser3JoinTable> list = exUser3JoinTableRepository.findAll(
                Specification.joinAndJoinAndJoinAll(
                        reqDto.getStartRegistDate(),
                        reqDto.getEndRegistDate(),
                        reqDto.getStartModifyDate(),
                        reqDto.getEndModifyDate(),
                        reqDto.getSearchKeyword(),
                        reqDto.getClientComponySn(),
                        reqDto.getAuthoritySn(),
                        reqDto.getUseStatus()
                ), Sort.by(Sort.Direction.DESC, "등록일자")
        );
        return list;
    }
}
```

- 위 `Service` 로직을 보면 `Specification`을 이용하여 `요청DTO`를 통해 검색조건을 만족하는(없으면 `Object.isEmpty`로 `if문` 패스) 3개의 `join`된 테이블로 `list`를 조회해한다.
- 하지만 여기서 문제는 바로 `List<ExUser3JoinTable>` 부분이다.

## 문제의 조인 테이블

```java
// file: "재현 코드 = 3개의 조인 테이블 재현 코드.java"
import java.io.Serializable;

@Entity
@Table(name = "고객정보테이블")
public class ExUser3JoinTable extends SuperUser implements Serializable {
    @NotFound(action = NotFoundAction.IGNORE)
    @JoinColumn(name = "authority_sn", insertable = false, update = false, referenceColumnName = "authority_sn")
    @ManyToOne(fetch = FetchType.EAGER)
    public Authority AuthorityInfo;

    @NotFound(action = NotFoundAction.IGNORE)
    @JoinColumn(name = "client_company_sn", insertable = false, update = false, referenceColumnName = "client_company_sn")
    @ManyToOne(fetch = FetchType.EAGER)
    public ClientCompony ClientComponyInfo;
}
```

## 클라이언트에 전송되는 과도한 데이터들 
![원인 테이블](/assets/img/development/problem_solving/2023-03-26/cause_join_table.png){:.centered width="50%"}

- 무려 **33개**의 컬럼의 데이터가 요청되어있던 것이다.
- 내가 필요한 데이터는 **13개**의 컬럼 데이터만 필요하다.
- 무려 20개의 사용되지 않는 컬럼들(물론 조인한 컬럼2개 제외하면 18개이다.)을 DB에서 가져와 `Client`쪽으로 데이터를 보내주고 있었다.

# 해결
- 기존 `JPA`의 `findAll`키워드를 사용해서 모든 컬럼을 찾아오는 대신에 `QueryDSL`로 필요한 컬럼만 조인하여 `응답 DTO`를 만들어 해결하였다.

## 데이터 조회를 QueryDSL로 
### 데이터 조회를 QueryDSL로 - BooleanBuilder 사용

```java
// file: "재현 코드 = queryDSL - BooleanBuilder를 사용한 예제.java"
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

@Repository
@RequiredArgsConstructor
class QClassUserRepository implements ExUserRepository {
    private final JPAQueryFactory jPAQueryFactory;
    @Override
    public List<UserListDTO> getList(SearchVO searchVO) {
        QUser qUser = QUser.user;
        QAuthority qAuthority = QAuthority.authority;
        QClientCompony qClientCompony = QClientCompony.clientCompony;

        BooleanBuilder builder = new BooleanBuilder();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("YYYY-MM-dd");
        if (ObjectUtils.isNotEmpty(searchVO.getRegStrDate()) && ObjectUtils.isNotEmpty(searchVO.getRegEndDate())) {
            LocalDateTime parseStrDate = LocalDateTime.of(LocalDate.parse(searchVO.getStrDate(), formatter), LocalTime.of(0, 0, 0));
            LocalDateTime parseEndDate = LocalDateTime.of(LocalDate.parse(searchVO.getEndDate(), formatter), LocalTime.of(23, 59, 59));
            builder.and(qUser.regDt.between(parseStrDate, parseEndDate));
        }
        if (StringUtils.isNotBlank(searchVO.getSearchKeyword())) {
            builder.or(qUser.nm.like("%" + searchVO.getSearchKeyword() + "%"));
            builder.or(qAuthority.authNm.like("%" + searchVO.getSearchKeyword() + "%"));
            builder.or(qClientCompony.coNm.like("%" + searchVO.getSearchKeyword() + "%"));
        }
        // 등등...
        return jPAQueryFactory
                .select(
                        Projections.filds(
                                UserListDTO.class,
                                // 조회 컬럼
                        )
                )
                .from(qUser)
                .leftjoin(qAuthority)
                .on(qUser.userSn.eq(qAuthority.userSn))
                .leftjoin(qClientCompony)
                .on(qUser.coSn.eq(qClientCompony.coSn))
                .where(builder)
                .orderBy(qUser.regDt.desc())
                .fetch();
    }
}
```

- 위 예제 코드를 보면 그럭저럭 나쁘지 않은거 같지만.. 
- 사실 해당 해당 `QClassUserRepository`에 구현 클래스가 하나이면 상관 없지만 여러 구현 클래스가생기면 문제가 발생하기 시작한다.
- 예를 들어 매번 검색조건에 날짜가 들어가게 되면 `BooleanBuilder`와 `DateTimeFormatter`을 구현 클래스 안에다가 작성해야하는 번거러움이 발생하게 된다.
- 위 방법을 좀 더 편리하게 바꾸려면 `BooleanExpression`을 사용하여 메서드를 만들고 호출하는 형식으로 리팩토링하면 코드가 훨씬 깔끔해진다.

### 데이터 조회를 QueryDSL로 - BooleanExpression 사용하여 리팩토링

```java
// file: "재현 코드 = queryDSL - BooleanExpression으로 리팩토링 사용한 예제.java"
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

@Repository
@RequiredArgsConstructor
class QClassUserRepository implements ExUserRepository {
    private final JPAQueryFactory jPAQueryFactory;
    @Override
    public List<UserListDTO> getList(SearchVO searchVO) {
        QUser qUser = QUser.user;
        QAuthority qAuthority = QAuthority.authority;
        QClientCompony qClientCompony = QClientCompony.clientCompony;
        
        return jPAQueryFactory
                .select(
                        Projections.filds(
                                UserListDTO.class,
                                // 조회 컬럼
                        )
                )
                .from(qUser)
                .leftjoin(qAuthority)
                .on(qUser.userSn.eq(qAuthority.userSn))
                .leftjoin(qClientCompony)
                .on(qUser.coSn.eq(qClientCompony.coSn))
                .where(
                        betWeenRegDt(searchVO.getRegStrDate(), searchVO.getRegEndDate(), "regDt"),
                        betWeenModDt(searchVO.getModStrDate(), searchVO.getModEndDate(), "modDt"),
                        likeSearchKeyword(searchVO.getSearchKeyword()),
                        // 등등
                )
                .orderBy(qUser.regDt.desc())
                .fetch();
    }
    
    // 검색조건 등록 날짜
    private BooleanExpression betWeenRegDt(String schStrRegDt, String schEndRegDt, String schDateType) {
        return getBooleanExpression(schStrRegDt, schEndRegDt, schDateType);
    }
    // 검색조건 수정 날짜
    private BooleanExpression betWeenModDt(String schStrModDt, String schEndModDt, String schDateType) {
        return getBooleanExpression(schStrModDt, schEndModDt, schDateType);
    }
    // 날짜 parsing 
    private BooleanExpression getBooleanExpression(String strDt, String endDt, String schDateType) {
        QUser qUser = QUser.user;
        if (StringUtils.isBlank(strDt) || StringUtils.isBlank(endDt)) return null;
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("YYYY-MM-dd");
        LocalDateTime parseStrDate = LocalDateTime.of(LocalDate.parse(searchVO.getStrDate(), formatter), LocalTime.of(0, 0, 0));
        LocalDateTime parseEndDate = LocalDateTime.of(LocalDate.parse(searchVO.getEndDate(), formatter), LocalTime.of(23, 59, 59));
        return schDateType("regDt") ? qUser.regDt.between(parseStrDate, parseEndDate) : qUser.modDt.between(parseStrDate, parseEndDate);
    }
    // 검색어
    private BooleanExpression likeSearchKeyword(String keyword) {
        QUser qUser = QUser.user;
        QAuthority qAuthority = QAuthority.authority;
        QClientCompony qClientCompony = QClientCompony.clientCompony;
        return keyword != null ? qUser.nm.like("%" + searchVO.getSearchKeyword() + "%")
                .or(qAuthority.authNm.like("%" + searchVO.getSearchKeyword() + "%"))
                .or(qClientCompony.coNm.like("%" + searchVO.getSearchKeyword() + "%")) : null;
    }
    // 등등 ...
}
```

- 위 예시 코드처럼 `BooleanExpression`을 활용하여 공통적으로 사용하거나 사용할 법한 메서드들을 만들어 사용하였다.
- 이렇게 되면 매번 구현 클래스를 만들때 일일이 조건문을 주지 않고 사용할 수 있는 편리한 장점이 있다.

## JPA findAll 대신 QueryDSL을 활용한 필요한 컬럼 조회
![QueryDSL](/assets/img/development/problem_solving/2023-03-26/solution_querydsl.png){:.centered width="50%"}
- 위의 코드로 인해 코드 량도 좀 더 깔끔하게 줄었다.
- 물론 검색 조건에 대한 추가작업은 해야하는 일이 남아 있지만 그것을 제외하더라도 크게 성능을 향상 시킨거 같다.

## 수정한 List
```java
// file: "재현 코드 = 수정한 테이블의 리스트 재현 코드.java"
@Service
@RequiredArgsConstrutor
public class ExampleService {
    private final ExUserRepository  exUserRepository;

    public List<UserListDTO> getList(SearchVO searchVO) {
        return exUserRepository.getList();
    }
}
```

- 해당 코드를 통해 좀 더 코드가 간편해진 듯하다.
- 물론 검색 조건의 추가 작업은 진행해야 하지만, 그것이 코드에 또는 성능에 크게 영향을 줄 거 같진 않다.

## 수정한 List Debugging
![수정한 테이블의 컬럼](/assets/img/development/problem_solving/2023-03-26/solution_join_table.png){:.centered width="50%"}
- 수정한 코드를 통해 필요한 **13개**의 컬럼 데이터만 보내주었다.
- 기존의 데이터(33개의 컬럼 데이터)와 약 20개의 차이가 난다.

## 기존 vs 수정한 코드의 실행 시간
### 1번째 페이지 - 약 61% 개선
![](/assets/img/development/problem_solving/2023-03-26/custco_list_non_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/custco_list_querydsl.png){:.centered width="100%"}
- 기존 `List` 호출 시간 : **`0.018sec`**
- 수정 `List` 호출 시간 : **`0.007sec`**
- 약 **`61%`**의 성능 개선

### 2번째 페이지 - 약 31%, 81% 개선
![](/assets/img/development/problem_solving/2023-03-26/mngracnt_list_non_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/mngracnt_list_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/mngracnt_detail_non_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/mngracnt_detail_querydsl.png){:.centered width="100%"}
- 기존 `List` 호출 시간 : **`0.077sec`**
- 수정 `List` 호출 시간 : **`0.053sec`**
- 약 **`31%`**의 성능 개선

- 기존 `Detail` 호출 시간 : **`0.027`**
- 수정 `Detail` 호출 시간 : **`0.005`**
- 약 **`81%`**의 성능 개선

### 3번째 페이지 - 약 74%, 39% 개선
![](/assets/img/development/problem_solving/2023-03-26/gateinfo_list_non_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/gateinfo_list_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/gateinfo_detail_non_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/gateinfo_detail_querydsl.png){:.centered width="100%"}
- 기존 `List` 호출 시간 : **`0.027`**
- 수정 `List` 호출 시간 : **`0.007`**
- 약 **`74%`**의 성능 개선

- 기존 `Detail` 호출 시간 : **`0.023`**
- 수정 `Detail` 호출 시간 : **`0.014`**
- 약 **`39%`**의 성능 개선

### 4번째 페이지 - 약 78%, 93% 개선
![](/assets/img/development/problem_solving/2023-03-26/dvccamerainfo_list_non_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/dvccamerainfo_list_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/dvccamerainfo_detail_non_querydsl.png){:.centered width="100%"}
![](/assets/img/development/problem_solving/2023-03-26/dvccamerainfo_detail_querydsl.png){:.centered width="100%"}
- 기존 `List` 호출 시간 : **`0.033`**
- 수정 `List` 호출 시간 : **`0.007`**
- 약 **`78%`**의 성능 개선

- 기존 `Detail` 호출 시간 : **`0.105`**
- 수정 `Detail` 호출 시간 : **`0.009`**
- 약 **`93%`**의 성능 개선

# 결론
- 위 문제 해결의 개선을 통해 **전체적으로 약 `65%`의 성능을 개선**하게 되었다.
- **물론!**, 컴퓨터의 성능에 따라 시간 차이는 좀 달라질 수 있다. 하지만, 기존 로직보다 성능이 좋은 건 사실이다!
- 또한, `client`에 필요한 정보로만 응답을 보내주어 보안상의 이슈 또한 해결하였다.
- 불필요한 정보는 요청/응답하지 않는 것이 좋다고 다시 한번 느끼게 되었다.