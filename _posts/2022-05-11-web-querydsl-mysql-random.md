---
title:  "[Web] QueryDsl Mysql 사용시 랜덤 레코드 뽑아오기"
search: true
categories: 
  - web
tag:
  - querydsl
  - mysql
last_modified_at: 2022-05-11T08:06:00-05:00
---

최근에 하고 있는 Spring boot프로젝트에서 복잡한 쿼리문은 QueryDsl을 이용해서 처리해주고 있는데

테이블의 무작위 레코드를 뽑아내야 하는 일이 생겼다.

관련 자료를 찾아보니 아래와 같은 코드를 확인할 수 있었다.

```java
public Food findFood() {
    return jpaQueryFactory.selectFrom(qFood)
            .orderBy(NumberExpression.random().asc())
            .fetchFirst();
}
```

<br>

하지만 이 방법은 동작하지 않았고 원인을 알아보니 내가 사용하고 있는 Mysql에서는 이 기능을 지원하지 않는다는 것이었다.

좀더 찾아보니 다행히 해결법을 찾을 수 있었다.

Mysql을 DB로 사용할 때는 아래와 같은 코드로 랜덤으로 정렬하여 레코드를 뽑아올 수 있다.

```java
public Food findFood() {
    return jpaQueryFactory.selectFrom(qFood)
            .orderBy(Expressions.numberTemplate(Double.class, "function('rand')").asc())
            .fetchFirst();
}
```

<br>

리스트로 받아오고 싶으면 아래와 같이 작성해주자

```java
public List<Food> findListOfFood() {
    return jpaQueryFactory.selectFrom(qFood)
            .orderBy(Expressions.numberTemplate(Double.class, "function('rand')").asc())
            .limit(5) //몇개를 뽑아올건지
            .fetch();
}
```