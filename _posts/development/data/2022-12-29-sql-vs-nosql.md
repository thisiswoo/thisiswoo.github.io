---
layout: post
title: "[SQL] SQL과 NoSQL의 차이"
subtitle: "Difference between SQL ans NoSQL"
category: development
tags: database SQL NoSQL
image:
  path: /assets/img/development/database/2022-12-29/sql_vs_nosql_title.png
---
> “SQL과 NoSQL의 차이를 알아보자.”

# 정의

* this unordered seed list will be replaced by the toc
{:toc}

## SQL(Structured Query Language) 이란?

![SQL](/assets/img/development/database/2022-12-29/sql_query.png){:.centered width="70%"}
**<center>SQL</center>**

- **SQL(Structured Query Language) 관계형 데이터베이스(RDBMS)** 또는 **SQL 데이터베이스 작업을 위한 표준 언어**이다.
- SQL을 사용하여 데이터베이스 객체를 만들고 데이터베이스 레코드를 **삽입(INSERT)**, **업데이트(UPDATE)**, **삭제(DELETE)** 또는 쿼리를 **조회(SELECT)**할 수 있다.
- SQL은 데이터베이스 **최적화 및 유지 관리 또는 데이터 분석 수행과 같은 복잡한 작업에 사용**될 수 있다.
- SQL 데이터베이스에는 **MySQL, Sybase, Microsoft SQL Server, Oracle** 등이 많이 사용되고 있다.

```SQL
-- Query 사용예
INSERT INTO EMPLOYEES (Name, Age, PhoneNumber) 
VALUES (“EMP1”, 21, “1234567890”);
```

<!-- Continue with [[SQL] SQL 명령어 종류](vue-lifecycle){:.heading.flip-title}
{:.read-more} -->

### 데이터베이스 구조(DB Structure)

![DB Structure](/assets/img/development/database/2022-12-29/sql_structure.png){:.centered width="70%"}
**<center>DB Structure</center>**

- **SQL 데이터베이스 구조의 데이터는 열(row)과 행(column)으로 구성된 테이블로 구성**되어 있다.
- SQL 데이터베이스는 **수직적 구조**로 새로운 데이터가 추가될 때마다 **수직적으로  쌓이게 된다**.

### 관계형 데이터베이스(Relational database)

![DB Relation](/assets/img/development/database/2022-12-29/relations.png){:.centered width="70%"}
**<center>DB Structure</center>**

- 관계형 DB(RDBMS)는 데이터베이스에 저장된 데이터의 구조, 즉 **데이터 스키마를 명확하게 정의하는 것 또한 매우 중요**하다.
- 테이블의 모든 레코드(column - fields)는 **정의된 열(row)을 준수**해야 하며, **해당 형식(테이블/행/열)에서만 데이터 조작을 수행**할 수 있다.

## NoSQL(Non SQL) 이란?

![NoSQL](/assets/img/development/database/2022-12-29/nosql_title.jpg){:.centered width="70%"}
**<center>SQL</center>**

- **NoSQL은 고정 스키마가 필요하지 않고 쉽게 확장**되며 조인을 수행할 수 없는 **비관계형 데이터베이스**이다.
- **방대한 데이터 스토리지가 필요한 분산 데이터 저장소에 적합**하여 빅데이터와 실시간 웹 앱에 많이 사용**된다.
- NoSQL 데이터베이스는 관계형 스키마에 데이터를 저장하지 않기 때문에 전체 테이블의 구조를 변경하거나 나머지 행에 중복 요소를 추가하지 않고도 **즉시 데이터 속성을 추가**할 수 있다.
- NoSQL 데이터베이스는 **수평적 확장이 뛰어나**고 파티션 허용 오차가 기반에 내장되어 있어 많은 양의 데이터에 대해 1초 미만의 응답 시간이 필요한 시나리오에서 잘 작동한다.

## NoSQL의 종류

![NoSQL](/assets/img/development/database/2022-12-29/database.png){:.centered width="70%"}
**<center>NoSQL</center>**

- NoSQL 데이터베이스는 **수평적 구조**로 새로운 데이터가 추가될 때마다 **수평적으로  쌓이게 된다**.

### Document Databases

```json
[
    {
        "year" : 2012,
        "title" : "The Thieves",
        "info" : {
            "directors" : [ "Choi Dong-hoon"],
            "release_date" : "2012-07-25T00:00:00Z",
            "rating" : 8,
            "genres" : ["Action", "Drama"],
            "image_url" : "https://en.wikipedia.org/wiki/The_Thieves#/media/File:The_Thieves.jpg",
            "plot" : "Ten Thieves. Only One Diamond. They began to Move.",
            "actors" : ["Kim Yoon-seok", "Kim Hye-soo", "Lee Jung-jae", "Jun Ji-hyun", "Simon Yam", "Kim Hae-sook", "Oh Dal-su", "Kim Soo-hyun", "Derek Tsang", "Angelica Lee", "Shin Ha-kyun"]
        }
    },
    {
        "year": 2016,
        "title": "The Age of Shadows",
        "info": {
            "plot": "Enemy or Comrade",
            "rating": 43
        }
    }
]
```

- **Document DB**는 데이터를 **JSON 형식**의 문서로 데이터를 저장 및 쿼리하도록 설계된 **비관계형 데이터베이스**이다. 
- **Document DB**를 사용하면 개발자들이 **자신의 애플리케이션 코드에서 사용하는 것과 동일한 문서 모델 형식을 사용**하여 데이터베이스에서 보다 손쉽게 데이터를 저장하고 쿼리 작업을 할 수 있다.
- **Document DB**는 문서 및 문서 데이터베이스의 유연하고 **반구조화된 계층적 특성**을 통해 개발자는 계속해서 애플리케이션의 요구를 발전시킬 수 있다.
- **Document DB**는 **유연한 인덱싱, 강력한 임시 쿼리, 문서 모음에 대한 분석을 지원**한다.
- **Document DB**는 **MongoDB**, **CouchDB**, **MarkLogic** 등이 있다.

```javascript
// mongo DB Query 사용예
db.employees.insert({
  "name": "John",
	"Age": 21,
	"cellphone": "1234567890"
})
```

### Key-Value-Based Databases

![Key-Value-Based DB](/assets/img/development/database/2022-12-29/key_value_db.png){:.centered width="70%"}
**<center>Key-Value-Based DB</center>**

- **Key-Value** DB는 키-값 메소드를 사용하여 데이터를 저장하는 비관계형 데이터베이스 유형이다.
- **Key-Value** DB는 key를 고유한 식별자로 사용하는 키-값 쌍의 집합으로 데이터를 저장한다.
- **Key-Value** DB는 단순한 객체에서 복잡한 집합체에 이르기까지 무엇이든 키와 값이 될 수 있으며 **파티셔닝이 가능**하고 다른 유형의 데이터베이스로는 불가능한 범위까지 **수평 확장을 가능**하게 한다.
- **Key-Value** 기반 DB는 **Redis**, **Memcache** 등이 있다.

### Column-Oriented/Family Databases

#### 1.Column-Oriented Databases
![Column Oriented DB](/assets/img/development/database/2022-12-29/column_oriented_db.png){:.centered width="70%"}
**<center>Column-Oriented DB</center>**

- **Column-Oriented DB**는 Tabled의 data를 Column 단위로 쪼개어 저장하는 DB를 의미한다. 
- **Column-Oriented DB**는 하나의 Column이 하나의 Disk Block안에 저장된다. 
- **Column-Oriented DB**는 1개의 Block만 읽고 결과를 구할 수 있기 때문에 빠른 처리가 가능하다.
-  이처럼 Data를 분석하는 동작의 경우 Data Table에서 **모든 Column이 필요한 것이 아니라 일부 Column이 필요한 경우가 대부분**이다. 
- 따라서 Column-oriented DB는 **OLAP(Online Analytical Processing) 처리에 유리**한 반면, **OTLP(Online transaction processing) 처리에 불리**하다.

#### 2.Column-Family Databases
![Column Family DB](/assets/img/development/database/2022-12-29/column-family_db.png){:.centered width="70%"}
**<center>Column-Family DB</center>**

- **Column-Family DB**는 Column을 나타내는 Column Key/Data/Timestamp Tuple을 Row를 나타내는 **Row Key에 Mapping하여 Data Table을 표현하는 DB**이다.
- RDBMS에서 NULL값도 Disk Block 공간을 차지하지만 Column Family DB에서는 **Row별로 자유로운 Column 추가/삭제가 가능하기 때문에 NULL값을 위한 Column이 별도로 필요없다**.
- 일반적으로 Column-oriented DB와 Column Family DB가 혼용되어 사용되어 Column Family DB가 Column-oriented DB라고 간주하는 경우가 많은데 같은 DB라고 할 수는 없다. 
- **Column Family DB인 HBASE**는 Column의 집합인 Column Family 단위로 **Disk Block에 저장**되기 때문에 **Column-oriented DB로 분류**된다. 
- 하지만 또 하나의 Column Family DB인 **Cassandra**는 **Row 단위로 Disk Block에 저장**되기 때문에 **Column-oriented DB**라고 할 수 없다.

### Graph Databases

![Graph DB](/assets/img/development/database/2022-12-29/graph.png){:.centered width="70%"}
**<center>Graph DB</center>**

- **Graph DB**는 관계를 저장하고 탐색하도록 특별히 구축되다. 
- **Graph DB**는 노드를 사용하여 데이터 엔터티를 저장하고, 엣지로는 엔터티 간의 관계를 저장하게 되는데, 엣지는 항상 시작 노드, 끝 노드, 유형과 방향을 가지며, 상-하위 관계, 동작, 소유자 등을 문서화 한다. 
- 하나의 노드가 가질 수 있는 관계의 수와 종류에는 제한이 없다.
- **Graph DB** 그래프는 특정 엣지의 유형 또는 전체 그래프를 전반을 통하여 트래버스될 수 있다. 
- **Graph DB**에서 노드 간의 관계는 쿼리 시간에는 포함되지 않지만 데이터베이스에서 유지되기 때문에 조인 또는 관계를 트래버스하는 속도가 매우 빠르다. 
- **Graph DB**는 데이터 간의 관계를 만들고 이러한 관계를 신속하게 쿼리해야 할 때 소셜 네트워킹, 추천 엔진, 이상 탐지 등의 사용 사례에 유용하다.
- **Graph DB**는 **Neo4j**, **RedisGraph(Redis에 내장된 그래프 모듈)**, **OrientDB** 등이 있다.

## 결론!


### SQL과 NoSQL 비교

|**Parameter**|**SQL**|**NoSQL**|
|:---:|:---:|:---:|
|관계|관계형|비관계형|
|쿼리 언어|구조화된 쿼리 언어(SQL)|문서, 키-값, 그래프, 컬럼기반|
|사용|OLAP 시스템에 사용|최신 애플리케이션 개발에 대한 요구에 부응하여 개발|
|개요|미리 정의된 스키마|구조화되지 않은 데이터에 동적 데이터베이스를 사용|
|확장성|수직 확장|수평 확장|
|유형|테이블 기반의 DB|문서 기반, 키-값 쌍 또는 그래프 DB|
|계층적 데이터 저장|적합하지 않음|적합|
|대표적인 DB|SQL, MySQL, MS-SQL, Postgres|MongoDB, Redis, Neo4j, Cassandra, Hbase|

### 2022 DB Ranking

![2022 DB Ranking](/assets/img/development/database/2022-12-29/db_ranking_2022.png){:.centered width="70%"}
**<center>2022 DB Ranking</center>**

- SQL의 언어의 사용량이 높긴 하지만 NoSQL의 사용량도 점차 꾸준히 늘고 있다. 즉, 기획하고 설계하는 프로젝트에 대해 선택하는 언어가 다른 거 같다. 어느 한쪽이 좋은 언어 라고 얘기하기는 어려운 거 같다. 각자의 장/단점이 있는 언어이기 때문에 해당 프로젝트에 가장 알맞은 언어를 선택하여 설계하여 개발하는 것이 가장 좋은 거 같다.

## Reference
- [https://hevodata.com](https://hevodata.com/learn/sql-vs-nosql/)
- [https://viblo.asia](https://viblo.asia/p/cau-chuyen-muon-thuo-cua-sql-va-nosql-Do754OXelM6#_12-database-structure-3)
- [AWS](https://aws.amazon.com/ko/nosql/)
- [SCYLLA](https://www.scylladb.com/glossary/wide-column-database/)
- [https://ssup2.github.io](https://ssup2.github.io/theory_analysis/NoSQL_Column-oriented_Column_Family_DB/)
- [https://db-engines.com/](https://db-engines.com/en/ranking_trend)







