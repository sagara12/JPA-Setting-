#JPA 세팅 문제 해결
## Setting 문제
* H2 DB와 JPA 연결 문제
* 쿼리문은 제대로 출력 되는데 H2 데이터베이스에 테이블이 생성 안됨
* 오류 없음


## 참고 사이트

* https://okky.kr/article/707417
* https://nyximos.tistory.com/80
* https://java.ihoney.pe.kr/264
* https://wadekang.tistory.com/27

## 예상 되는 문제점
* 엔티티에 있는 변수명이 DB에 있는 예약어와 겹친다
* yml파일의 띄어쓰기가 잘못되었다.
* 엔티티및 레포지토리의 폴더 뎁스가 달라서 하이버네이트 및 JPA가 자동인식을 하지못한다


## 해결방법
> 엔티티및 레포지토리의 폴더 뎁스가 달라서 하이버네이트 및 JPA가 자동인식을 하지못한다.

```JAVA
package com.datePage;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.domain.EntityScan;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;

@EntityScan(basePackages = {"com.datePage.request"}) // com.datePage.request.domain 하위에 있는 @Entity 클래스 scan*/
@SpringBootApplication
@EnableJpaRepositories(basePackages = {"com.datePage.repository"}) // com.datePage.datePage.repository 하위에 있는 jpaRepository를 상속한 repository scan*/
public class DatePageApplication {

	public static void main(String[] args) {
		SpringApplication.run(DatePageApplication.class, args);
	}

}
```

> 엔티티에 있는 변수명이 DB에 있는 예약어와 겹친다.

```JAVA
package com.datePage.request.domain;

import lombok.AccessLevel;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.*;

@Entity
@NoArgsConstructor(access = AccessLevel.PUBLIC)
@Getter
@Table(name = "Document_Write")
public class Write {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    @Column(name = "write_number")
    private Long write_id;

    @Column(name = "write_title")
    private String title;

    @Column(name = "write_content")
    private String content;

    @Builder
    public Write(String title, String content) {
        this.title = title;
        this.content = content;
    }

}
```

>  yml파일의 띄어쓰기가 잘못되었다.
* application.yml
 ```JAVA

spring:
  h2:
    console:
      enabled: true
      path: /h2-console

  datasource:
    url: jdbc:h2:mem:datePage # test 부분을 자신이 원하는 것으로 바꾸시면 됩니다.
    username: sa # username과 password는 자신의 설정에 맞게
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create # 어플리케이션을 시작할 때 데이터베이스를 초기화하고 다시 테이블 생성
    properties:
      hibernate:
        format_sql: true # 실행되는 query를 보여줌

logging.level:
  org.hibernate.SQL: debug
```

## 실행
* localhost:8080/h2-console로 데이터 베이스 콘솔 접속
* jdbc:h2:mem:datePage URL로 
