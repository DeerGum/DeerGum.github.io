---
title:  "[Web] Spring 개념 정리"
search: true
categories: 
  - Spring
tag:
  - spring
last_modified_at: 2022-06-29T08:06:00-05:00
---

김영한님의 스프링 인프런 강의를 듣고 내용을 정리해보았다.

# 목차
- [Web개발에서 자바 진영의 역사](#web개발에서-자바-진영의-역사)
- [Spring](#spring)
- [Ioc Di 컨테이너](#ioc-di-컨테이너)
- [Spring boot](#spring-boot)


# Web개발에서 자바 진영의 역사

- Spring 프레임워크가 나오기 이전의 자바 진영에서는 EJB(Enterprise JavaBeans)를 표준으로 사용해서 Web을 개발했음
- 복잡하고 프레임워크 종속적인 EJB에 반하는 개념으로 순수 자바 객체(POJO - Plain Old Java Object)라는 용어가 등장함
- 2002년 로드 존슨이라는 개발자가 EJB의 문제점을 지적하면서 `J2EE Design and Development`라는 책을 냄
    - 이 책에서는 EJB 없이도 충분히 고품질의 확장 가능한 애플리케이션을 개발할 수 있음을 보여줌
    - 이 책에 지금의 스프링 핵심 개념과 기반 코드가 들어 있음
    - BeanFactory, ApplicationContext, POJO(Plain Old Java Object), 제어의 역전, 의존관계 주입
- 책 출간 후 유겐 휠러, 얀 카로프가 로드 존슨에게 오픈소스 프로젝트를 제안
- 스프링 이름은 전통적인 J2EE(EJB)라는 겨울을 넘어 새로운 시작이라는 뜻으로 지음

<br>

# Spring
- 자바 플랫폼을 위한 오픈소스 애플리케이션 프레임워크로 엔터프라이즈급 애플리케이션 개발을 위한 경량화된 솔루션
- 자바 언어의 큰 특징은 객체 지향 언어 -> 스프링은 객체 지향 언어가 가진 강력한 특징을 살려내는 프레임워크


## 좋은 객체 지향 설계의 5가지 원칙 (SOLID)
- 클린코드의 저자 로버트 마틴이 정리한 5원칙
1. SRP(Single Responsibility Principle) : 단일 책임 원칙
    - 한 클래스는 하나의 책임만 가져야 한다.
    - 중요한 기준은 `변경`, 변경이 있을 때 파급 효과가 적으면 SRP원칙을 잘 따른 것
2. OCP(Open/Closed Principle) : 개방-폐쇄 원칙
    - 소프트웨어 요소는 확장에는 열려 있으나 변경에는 닫혀 있어야 한다.
    - 다형성 활용(단 다형성만 사용해선 원칙 지킬 수 없음)
    - 객체를 생성하고, 연관관계를 맺어주는 별도의 조립, 설정자가 필요함
3. LSP(Liskov Substitution Principle) : 리스코프 치환 원칙
    - 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위 타입의 인스턴스로 바꿀 수 있어야 한다.
    - 다형성에서 하위 클래스는 인터페이스 규약을 다 지켜야 한다, 다형성을 지원하기 위한 원칙, 인터페이스를 구현한 구현체는 믿고 사용하려면, 이 원칙이 필요 (ex - 자동차 인터페이스의 엑셀은 앞으로 가라는 기능, 뒤로 가게 구현하면 LSP 위반, 느리더라고 앞으로 가야함)
4. ISP(Interface Segregation Principle) : 인터페이스 분리 원칙
    - 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.
    - ex) 자동차 인터페이스 -> 운전 인터페이스, 정비 인터페이스로 분리
    - 인터페이스가 명확해지고, 대체 가능성이 높아진다.
5. DIP(Dependency Inversion Principle) : 의존관계 역전 원칙
    - 프로그래머는 "추상화에 의존해야지, 구체화에 의존하면 안된다." 의존성 주입은 이 원칙을 따르는 방법 중 하나
    - 구현 클래스에 의존하지 말고 인터페이스에 의존하라는 뜻
- 다형성만 갖고 OCP와 DIP를 지킬 수 없음 뭔가 더 필요

## 스프링과 객체 지향
- 스프링은 다음 기술로 다형성 + OCP, DIP를 가능하게 지원
    - DI(Dependency Injection) : 의존관계, 의존성 주입
    - DI 컨테이너 제공
- 클라이언트 코드의 변경 없이 확장 가능

<br>

# IoC, DI, 컨테이너
## 제어의 역전 IoC(Inversion of Control)
- 기존의 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고, 연결하고, 실행함, 한마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종, 개발자 입장에서는 자연스러운 흐름임

## 프레임워크 vs 라이브러리
- 내가 작성한 코드를 제어하고, 대신 실행하면 프레임워크(프레임워크가 지정한 규칙에 따라서 코드를 작성하면)
- 내가 작성한 코드가 직접 제어의 흐름을 담당한다면 라이브러리(라이브러리의 클래스나 메소드를 내 입맞에 맞춰서 가져다 사용하면)

## 의존관계 주입 DI(Dependency Injection)
### 의존관계
- 의존관계란 의존대상 B가 변하면 그것이 A에 영향을 미치는 것이라고 표현할 수 있음. 

```java
class A {
  public void func() {
    //~~~
  }
}

class B {
  static A a = new A();

  public void func2() {
    a.func();
  }
}
```

> B클래스는 A클래스의 `func()` 함수 내용이 변하면 영향을 받으므로 B는 A에 의존하고 있음

- 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있음



## IoC 컨테이너, DI 컨테이너
- 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 IoC 컨테이너 또는 `DI 컨테이너`라고 함 (또는 어셈블러, 오브젝트 팩토리 등으로 불림)
- IoC컨테이너는 `ApplicationContext`에서 관리됨

## Bean
- IoC 컨테이너가 관리하는 자바 객체를 빈이라고 부름
- 우리가 그저 `new`연산자로 생성한 객체는 빈이 아님
- `ApplicationContext.getBean()`으로 얻을 수 있는 객체가 빈

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FnsE6w%2Fbtq9flCz2G4%2FumjyApnZqSkHTQZ4Aa1f60%2Fimg.png)

## BeanFactory
- 스프링 컨테이너의 최상위 인터페이스
- Bean을 등록, 생성, 조회, 반환 관리 기능이 존재
- 보통은 BeanFactory를 바로 사용하지 않고 이를 확장한 ApplicationContext를 사용

## ApplicationContext
- BeanFactory를 확장한 인터페이스
- 다양한 부가 기능을 추가로 제공함
  - 국제화가 지원되는 텍스트 메시지 관리
  - 이미지같은 파일 자원을 로드할 수 있는 포괄적인 방법 제공
  - 리스너로 등록된 빈에게 이벤트 발생을 알려줌


<br>

# Spring boot
- 스프링은 기능이 많은만큼 환경설정이 복잡함 -> 이것을 편리하게 만들어준 것이 스프링 부트

## Spring보다 편리한 점
- dependency 설정 간소화
- 손쉬운 빌드 구성을 위한 starter dependency 제공
- Tomcat 같은 웹 서버를 내장해서 별도의 웹 서버를 설치하지 않아도 됨
- 스프링과 3rd party 라이브러리 자동 구성
- 메트릭, 상태 확인, 외부 구성 같은 프로덕션 준비 기능 제공

