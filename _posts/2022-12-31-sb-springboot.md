---
layout: post
title: "SpringBoot - Spring / SpringBoot 개념정리"
categories: springboot
tags: [Spring Boot, Spring]
---

## Spring 이란?

<I>Spring은 자바 기반의 웹 어플리케이션을 만들 수 있는 프레임 워크이다.</I>

- Spring Framework는 자바 기반의 엔터프라이즈 어플리케이션을 위한 프로그래밍 및 Configuration Model을 제공한다.
- 다른 언어들로 웹 서버 개발과 같이 java 개발자들은 Spring을 사용하여 웹 서비스를 만들 수 있다.
- 자바 백엔드 개발자들은 웹 애플리케이션을 개발할 때 대부분 스프링을 사용한다고 한다. 아래의 사진으로 간략하게 스프링의 구조를 보자.
  <br/><br/>

![spring_architect](/assets/images/spring_architect.png)

<hr/>

## Spring 핵심 개념

- Spring은 자바 객체와 라이브러리들을 관리해주며, 톰캣과 같은 WAS가 내장되어 있어 자바 웹 어플리케이션을 구동할 수 있다.
- Spring은 경량 컨테이너로 자바 객체를 직접 Spring 안에서 관리하며 객체의 생성과 소멸과같은 생명주기(Life cycle)을 관리하며, Spring 컨테이너에서 필요한 객체를 가져와 사용한다

### Spring의 가장 큰 특징으로 IOC와 DI가 많이 언급된다

- Java 프로그램에서는 각 객체들이 프로그램의 흐름을 결정하며 각 객체를 직접 생성하고 조작하는(객체를 직접 생성하여 메소드 호출)작업을 했다. 즉 모든 작업을 사용자가 제어하는 구조였다.
- 하지만 Spring의 IOC는 객체의 생성을 특별한 관리 위임 주체에게 맡긴다. 사용자는 객체를 직접 생성하지 않고 객체의 생명주기를 컨트롤 하는 주체는 다른 주체가 된다. 즉 <strong>IOC(Inversion of Control)</strong> 제어가 역전이 되는 것이다. Spring에서는 이를 전담하는 Spring Container라는 것이 존재하며, 이 Container에 제어를 맡긴 객체를 빈(Bean)이라고 한다.
  spring-boot-start-jdbc는 jdbcTemplate를 위함이고, mysql-connector-java는 mysql을 기반으로 connection을 하기 위함이다.
- IOC는 DI로 구현된다. DI는 객체 내부가 아닌, 외부에서 객체와 객체를 자동으로 연결해주는 것이다. 외부에서 의존성을 넣어줬으므로 <strong>DI(Dependency Injection)</strong> 의존성 주입 이라고 하는 것이다.
<hr/>

## Spring Boot란?

Spring Boot는 Spring을 더 쉽게 이용하기 위한 도구라고 생각하면 된다. Spring에서는 Dependency 에서의 버전관리를 하나하나 다 해줘야하지만 Spring Boot에서는 설정도 보다 간편하며 버전 관리도 자동으로 해준다. Spring에서의 Configuration의 설정은 매우 길도 모든 어노테이션 및 빈 등록을 설정 해 줘야하지만 Spring boot에서는 implementation 설정 한줄이면 해결된다.
<br/><br/>

![bootisking](/assets/images/bootisking.jpg)

<I><small>Spring Boot 쓰자 그냥</small></I>
