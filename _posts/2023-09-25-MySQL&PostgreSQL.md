---
layout: post
title: " [DB]- MySQL과 PostgreSQL 비교 분석"
categories: DataBase
tags: [DataBase, Mysql, PostgreSQL, DBMS, RDBMS]
---

## MySQL 이란?

<span style = "color:#FF8C00">MySQL</span>은 세계에서 가장 많이 쓰이는 오픈 소스 데이터베이스이며, 표준 데이터베이스 질의 언어 <span style = "color:#FF8C00">SQL(Structed Query Language)</span>를 사용하는 오픈 소스의 <span style = "color:#FF8C00">관계형 데이터베이스 관리 시스템(RDBMS)</span>이며, 매우 빠르고, 유연하며, 사용하기 좋은 특징이 있다.

관계형 데이터베이스란 데이터가 하나 이상의 열과 행의 레이블에 저장되어 서로 다른 데이터 구조가 어떻게 관련되어 있는지 쉽게 파악하고 이해할 수 있도록 사전 정의된 관계로 데이터를 구성하는 정보 모음이다.

- 사용자에게 데이터를 관계로써 표현한다. 즉, 행과 열의 집합으로 구성된 테이블의 묶음 형식으로 데이터를 제공한다
- 테이블 형식의 데이터를 조작할 수 있는 관계 연산자를 제공한다.

<div style = " font-weight:bold">
<span style = "font-weight:bold;font-size:20px;color:#CC8C00">MySQL 특징</span><br/>

1. Performance - 대용량 데이터를 효율적으로 처리하도록 설계 되었으며 많은 동시 연결을 처리 할 수있다(인덱싱, 캐싱, 저장프로시저)<br/>

2. Reliability - 오류를 정상적으로 처리하여 신뢰할 수 있도록 설계되었다(복제, 백업 및 복구, 트랜잭션 지원)<br/>

3. Scalability - 대량의 데이터를 처리하고 확장할 수 있도록 설계되었다(파티셔닝, 샤딩, 클러스터 지원)<br/>

4. High Availability Solutions- 장애를 처리하고 고가용성 서비스를 제공할 수 있다(복제, 로드 밸런싱)<br/>

</div><br/>

<hr/>

## PostgreSQL 이란?

<span style = "color:#FF8C00">PostgreSQL</span>은 오픈 소스의 <span style = "color:#FF8C00">객체-관계형 데이터베이스 시스템(ORDBMS)</span>이며, 연산자, 복합 자료형, 집계 함수, 자료형 변환자, 확장 기능 등 다양한 데이터베이스 객체를 사용자가 임의로 만들 수 있는 기능을 제공함으로써 마치 하나의 새로운 프로그래밍 언어처럼 기능을 손쉽게 구현할 수 있다.

<div style = " font-weight:bold">
<span style = "font-weight:bold;font-size:20px;color:#CC8C00">PostgreSQL 특징</span><br/>

1. Reliability - 트랜젝션 ACID에 대한 구현으로 신뢰 가능(로우, 레벨, 라킹)<br/>

2. Scalability - 대용량 데이터 처리를 위한 테이블 파티셔닝등 구현 가능<br/>

3. Recovery & Availability - streaming replication(데이터베이스 복제 기술 중 하나)동기식/비동기식 상시대기 서버 구축 가능, Point in time recovery가능<br/>

4. Advanced - 웹 또는 C/S 기반의 GUI 관리 도구를 제공하여 모니터링 및 관리, 튜닝까지 가능, 사용자 정의로 Perl, Java, php등의 스크립터 언어 지원 가능<br/>

</div><br/>

<hr/>

## Mysql vs PostgreSQL 유사점

<div style = " font-weight:bold">

- 구조화된 쿼리 언어(SQL)를 인터페이스로 사용하여 데이터를 읽고 수정 가능.

- 오픈 소스이며 든든-한 개발자 커뮤니티 지원 제공

- 데이터 백업, 복제 및 엑세스 제어 기능이 내장되어 있음

</div><br/>

## Mysql vs PostgreSQL 차이점

|                   |                                MySQL                                 |                                              PostgreSQL                                               |
| :---------------: | :------------------------------------------------------------------: | :---------------------------------------------------------------------------------------------------: |
| 데이터베이스 기술 |                 순수 관계형 데이터베이스 관리 시스템                 |                                 객체 관계형 데이터베이스 관리 시스템                                  |
|       기능        | 보기, 트리거 및 프로시저와 같은 데이터 베이스 기능을 제한적으로 지원 |   구체화된 보기, INSTEAD OF 트리거, 여러 언어의 저장 프로시저와 같은 최고급 데이터 베이스 기능 지원   |
|    데이터 유형    |       숫자, 문자, 날짜 및 시간, 공간, JSON 데이터 유형을 지원        | 기하학, 열거형, 네트워크 주소, 배열, 범위, XML, hstore, 복합을 포함하여 모든 MySQL 데이터 유형을 지원 |
|  ACID 규정 준수   |        InnoDB 및 NDB 클러스터 스토리지 엔진에서만 ACID를 준수        |                                           항상 ACID와 호환                                            |
|      인덱스       |                    B-트리 및 R-트리 인덱스를 지원                    |          트리와 함께 표현식 인덱스, 부분 인덱스, 해시 인덱스와 같은 여러 인덱스 유형을 지원           |
|       성능        |                  높은 빈도의 읽기 작업 성능을 개선                   |                                   높은 빈도의 쓰기 작업 성능을 개선                                   |
|    초보자 지원    |                                 쉽다                                 |                                               복잡하다                                                |

<hr/>

## Mysql vs PostgreSQL 선택

**PostgreSQL**은 쓰기 작업이 빈번하고 쿼리가 복잡한 애플리케이션에 더 적합하다. 하지만 프토로타입을 만들거나, 사용자 수가 적은 애플리케이션을 만들거나, 읽기 횟수가 많고 데이터 업데이트가 자주 이루어지지 않는다면 **MySQL**을 사용하는 것이 이롭다.
