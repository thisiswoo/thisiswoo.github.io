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
    private final Ex3JoinTableRepository  ex3JoinTableRepository; 
    
    public List<ExJoinAndJoinAndJoinTable> getList(ReqDTO reqDto) {
        List<ExJoinAndJoinAndJoinTable> list = ex3JoinTableRepository.findAll(
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
- 하지만 여기서 문제는 바로 `List<ExJoinAndJoinAndJoinTable>` 부분이다.

## 문제의 조인 테이블

```java
// file: "재현 코드 = 3개의 조인 테이블 재현 코드.java"
import java.io.Serializable;

@Entity
@Table(name = "고객정보테이블")
public class Ex3JoinTable extends SuperUser implements Serializable {
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

    public List<ResJoinTableDTO> getList(ReqDTO reqDto) {
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

## 기존 코드의 실행 시간
![기존코드 실행 시간](/assets/img/development/problem_solving/2023-03-26/mngrAcntList_timelaps_before.png){:.centered width="100%"}

## 수정한 코드의 실행 시간
![수정코드 실행 시간](/assets/img/development/problem_solving/2023-03-26/mngrAcntList_timelaps_after.png){:.centered width="100%"}
- 기존 `List` 호출 시간 : `0.113sec`
- 수정 `List` 호출 시간 : `0.061sec`
- 약 `54%`의 성능 개선을 이루었다.

# 결론
- 위 문제 해결의 개선을 통해 약 `54%`의 성능을 개선하게 되었다.
- 또한, `client`에 필요한 정보로만 응답을 보내주어 보안상의 이슈 또한 해결하였다.
- 불필요한 정보는 요청/응답하지 않는 것이 좋다고 다시 한번 느끼게 되었다.