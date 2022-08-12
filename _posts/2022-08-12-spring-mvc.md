---
title:  "[Web] Spring MVC 와 Filter, Interceptor"
search: true
categories: 
  - Spring
tag:
  - spring
last_modified_at: 2022-08-12T08:06:00-05:00
---

이번엔 Spring MVC와 Filter, Interceptor에 대해 정리해보았다.

# 목차
- [Spring MVC](#spring-mvc)
- [Filter vs Interceptor](#filter-vs-interceptor)


<br>
<br>

# Spring MVC
- Spring 프레임워크는 다양한 모듈로 구성되어 있는데 그 중 `Spring Core`를 바탕으로 다른 모듈들을 조합해 사용한다.
- 대표적으로 우리는 웹 개발을 하기 위해 Spring MVC나 Spring boot를 더해 사용하는데 이중 MVC 모듈을 알아보자. 
- Spring MVC란 Spring에서 제공하는 웹 모듈로 `Model`, `View`, `Controller` 세가지 구성요소를 사용해 사용자의 다양한 HTTP Request를 처리할 수 있게 만든 프레임워크이다. (MVC 패턴 적용)
- 즉 Spring 프레임워크 하나로 풀스택 개발을 하기 위한 모듈이다.

<br>

## MVC 패턴

- MVC 패턴이란 애플리케이션을 `Model`, `View`, `Controller`세 가지 역할로 나누어 개발하는 디자인 패턴을 말한다.
- 애플리케이션을 3개의 컴포넌트로 나누면 컴포넌트의 변경이 다른 컴포넌트에 영향을 미치지 않는다.
    - 유지보수 용이
- 컴포넌트 간의 결합성이 맞아 프로그램 수정이 용이하다.
    - 확장성 뛰어남
- `Model`
    - 어플리케이션의 데이터, 정보, DB 등을 말함
- `View`
    - 사용자에게 보여지는 화면, UI
- `Controller`
    - 데이터와 비즈니스 로직 사이의 상호 동작을 관리
    - 어플리케이션의 행위 정의
    
<br>

## Spring MVC 구조

![image](https://user-images.githubusercontent.com/47655983/183898143-61d40d36-646d-4c8b-b825-208fea3dc8f2.png)

![image](https://user-images.githubusercontent.com/47655983/183898215-79efc6c5-24f3-46b2-803f-efd63892dfde.png)

- MVC 패턴에서 Controller는 제일 앞단에서 클라이언트의 요청을 받아 적절한 처리를 해준다.
    - 하지만 Spring에서는 클라이언트의 요청을 `DispatcherServlet`이라는 녀석이 제일 먼저 받아서 적절한 Controller로 요청을 위임한다. (Front Controller 패턴)
- `HandlerMapping`
    - 클라이언틔 요청을 어떤 Controller가 처리할지 결정
    - URL과 요청 정보를 기준으로 어떤 핸들러 객체를 사용할지 결정한다.
- `ModelAndView`
    - Controller가 처리한 데이터 및 화면에 대한 정보를 보유한 객체
- `ViewResolver`
    - Controller가 리턴한 View 이름을 기반으로 Controller의 처리 결과를 보여줄 View 결정

<br>
<br>

# Filter vs Interceptor
![image](https://user-images.githubusercontent.com/47655983/183898401-7b54cb7c-3bfa-41e2-b2bb-3f8f05d30694.png)

- Request 요청을 받을 때 비즈니스 로직에 들어가기전 요청한 데이터를 가공하거나 요청이 적절한 권한을 가졌는지 확인하는 등의 일을 해야 할 때가 있다.
- 그럴 때마다 비즈니스 로직 사이에 해당하는 코드를 넣으면 나중에 유지보수하기 좋지 않다.
- 그래서 사용하는 것이 `Filter`와 `Interceptor`인데 둘의 차이를 알아보자.

<br>

## Filter
- J2EE 표준 스펙 기능
- `DispatcherServlet`에 요청이 전달되기 전/후에 url 패턴에 맞는 모든 요청에 대해 부가작업을 처리하는 기능을 제공한다. (ex - 인코딩 변환 처리, XSS 방어, 공통된 보안 인증)
- `DispatcherServlet`은 Spring의 맨 앞단이기 대문에 `Filter`는 Spring 범위 밖에서 처리가 된다.
- 즉 스프링 컨테이너가 아닌 톰캣과 같은 웹 컨테이너에 의해 관리(스프링 빈으로 등록은 됨)
- `web.xml`에서 설정
- Request/Response 객체 조작 가능

![image](https://user-images.githubusercontent.com/47655983/183898421-6f5dd987-6fc0-48a8-a841-e7c8ff17fb41.png)

<br>

## Interceptor
- J2EE 표준 스펙이 아닌 Spring이 제공하는 기술
- `DispatcherServlet`이 컨트롤러를 호출하기 전과 후에 요청과 응답을 참조하거나 가공할 수 있는 기능 제공 (ex - 로그인 체크, 권한 체크, 세부적인 보안 인증, `Controller`로 넘어가는 데이터 가공)
- `spring-servlet.xml`에서 설정
- Controller 마다 Handler가 매핑되어 있는데 권한 체크 등을 핸들러에서 해준다고 하면 Controller가 많아질수록 작성해야하는 코드량이 많아짐 → `Interceptor` 클래스에 위임해서 작업하면 반복되는 코드를 줄일 수 있음
- Request/Response 객체 조작 불가능

<br>

## 정리
- 정리하자면 `Filter`는 Spring Context 외부에 있기 때문에 전체 요청에 대해 처리해야하는 로직을 작성할 때 효율적이고
- `Interceptor`는 Spring Context 내부에 있기 때문에 DispatcherServlet에서 각 Controller로 요청을 넘겨줄 때 세부적으로 처리해야하는 로직에 효율적이다.