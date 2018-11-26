---
title: J2EE 설계와 개발 without ejb
key: 20181119
tags: Book
---

## Architectures

비즈니스 로직이 별도의 계층이 아닌 스프링의 컨트롤러에 구현되면 어떻게 될까?

* servelet API와 비즈니스 객체가 묶여버린다.
* 비즈니스 객체의 재사용성 감소
* 책임의 혼란
* 선언적 트랜잭션의 부재



경량 컨테이너에서가 서비스 계층을 구동할 때의 행동

* 비즈니스 객체의 라이프사이클을 관리
* IOC 구현
* 선언적 트랜잭션 구현



### Introducing the Spring Framework

기본 구성요소

* Bean factory
* Application context
* AOP framework
* Auto-proxing
* Transaction management
* DAO abstraction
* JDBC support
* Integration with O/R mapping tools
* Web MVC framework
* Remoting support