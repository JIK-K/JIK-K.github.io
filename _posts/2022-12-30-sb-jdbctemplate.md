---
layout: post
title: "SpringBoot - jdbcTemplate 사용법"
categories: springboot
tags: [Spring Boot, jdbcTemplate]
---

JdbcTemple은 데이터를 저장하기 위해 도와주는 API이다(SQL Mapper)<br/>
ㄴ SQL Mapper는 SQL을 직접 입력하고 Object의 필드를 매핑하여 데이터를 객체화하는것.

- <I>build.gradle</I>

```
//database
implementation 'org.springframework.boot:spring-boot-starter-jdbc'
runtimeOnly 'mysql:mysql-connector-java'
```

spring-boot-start-jdbc는 jdbcTemplate를 위함이고, mysql-connector-java는 mysql을 기반으로 connection을 하기 위함이다.

- <I>application.properties</I>

```
//DB
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/xxxxxx
spring.datasource.username=xxxxx
spring.datasource.password=xxxxx
```

DB의 경로와 주소 아이디와 패스워드를 입력한다

- <I>DB구조</I>
  ![springboot-jdbctemplate](/assets/images/springboot-jdbctemplate.png)
  UserTB 테이블에는 userId, userPassword.. 등등 컬럼들이 있다.

- <I>jdbc 클래스 선언</I>

```java
@Autowired
JdbcTemplate jdbcTemplate;
```

jdbctemplate를 사용하기위해 @Autowired로 bean을 선언한다.

- <I>Query(update, insert, select)</I>

```java
public String insertRegisterData(User user){
        String success = "False";
        String sql = "INSERT INTO UserTB(userId, userPassword, nickName, userName, birthDate, phoneNum, userMail)" +
                "VALUES(?,?,?,?,?,?,?)";
        int result = jdbcTemplate.update(sql, user.getId(), user.getPw(),
                user.getNickname(), user.getName(), user.getBirth(), user.getPhone(), user.getEmail());
        if(result != 0)
            success = "true";
        return success;
    }
```

jdbcTemplate의 update 메소드는 insert, select, update 모두 사용 가능하며 사용방법은 쿼리, 나머지 파라미터를 나열해주면 되며, return은 처리된 카운트로 int형으로 반환된다

- <I>QueryForObject</I>

```java
public String selectLoginData(User user){
        String success = "False";
        String sql = "SELECT count(*) FROM UserTB WHERE userId=? AND userPassword=?";
        int result = jdbcTemplate.queryForObject(sql, Integer.class, user.getId(), user.getPw());
        if(result != 0)
            success = "true";
        return success;
    }
```

queryForObject는 단일 건을 가져오기 위하여 사용한다. 작동방식은 update 메서드와 유사하다.
