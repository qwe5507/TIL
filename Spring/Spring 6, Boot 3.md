# Spring 6, Boot 3

## Spring 5.x → 6.0

- Java 17 기반으로 변경
- 일부 Java EE API 지원 종료
- XML이 Spring 에서는 점차적으로 사라지게 될 것
- RPC 지원 종료
- 새로운 AOT 엔진 도입
- @Inject 같은 어노테이션들이 jakarta.annotaion 패키지로 변경
- jakarta EE 로의 변경
    - hibernate ORM 5.6.x 버전부터 hibernate-core-jakarta 사용
    - 영속성 어노테이션 패키지 이름 javax.persistence -> jakarta.persistence로 변경
    - Tomcat 10, Jetty 11, Undertow 2.2.14 로 업그레이드 필요
    - javax.servlet -> jakarta.servlet 으로 변경
- url에서 마지막의 "/" 매칭 기능 지원 X("/home/" 과 "/home" 을 구분한다.)

## Spring Boot 3.0 요구사항

- Java Version : Java 17+
- Gradle 7.5+, Maven 3.5+
- Hibernate 6.1
- Spring Framework 6
- Jakarta EE 9 +
- Kotlin 사용시 1.6+ 버전 사용 필요
- Servlet, JPA 네임스페이스 -> Jakarta 로 대체이전에 사용하던 JPA의 패키명은 javax.* 이었으나 jakarta.* 로 대체

## Spring Boot 3.0 업그레이드

- https://github.com/spring-projects/spring-boot/wiki/Spring-Boot-3.0-Migration-Guide#spring-security

1. 스프링 부트 버전의 의존성을 체크
    - 스프링 부트 2.7 이전버전 사용 시, 2.7 버전 으로 먼저 마이그레이션 후 3.0으로 업그레이드 하는 것을 권고
2. Spring Security
    - 스프링  부트 3.0은 스프링 시큐리티 6.0을 사용
    - 이전 버전을 사용하고 있다면 5.8버전으로 먼저 마이그레이션 하고 6.0으로 업그레이드 하는 것을 권고
    - 개인적으로 인가 API가 많이 변경되어, 따로 학습이 필요
        - **Voter등의 클래스를 사용하지 않고, AuthorizationFilter가 인가 처리를 담당**
3. javax -> jakarta
    - Java EE에서 Jakarta EE 로 전환되면서 생기는 에러.
    - javax 로 시작하는 패키명은 jakarta로 변경해준다.
        - Ex)javax.persistence.* -> jakarta.persistence.*javax.validation.* -> jakarta.validation.*javax.servlet.* -> jakarta.servlet.*javax.annotation.* -> jakarta.annotation.*javax.transaction.* -> jakarta.transaction.*
    - javax.sql., javax.crypto. 패키지는 jakarta로 변화가 없다.
    - Java EE에서 제공하는 패키지가 아닌 JDK 에서 제공하는 패키지이기 때문
4. QueryDsl 설정
    
    javax.persistence.*에서 jakarta.persistence.*로 변경되면서 QueryDsl관련 Gradle 혹은 maven 설정 변경이 필요하다.
    
    - build.gradle
    
    ```jsx
    dependencies {
        // 
        implementation 'com.querydsl:querydsl-jpa:5.0.0:jakarta'
        annotationProcessor "com.querydsl:querydsl-apt:${dependencyManagement.importedProperties['querydsl.version']}:jakarta"
        annotationProcessor "jakarta.annotation:jakarta.annotation-api"
        annotationProcessor "jakarta.persistence:jakarta.persistence-api"s
        // 
    }
    ```
    

# Spring Boot 3.0

## AOT(Ahead Of Time) Compiler

JIT 컴파일러와 AOT 컴파일러 모두 바이트코드를 기계어로 생성한다. 

JIT 컴파일러가 바이트코드를 런타임에 기계어로 바꾼다면 **AOT는 실행 전에 바이트코드를 기계어로 바꾸는 컴파일러** 

- AOT컴파일러는 실행하기 전에 코드에 대한 정적 코드 테스트를 진행하고 그것을 기반으로 기계어를 생성
- JIT컴파일러는 런타임에 기계어를 생성

AOT컴파일러

- 실행 전에 무겁고 복잡한 분석 및 최적화가 수행됨
- 런타임에 실행하는 속도가 빠름
- 아직코드 최적화 부족

JIT컴파일러

- 상황에 맞춘 최적화 코드를 생성 할 수 있음 (할당받은 코어, OS, 커널버전, CPU)
- 런타임에 오버헤드가 발생
- C1, C2 컴파일러로 구성되어 있음

컴파일러 별로 장단점이 있기 때문에, 상황에 따라 적용 해야 한다.

## GraalVM

## Reference

- https://www.youtube.com/watch?v=ii6Iww6BCVI
- https://www.youtube.com/watch?v=1WT6oxchM9M
- https://marrrang.tistory.com/m/101?category=925235
- https://abbo.tistory.com/m/365
- https://covenant.tistory.com/279
