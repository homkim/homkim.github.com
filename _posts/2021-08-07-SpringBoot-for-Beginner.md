---
title: Spring Boot 입문
layout: single
author_profile: true
read_time: true
comments: true
share: true
related: true
categories:
- JAVA
- Spring Boot
toc: true
toc_sticky: true
toc_label: 목차
description: Spring Boot
article_tag1: Java
article_tag2: Spring Boot
article_tag3: 게시판
article_section: Spring Boot 공부하기
meta_keywords: Java, Spring Boot, Spring WEB, Mustache, JPA
last_modified_at: '2021-08-07 07:31:02'
---

> 이 글은 유튜버 홍팍님의 강의를 정리한 글입니다.  
> 다른 점은 linux 데스크탑에서 vscode를 사용했습니다.

사용한 툴 : 
* Linux Mint 20.1
* OpenJDK-11-jdk
* Visual Studio Code 1.58.2

## 01.스프링부트란
> Q : 스프링부트란 무엇인지 설명하시오

* 자바프로그램을 쉽고 빠르게 만들기 위한 도구
* 프로그램을 만들기 위한 레시피가 존재하고 원하는 레시피를 찾고 내것으로 만들기만 하면 됩니다.

## 02.개발환경 설정
> Mission : Spring Boot 개발환경을 만들고 "Hello World" 출력하시오.

* jdk 설치 : sudo apt install openjdk-11-jdk
* ide 설치 : VS Code (기타: Eclipse, IntelliJ,, )
* Spring Boot Project

### Spring Boot Project 생성 >>

#### ① start.spring.io 페이지에서 만드는 방법
1. Project : Gradle Project
2. Language : Java
3. Spring Boot : 최신버전 암꺼나. 예) 2.4.1
4. Project Metadata :   
5. Group : com.example
6. Artifect : 프로젝트명
7. Package : Jar
8. Java : 11 (로컬에 깔려 있는 버전)
9. Dependency (나중에 추가하면 됨)  
Spring Web  
H2 Database (데이타베이스, 메모리 또는 파일 가능)  
Mustache (MVC의 View를 담당)  
Spring Data JPA (데이타베이스 연동 기능)  

다운로드 받은 zip파일 압축을 풀고 vscode로 Open

#### ② vscode에서 프로젝트 만들기
1. Java Extension Package 설치
2. Spring Boot Extension Package 설치
3. Spring Initializer: Create a gradle Project... 실행
1. Project : Gradle Project
2. Language : Java
3. Spring Boot : 최신버전 암꺼나. 예) 2.4.1
4. Project Metadata :   
5. Group : com.example
6. Artifect : 프로젝트명
7. Package : Jar
8. Java : 11 (로컬에 깔려 있는 버전)
9. Dependency (나중에 추가하면 됨)  
Spring Web  
H2 Database (데이타베이스, 메모리 또는 파일 가능)  
Mustache (MVC의 View를 담당)  
Spring Data JPA (데이타베이스 연동 기능)  

#### ③ "Hello World" 출력하기
src/main/resources/static/index.html 파일 생성  
브라우저에서 localhost:8080/index.html 열어서 확인  
페이지 없다고 나오면 서버 재시작  
서버 실행: Spring Boot Dashboard에서 app 실행

## 03.웹서비스 동작원리
> Mission : "Hello World"가 출력되는 과정을 설명하시오

* 웹페이지는 Client가 Request하고 Server가 이에 응답(Response)하는 구조로 실행됨
* 서버 실행1 : Spring Boot Dashboard에서 실행
* 서버 실행2 : <ArtifactID>Application.java 실행해도 됨 (IntelliJ)
* localhost 서버의 8080 포트에서 실행 중
* 서버 실행로그에서 서버가 실행된 포트(8080) 확인이 가능
* localhost:8080/hello.html 과 같이 파일명을 직접 명시하게되면  .../resources/static 디렉토리에서 해당 파일을 찾는다.

## 04.뷰 템플릿과 MVC패턴
> Mission : MVC 패턴을 활용한, 템블릿 페이지를 만드시오.

### MVC 기본
* View : Presentation 영역 담당  
* Control : Client의 요청을 분석하여 처리과정 담당  
* Model : 데이타 처리 담당  

html과 다르게 동적으로 페이지를 보여주기 위해서는 template 파일이 필요함  
/src/main/resources/templates 디렉토리에서 view를 위한 템플릿 파일을 생성해야함  

### ① 파라미터 없이 구성

**greetings.mustache**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>유저님, 반갑습니다!</h1>
</body>
</html>
```

/src/main/java/com/example/myapp/ 디렉토리에 controller 디렉토리를 만들고 아래에 controller를 생성함

**MyappController.java**
1. Controller 선언
2. 함수생성: view template명 리턴
3. GetMapping 어노테이션 설정

```java
package com.example.myapp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class MyappController {
    
    @GetMapping("/hi")
    public String niceToMeetYou(){

        return "greetings";  // templates/greetings.mustache를 브라우저로 전송
    }
}
```

### ② 파라미터 전달 구성

**mustache 기본**  
* \{\{parameter\}\} : 파라미터 전달  
* \{\{>filename\}\} : 파일명 전달  

**greeting.mustache**
* 변수로 username 선언

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>\{\{username\}\}님, 반갑습니다!</h1>
</body>
</html>
```

**MyappController.java**
* Contoller 함수의 인자값에 Model model 선언
* addAttribute 함수를 통해 전달할 파라미터 할당

```java
package com.example.myapp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class MyappController {
    
    @GetMapping("/hi")
    public String niceToMeetYou(Model model){

        model.addAttribute("username", "홍길동");
        return "greetings";  // templates/greetings.mustache를 브라우저로 전송
    }
}

```


## 05.mvc의 역할과 실행 흐름
> Mission : 뷰 템플릿 페이지가 출력되기까지, MVC의 역할과 실행 흐름을 설명하시오.

### ① 기본 흐름
1. Controller가 Client의 localhost:8080/hi 페이지 요청을 받음
2. Model은 username에 값을 전달
3. View Template은 Model 로 부터 받은 데이터를 가지고 화면을 출력

### ② goodbye 페이지 만들기
* view template 생성합니다.

**goodbye.mustache**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>\{\{nicname\}\}님, 다음에 만나요!</h1>
</body>
</html>
```

**controller 생성 및 model 값 전달**

```java
    @GetMapping("/bye")
    public String seeYouNext(Model model){

        model.addAttribute("nicname", "배고픈사자");
        return "goodbye";  // templates/greetings.mustache를 브라우저로 전송
    }
```


## 06.뷰 템플릿과 레이아웃
> Mission : 뷰 템플릿 페이지에 헤더-푸터 레이아웃을 적용하시오.

view template 에서 header와 footer를 만들어서 모듈화하는 과정입니다.

### ① 페이지 기본레이아웃 설정
① getbootstrap.com 페이지에서 get started > Starter Template 을 그대로 복사해와서 greetings와 goodbye를 수정합니다.

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript; choose one of the two! -->

    <!-- Option 1: Bootstrap Bundle with Popper -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-U1DAWAznBHeqEIlVSCgzq+c9gqGAJn5c/t99JyeKa9xxaYpSvHU5awsuZVVFIhvj" crossorigin="anonymous"></script>

    <!-- Option 2: Separate Popper and Bootstrap JS -->
    <!--
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js" integrity="sha384-eMNCOe7tC1doHpGoWe/6oMVemdAVTMs2xqW4mwXrXsW0L84Iytr2wi5v2QjrP/xp" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.min.js" integrity="sha384-cn7l7gDp0eyniUwwAZgrzD06kc/tftFf19TOAs2zVinnD/C7E91j9yyk5//jjpt/" crossorigin="anonymous"></script>
    -->
  </body>
</html>
```

### ② Navigation Bar 설정
부트스트랩에서 navbar를 검색하여 코드를 가져옵니다.

```html
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Link</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Dropdown
          </a>
          <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
            <li><a class="dropdown-item" href="#">Action</a></li>
            <li><a class="dropdown-item" href="#">Another action</a></li>
            <li><hr class="dropdown-divider"></li>
            <li><a class="dropdown-item" href="#">Something else here</a></li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
        </li>
      </ul>
      <form class="d-flex">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>
```

### ③ 모듈화
greetings.mustache 파일을 위 두개의 소스로 재구성한 뒤에 templates/layouts 디렉토리를 생성하고 그 아래에 header.mustache와 footer.mustache 파일을 생성합니다.  
header - content - footer 영역으로 단순 잘라내서 각각의 파일로 구성하면 됩니다.  
최종 모습은 아래와 같습니다.  

**header.mustache**

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
          <a class="navbar-brand" href="#">Navbar</a>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarSupportedContent">
            <ul class="navbar-nav me-auto mb-2 mb-lg-0">
              <li class="nav-item">
                <a class="nav-link active" aria-current="page" href="#">Home</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Link</a>
              </li>
              <li class="nav-item dropdown">
                <a class="nav-link dropdown-toggle" href="#" id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false">
                  Dropdown
                </a>
                <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
                  <li><a class="dropdown-item" href="#">Action</a></li>
                  <li><a class="dropdown-item" href="#">Another action</a></li>
                  <li><hr class="dropdown-divider"></li>
                  <li><a class="dropdown-item" href="#">Something else here</a></li>
                </ul>
              </li>
              <li class="nav-item">
                <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
              </li>
            </ul>
            <form class="d-flex">
              <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
              <button class="btn btn-outline-success" type="submit">Search</button>
            </form>
          </div>
        </div>
      </nav>
    
```
**footer.mustache**

```html
    <!-- footer -->
    <div>
        <hr>
        @example - <a href="#">Privacy</a> - <a href="#">Terms</a></p>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-U1DAWAznBHeqEIlVSCgzq+c9gqGAJn5c/t99JyeKa9xxaYpSvHU5awsuZVVFIhvj" crossorigin="anonymous"></script>

  </body>
</html>
```
**greetings.mustache**

```html
\{\{>layouts/header\}\}    

<!-- content -->
<div class="bg-dark text-white p-5">
    <h1>\{\{username\}\}님, 반갑습니다!</h1>

</div>

\{\{>layouts/footer\}\}    

```


## 07.폼 데이터 주고 받기
> Mission : 사용자로부터 폼 데이터를 받고, 이를 컨트롤러에서 확인하시오.

### ① view 생성
* action에 컨트롤러가 받기 위한 주소를 입력합니다.
* method는 보안 때문에 post방식을 써야 합니다.
* form 객체로 데이터를 보내기 위해서는 각 항목의  name에 이름을 적어줘야 합니다.
(예, name="title")


**.../templates/articles/new.mustache**
```html
\{\{>layouts/header\}\}

<form class="container" action="/articles/create" method="post">
    <div class="mb-3">
        <label class="form-label">제목</label>
        <input type="text" class="form-control" name="title">
    </div>

    <div class="mb-3">
        <label class="form-label">내용</label>
        <textarea class="form-control" rows=3 name="content" ></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>

\{\{>layouts/footer\}\}
```

### ② controller 생성
* form에서 submit을 눌렀을 때 처리하기 위함
* form에서 method를 post로 했기 때문에 PostMapping으로 받아야 함

**.../controller/ArticleController.java**
```java
package com.example.myapp.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import dto.ArticleForm;

@Controller
public class ArticleController {
    
    @GetMapping("/articles/new")
    public String newArticleForm(){

        return "articles/new";
    }

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form){
        System.out.println(form.toString());
        return "articles/new";
    }
}
```

### ③ model 생성
* form의 값을 java로 가져오기 위해 정의함

**.../dto/ArticleForm.java**
```java
package dto;

public class ArticleForm {
    private String title;
    private String content;
    
    public ArticleForm(String title, String content) {
        this.title = title;
        this.content = content;
    }

    @Override
    public String toString() {
        return "ArticleForm [title=" + title + ", content=" + content + "]";
    }
}

```

### ④ 테스트
localhost/articles/new 로 접속하여 화면에 값을 입력하고
서버 로그에 해당 값이 잘 넘어오는지 확인해봅니다.

콘솔 로그 화면에 title과 content 값이 찍히는지 확인합니다.


## 08.데이터 생성 with JPA
> Mission : JPA를 활용하여, DB에 데이터를 생성하시오.

### DB 선택
oracle, mysql, mariadb, postgresql, h2, ,,,  
실습은 프로젝트 생성시 셋팅한 h2 db 활용합니다.  
DB는 자바를 이해하지 못하기 때문에 JPA가 필요합니다.  
jpa 핵심도구는 Entity와 Repository 두가지가 있습니다.  


### 데이터 저장 과정
DTO > Controller > Entity > Repository > save() -> DB

### DB에 저장하기 중 form 데이터를 Entiry로 변환하기

* DB에 저장하기 위해 우선 Form데이터를 Entity로 변환해야 하며
* Entity데이터를 Repository를 통해서 save()하면 DB에 데이터가 저장된다.
* 우선 코드를 먼저 작성한 후에 에러를 해결하는 방식으로 진행한다.


**.../ArticleController.java**
1. DTO를 Entity로 변환
2. Repository에게 Entity를 DB안에 저장

```java
package com.example.myapp.controller;

import com.example.myapp.entity.Article;
import com.example.myapp.repository.ArticleRepository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import dto.ArticleForm;

@Controller
public class ArticleController {
    
    @Autowired
    private ArticleRepository articleRepository;

    @GetMapping("/articles/new")
    public String newArticleForm(){

        return "articles/new";
    }

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form){
        System.out.println(form.toString());

        // 1.DTO를 Entity로 변환
        Article article = form.toEntity();
        System.out.println(article.toString());

        // 2.Repository에게 Entity를 DB안에 저장
        Article saved = articleRepository.save(article);
        System.out.println(saved.toString());

        return "articles/new";
    }
}

```

### Article 클래스 생성
좌측의 Article article 의 에러를 해결하기 위해 Article 클래스를 생성함니다.

**.../entity/Article.java**
1. entity package 생성
2. 그 안에 Article 클래스 생성
3. DB가 해당 객체 인식가능하도록 @Entity 어노테이션 추가
4. title, content를 컬럼으로 정의하고 Column 어노테이션 설정
5. 테이블에 담길 Key 설정을 위해 Id 컬럼을 정의하고 @Id 어노테이션 지정 및 값을 자동생성 하도록 설정

```java
package com.example.myapp.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

@Entity
public class Article {

    @Id // 대표값 지정
    @GeneratedValue // 1,2,3 자동생성
    private Long id;

    @Column
    private String title;

    @Column
    private String content;

    public Article(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }

    @Override
    public String toString() {
        return "Article [id=" + id + ", title=" + title + ", content=" + content + "]";
    }
    
}
```

### Form 데이터를 Entity로 변환
우측의 form.toEntity() 에러를 해결하기 위해 ArticleForm 클래스에 toEntity() 함수를 추가합니다.

**.../dto/ArticleForm.java**
1. toEntity 함수를 생성하고
2. Article 객체를 생성하여 리턴합니다.

```java
package dto;

import com.example.myapp.entity.Article;

public class ArticleForm {
    private String title;
    private String content;
    
    public ArticleForm(String title, String content) {
        this.title = title;
        this.content = content;
    }

    @Override
    public String toString() {
        return "ArticleForm [title=" + title + ", content=" + content + "]";
    }

    public Article toEntity() {

        return new Article(null, title, content);
    }
    
    
}
```


### Entiry 데이터를 Repository 통해 save 기능 구현

* Repository 기능은 CrudRepository를 상속하여 Interface로 구현해야 합니다.
* 관리대상 Entity Article과 키 값의 타입 Long을 지정해야 합니다.

**.../repository/ArticleRepository.java**

```java
package com.example.myapp.repository;

import com.example.myapp.entity.Article;

import org.springframework.data.repository.CrudRepository;

public interface ArticleRepository extends CrudRepository<Article, Long> {
    
}
```

### articleRepository 객체를 생성
* 객체 자동주입(DI)을 위해 @Autowired 설정
* 단계별 변환 및 저장이 잘 되었는지 확인하기 위해 form, article, saved의 내용을 확인

**.../controller/ArticleController.java**

```java
package com.example.myapp.controller;

import com.example.myapp.entity.Article;
import com.example.myapp.repository.ArticleRepository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import dto.ArticleForm;

@Controller
public class ArticleController {
    
    @Autowired // 스프링 부트가 미리 생성해놓은 객체를 가져다가 자동 연결
    private ArticleRepository articleRepository;

    @GetMapping("/articles/new")
    public String newArticleForm(){

        return "articles/new";
    }

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form){
        System.out.println(form.toString());

        // 1.DTO를 Entity로 변환
        Article article = form.toEntity();
        System.out.println(article.toString());

        // 2.Repository에게 Entity를 DB안에 저장
        Article saved = articleRepository.save(article);
        System.out.println(saved.toString());

        return "articles/new";
    }
}
```


## 09.DB 테이블과 SQL
> Mission : 저장된 데이터를 직접 DB에서 확인하시오.

이전시간에 저장한 데이터를 직접 DB에서 확인해봅시다.  
먼저 Project properties 파일에 console 사용으로 셋팅합니다.

**.../resources/application.properties**  
```java
spring.h2.console.enabled=true
```

그 다음 localhost:8080/h2-console 로 접속합니다.  
connect page 에서 jdbc url을 입력해야 하는데 서버 로그에서 찾습니다.  
로그의 맨 마지막 h2-console 문구를 찾아서 url을 복사하여 화면에 입력합니다.

```log
2021-08-08 10:17:24.957  INFO 24109 --- [  restartedMain] o.s.b.a.h2.H2ConsoleAutoConfiguration    : H2 console available at '/h2-console'. Database available at 'jdbc:h2:mem:424e04f6-3ba0-4c5d-a5d9-03c3220c712a'
```

테이블에 데이터가 잘 들어가 있는지 확인해보고  
insert, update, delete 등의 sql문을 실행해봅니다.



## 10.Lombok과 리팩터링
> Mission : 롬복을 활용하여 기존 코드를 리팩토링 해보세요.

DB에 데애터를 저장하는 복잡한 절차를 롬복을 통해 간소화 해봅시다.  
<br>
클래스를 생성할 때  getter(), setter(), constructor(), toString(),,, 등을  
매번 작성해야 하는데 이런게 생각보다 만만치 않습니다.  
<br>
이런 이유 때문에 간소화하기 위한 툴인 롬복이 등장했습니다.  
Refactoring이란 코드의 구조 성능을 개선하는 작업을 말하며 
Logging이란 일련의 과정을 기록하는 것입니다.  

### ① Refactoring
리펙토링은 constructor, getter, setter 등을 간소화하는 기능입니다.  
Annotation을 추가하면 해당 패키지가 import 되는 것을 확인할 수 있습니다.  

**.../dto/ArticleForm.java**
```java
package dto;

import com.example.myapp.entity.Article;

import lombok.AllArgsConstructor;
import lombok.ToString;

@AllArgsConstructor
@ToString
public class ArticleForm {
    private String title;
    private String content;
    
    // @AllArgsConstructor 으로 생성자 리펙토링
    /*
    public ArticleForm(String title, String content) {
        this.title = title;
        this.content = content;
    }
    */

    // @ToString 으로 리펙토링
    /*
    @Override
    public String toString() {
        return "ArticleForm [title=" + title + ", content=" + content + "]";
    }
    */

    public Article toEntity() {

        return new Article(null, title, content);
    }
    
    
}
```

**.../entity/Article.java**
```java
package com.example.myapp.entity;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

import lombok.AllArgsConstructor;
import lombok.ToString;

@Entity //DB가 해당 객체 인식가능
@AllArgsConstructor
@ToString
public class Article {

    @Id // 대표값 지정
    @GeneratedValue // 1,2,3 자동생성
    private Long id;

    @Column
    private String title;

    @Column
    private String content;

    // @AllArgsConstructor 으로 리펙토링
    /*
    public Article(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }
    */

    // @ToString 으로 리펙토링
    /*
    @Override
    public String toString() {
        return "Article [id=" + id + ", title=" + title + ", content=" + content + "]";
    }
    */
    
}
```

### ② Logging
System.out.println을 통해 로그를 출력하던 것을 롬복의 log 출력기능을 활용하여 구현합니다. 기존 방식의 경우 서버에 부하를 주기 때문에 사용하는 것을 지양해야 합니다.

log 기능을 사용하기 위해 @Slf4j 어노테이션을 추가하고 
기존 로그 출력기능을 log.info() 함수로 변경합니다.

**.../controller/ArticleController.java**
```java
package com.example.myapp.controller;

import com.example.myapp.entity.Article;
import com.example.myapp.repository.ArticleRepository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import dto.ArticleForm;
import lombok.extern.slf4j.Slf4j;

@Controller
@Slf4j
public class ArticleController {
    
    @Autowired
    private ArticleRepository articleRepository;

    @GetMapping("/articles/new")
    public String newArticleForm(){

        return "articles/new";
    }

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form){

        
        // 서버에 부하를 주는 문법으로 로깅으로 변경한다
        // 아래도 모두 변경해준다.
        /*
        System.out.println(form.toString()); 
        */
        log.info(form.toString());

        // 1.DTO를 Entity로 변환
        Article article = form.toEntity();
        //System.out.println(article.toString());
        log.info(article.toString());

        // 2.Repository에게 Entity를 DB안에 저장
        Article saved = articleRepository.save(article);
        //System.out.println(saved.toString());
        log.info(saved.toString());

        return "articles/new";
    }
}
```
### ③ AJAX 적용
데이터를 저장하고 목록을 가져올 때 전부 다시 가져오는데 건수가 적을 땐 상관없으나,  
데이타가 많을 경우 문제가 발생할 수 있어 필요한 데이타만 가져와서 화면을 경신해야 합니다.  

**new.mustache**
```html
{{>layouts/header}}


<div class="container">   
    <h1>Article 등록</h1>
    <hr>
    <p>articles/new.mustache</p>
</div>

<!-- form tag에서 method와 action 삭제. ajax로 보내기 때문에 js로 구현 -->

<form class="container">
    
    <div class="form-group">
        <label for="title">제목</label>
        <!-- name="title" 추가, DTO의 필드명과 일치해야 함! -->
        <input name="title" type="text" class="form-control" id="article-title" placeholder="제목을 입력하세요">
    </div>

    <div class="form-group">
        <label for="content">내용</label>
        <!-- name="content" 추가, DTO의 필드명과 일치해야 함! -->
        <textarea name="content" class="form-control" id="article-content" placeholder="내용을 입력하세요" rows="3"></textarea>
    </div>

    <!-- type="button"으로 변경, id값 추가 -->
    <button type="button" class="btn btn-primary" id="article-create-btn">제출</button>
</form>

<Script>
// article 객체 생성
var article = {

    // 초기화(이벤트 등록) 메소드
    init: function() {
        // 스코프 충돌 방지! (https://mobicon.tistory.com/189)
        var _this = this;
        // 생성 버튼 선택
        const createBtn = document.querySelector('#article-create-btn');
        // 생성 버튼 클릭 시, 동작 할 메소드를 연결!
        createBtn.addEventListener('click', _this.create);
    },

    // article 생성 메소드
    create: function() {
        // form 데이터를 JSON으로 만듬
        var data = {
            title: document.querySelector('#article-title').value,
            content: document.querySelector('#article-content').value,
        };

    // 데이터 생성 요청을 보냄
    // fetch(URL, HTTP_REQUEST)
    fetch('/api/articles', {
        method: 'POST', // POST 방식으로, HTTP 요청.
        body: JSON.stringify(data), // 위에서 만든 폼 데이터(data)를 함께 보냄.
        headers: {
          'Content-Type': 'application/json'
        }
    }).then(function(response) { // 응답 처리!
        // 요청 성공! 
        if (response.ok) {
        alert('게시글이 작성 되었습니다.');
        window.location.href='/articles'; // 해당 URL로 브라우저를 새로고침!
        } else { // 요청 실패..
        alert('게시글 작성에 문제가 생겼습니다.');
        }
    });
    }
};
// 객체 초기화
article.init();
</script>

<!--  원래 소스:: AJAX 구현을 위해 주석처리함
<form class="container" action="/articles/create" method="post">
    <div class="jumbotron">
        <h1>Article 등록</h1>
        <hr>
        <p>articles/new.mustache</p>
    </div>

    <div class="mb-3">
        <label class="form-label">제목</label>
        <input type="text" class="form-control" name="title">
    </div>

    <div class="mb-3">
        <label class="form-label">내용</label>
        <textarea class="form-control" rows=3 name="content" ></textarea>
    </div>
    <button type="submit" class="btn btn-primary">Submit</button>
</form>
-->

{{>layouts/footer}}
```


**REST API 구현**

**.../api/ArticleApiController.java**
기존 ArticleController 있던 기능을 Rest로 다시 구성합니다.(@RestController)

```java
package com.example.myapp.api;

import com.example.myapp.dto.ArticleForm;
import com.example.myapp.entity.Article;
import com.example.myapp.repository.ArticleRepository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import lombok.extern.slf4j.Slf4j;

@Slf4j
@RestController
public class ArticleApiController {

    @Autowired
    private ArticleRepository articleRepository;
    
    @PostMapping("/api/articles") // Post 요청이 "/api/articles"로 들어오는 경우 처리
    public Long create(@RequestBody ArticleForm form) { // Json 데이터를 받아옴
        log.info(form.toString() + "::form"); // 받아온 데이터 확인

        Article article = form.toEntity();
        log.info(article.toString() + "::article");

        Article saved = articleRepository.save(article);
        log.info(saved.toString() + "::saved");

        return saved.getId();
    }
    
}
```

**entity/Article.java**
```java
@Getter // 게터를 자동 생성!
@ToString // toString() 자동 생성!
@NoArgsConstructor // 디폴트 생성자 넣어 줌!
@Entity // DB 테이블에 저장될 클래스 임!
public class Article {
    @Id // PK(Primary Key)
    @GeneratedValue(strategy = GenerationType.IDENTITY) // DB에서 자동관리. 숫자 자동 증가(1,2,,,)
    private Long id;

    @Column(length = 100, nullable = false) 
    private String title;
    
    @Column(columnDefinition = "TEXT", nullable = false) // 
    private String content;

    @Builder // 호출하는 곳에서 인자를 순서에 상관없이 입력하기 위함
    public Article(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }
}
```

**dto/ArticleForm.java**
```java
@Data // 생성자(디폴트, All), 게터, 세터, toString 등 다 만들어 줌!
public class ArticleForm {
    private String title;
    private String content;
    // 빌더 패턴으로 객체 생성! 생성자의 변형. 입력 순서가 일치하지 않아도 됨.
    public Article toEntity() {
        return Article.builder()
                .id(null)
                .title(title)
                .content(content)
                .build();
    }
}
```

### ④ h2 file 적용하기
메모리 DB로는 작업이 매우 번거로움

**Application.properties**
* Application.yaml로 파일명을 변경하고 내용을 변경해준다.
* path는 localhost:8080/testdb로 접속
* 파일은 app root에 testdb로 생성됨
* AUTO_SERVER=TRUE 다중접속 허용

```yml
# application.properties -> application.yaml로 변경
# spring.h2.console.enabled=true

spring:
    # DB 설정
    h2:
        console:
            enabled: true
            path: /testdb
    datasource:
        driver-class-name: org.h2.Driver
        url: jdbc:h2:file:./testdb;AUTO_SERVER=TRUE
        username: sa
        password:

```

파일을 만들경우 테이블이 자동생성되지 않아 에러가 발생됩니다.  
테이블을 생성한 뒤에 실행하세요.  
```sql
CREATE TABLE article(
id Long PRIMARY KEY auto_increment, 
title VARCHAR(100),
content varchar(4096)
);
```



## 11.데이터 조회하기 with JPA
> Mission : 


## 12.데이터 목록조회
> Mission : 전체 목록을 가져와서 목록 화면에 출력하시오.


### 목록화면 생성

**articles/index.mustache**
```html
{{>layouts/header}}

<!-- jumbotron -->
<div class="jumbotron">
  <h1>Article 목록</h1>
  <hr>
  <p>articles/index.mustache</p>
  <a href="/articles/new" class="btn btn-primary">글쓰기</a>
</div>
<!-- articles table -->
<table class="table table-hover">
  <thead>
    <tr>
      <th>#</th>
      <th>제목</th>
    </tr>
  </thead>
  <tbody>
    <!-- 모델에서 보내준 articles를 사용! 데이터가 여러개라면 반복 출력 됨! https://taegon.kim/archives/4910 -->
    {{#articles}}
      <tr>
        <td>{{id}}</td> <!-- id 출력 -->
        <td>{{title}}</td> <!-- title 출력 -->
      </tr>
    {{/articles}}
  </tbody>
</table>

{{>layouts/footer}}
```

### view에 넘겨줄 데이터 처리
* @RequiredArgsConstructor 추가 및 private final ArticleRepository articleRepository;에 final 추가
* Iterable<Article> articleList = articleRepository.findAll(); 으로 모든 데이터 가져옴
* model.addAttribute("articles", articleList); view로 데이터 전달
* 글쓰기 버튼이 Get 방식으로 전환되어 Annotation 수정 @GetMapping("/articles/new")

**controller/ArticleController.java**
```java
package com.example.myapp.controller;

import com.example.myapp.dto.ArticleForm;
import com.example.myapp.entity.Article;
import com.example.myapp.repository.ArticleRepository;

// import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;

@Controller
@RequiredArgsConstructor // final 필드 값을 알아서 가져옴(@Autowired 대체)
@Slf4j
public class ArticleController {
    
    // @Autowired -> @RequiredArgsConstructor로 대체 : !!final 붙여야 함!!
    private final ArticleRepository articleRepository;

    @GetMapping("/articles")
    public String index(Model model){ //뷰로 전달하기 위한 모델 객체 자동삽입
        // 모든 Article을 가져옴
        // Iterable 인터페이스는 ArrayList의 부모 인터페이스
        Iterable<Article> articleList = articleRepository.findAll();

        // 뷰페이지로 articles 전달
        model.addAttribute("articles", articleList);

        // 뷰페이지 설정
        return "articles/index";
    }

    @GetMapping("/articles/new")
    public String newArticleForm(){

        return "articles/new";
    }

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form){

        
        // 서버에 부하를 주는 문법으로 로깅으로 변경한다
        // 아래도 모두 변경해준다.
        /*
        System.out.println(form.toString()); 
        */
        log.info(form.toString());

        // 1.DTO를 Entity로 변환
        Article article = form.toEntity();
        //System.out.println(article.toString());
        log.info(article.toString());

        // 2.Repository에게 Entity를 DB안에 저장
        Article saved = articleRepository.save(article);
        //System.out.println(saved.toString());
        log.info(saved.toString() + " : 잘 저장되었습니다!!");

        return "redirect:/articles";
    }
}

```


## 13.링크와 리다이렉트
> Mission : 목록에서 클릭하면 상세 페이지를 조회하게 만드세요.

### 메인화면 링크 추가
**articles/index.mustache**
* 제목에 링크를 추가합니다.
```html
{{>layouts/header}}

<!-- jumbotron -->
<div class="container">
  <h1>Article 목록</h1>
  <hr>
  <p>articles/index.mustache</p>
  <a href="/articles/new" class="btn btn-primary">글쓰기</a>
</div>
<!-- articles table -->
<div class="container">
<table class="table table-hover">
  <thead>
    <tr>
      <th>#</th>
      <th>제목</th>
      <th>내용</th>
    </tr>
  </thead>
  <tbody>
    <!-- 모델에서 보내준 articles를 사용! 데이터가 여러개라면 반복 출력 됨! https://taegon.kim/archives/4910 -->
    {{#articles}}
      <tr>
        <td>{{id}}</td> <!-- id 출력 -->
        <td>
          <a href="/articles/{{id}}">{{title}}</a> <!-- 링크 추가 -->
        </td>
        <td>{{content}}</td>
      </tr>
    {{/articles}}
  </tbody>
</table>
</div>
{{>layouts/footer}}
```

### 상세 화면 추가
**articles/show.mustache**
```html
{{>layouts/header}}
<!-- jumbotron -->
<div class="container">
  <h1>Article 상세보기</h1>
  <hr>
  <p>articles/show.mustache</p>
</div>
<!-- table -->
<div class="container">
<table class="table table-hover">
  <tbody>
    {{#article}}
      <tr>
        <th>글번호</th>
        <td>{{id}}</td>
      </tr>
      <tr>
        <th>제목</th>
        <td>{{title}}</td>
      </tr>
      <tr>
        <th>내용</th>
        <td>{{content}}</td>
      </tr>
    {{/article}}
  </tbody>
</table>
<a href="/articles" class="btn btn-success btn-block">목록으로</a>
</div>

{{>layouts/footer}}
```

### 상세화면으로 전환 및 데이터 전달
**controller/ArticleController**
* show() 함수를 추가합니다.
* 링크를 클릭하여 id 값을 가져오는 처리를 해줘야 합니다.

```java
package com.example.myapp.controller;

import com.example.myapp.dto.ArticleForm;
import com.example.myapp.entity.Article;
import com.example.myapp.repository.ArticleRepository;

// import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;

@Controller
@RequiredArgsConstructor // final 필드 값을 알아서 가져옴(@Autowired 대체)
@Slf4j
public class ArticleController {
    
    // @Autowired -> @RequiredArgsConstructor로 대체 : !!final 붙여야 함!!
    private final ArticleRepository articleRepository;

    @GetMapping("/articles")
    public String index(Model model){ //뷰로 전달하기 위한 모델 객체 자동삽입
        // 모든 Article을 가져옴
        // Iterable 인터페이스는 ArrayList의 부모 인터페이스
        Iterable<Article> articleList = articleRepository.findAll();

        // 뷰페이지로 articles 전달
        model.addAttribute("articles", articleList);

        // 뷰페이지 설정
        return "articles/index";
    }

    @GetMapping("/articles/new")
    public String newArticleForm(){

        return "articles/new";
    }

    @PostMapping("/articles/create")
    public String createArticle(ArticleForm form){

        
        // 서버에 부하를 주는 문법으로 로깅으로 변경한다
        // 아래도 모두 변경해준다.
        /*
        System.out.println(form.toString()); 
        */
        log.info(form.toString());

        // 1.DTO를 Entity로 변환
        Article article = form.toEntity();
        //System.out.println(article.toString());
        log.info(article.toString());

        // 2.Repository에게 Entity를 DB안에 저장
        Article saved = articleRepository.save(article);
        //System.out.println(saved.toString());
        log.info(saved.toString() + " : 잘 저장되었습니다!!");

        return "redirect:/articles";
    }

    @GetMapping("/articles/{id}")
    public String show(@PathVariable Long id, // url의 {id} 값을 변수화!
                       Model model) {
        // id를 통해 Article을 가져옴!
        Article article = articleRepository.findById(id).orElse(null);
        // article을 뷰 페이지로 전달
        model.addAttribute("article", article);
        return "articles/show";
    }
}
```

### JSON API로 데이터 가져오기

Article 객체를 JSON API로 가져오기 위한 작업입니다.
API형식으로 데이터를 받을 때, JSON을 사용한다. 왜 그럴까? 장점이 있기 때문이다. 어떤 장점? 바로 범용성이다.

**json api 사용이유**
클라이언트는 사실 수도 없다. 웹기반의 브라우저, 앱 기반의 스마트폰, IoT 등… 수도 없다. 수 많은 클라이언트 별, 맞춤 뷰 페이지를 만드는 것은 어렵다. 이를 해결하는 방법이 역할 분담이다. 서버는 데이터만 전달하고, 클라이언트는 이를 받아 화면에 보여주기로 하는 것이다. 이때 데이터는 JSON으로 나타낸다.

**DTO와 Entity는 굳이 나눠야 할까?**
비지니스의 데이터를 담고있는 Entity. 이는 쉽게 변하지 않는다. 그러나 화면에서 보여주는 데이터는 수시로 변한다. 따라서, 이를 구분하여 만드는 것이 더 효율적이다.

Entity가 식재료라면, DTO는 어느정도 개별 조리가 된 음식이랄까? 이를 최종적으로 플레이팅 하는 건, 클라이언트의 영역이다.

**api/ArticleApiController**

*  @GetMapping("/api/articles/{id}") 처리를 해줍니다.

```java
package com.example.myapp.api;

import com.example.myapp.dto.ArticleForm;
import com.example.myapp.entity.Article;
import com.example.myapp.repository.ArticleRepository;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RestController;

import lombok.extern.slf4j.Slf4j;

@Slf4j
@RestController
public class ArticleApiController {

    @Autowired
    private ArticleRepository articleRepository;
    
    @PostMapping("/api/articles") // Post 요청이 "/api/articles"로 들어오는 경우 처리
    public Long create(@RequestBody ArticleForm form) { // Json 데이터를 받아옴
        log.info(form.toString() + "::form"); // 받아온 데이터 확인

        Article article = form.toEntity();
        log.info(article.toString() + "::article");

        Article saved = articleRepository.save(article);
        log.info(saved.toString() + "::saved");

        return saved.getId();
    }

    @GetMapping("/api/articles/{id}")
    public ArticleForm getArticle(@PathVariable Long id) {
        Article entity = articleRepository.findById(id) // id로 article을 가져옴!
                .orElseThrow( // 만약에 없다면,
                        () -> new IllegalArgumentException("해당 Article이 없습니다.") // 에러를 던짐!
                );
        // article을 form으로 변경! 궁극적으로는 JSON으로 변경 됨! 왜? RestController 때문!
        return new ArticleForm(entity);
        
    }
    
}
```

**dto/ArticleForm.java**

* id 필드 추가
* 생성자: entity 객체를 form으로 변환!

```java
package com.example.myapp.dto;

import com.example.myapp.entity.Article;

import lombok.AllArgsConstructor;

import lombok.Data;   // @Data 하나로 통합

// import lombok.AllArgsConstructor;
// import lombok.ToString;

@AllArgsConstructor
// @ToString

@Data  // 생성자(디폴트 All), getter, setter, toString 알아서 다 만들어줌
public class ArticleForm {
    private Long id;   //id 필드 추가
    private String title;
    private String content;
    
    // @AllArgsConstructor 으로 생성자 리펙토링
    
    // public ArticleForm(String title, String content) {
    //     this.title = title;
    //     this.content = content;
    // }

    // 생성자: entity 객체를 form으로 변환.
    public ArticleForm(Article entity) {
        this.id = entity.getId();
        this.title = entity.getTitle();
        this.content = entity.getContent();
    }    

    // @ToString 으로 리펙토링
    /*
    @Override
    public String toString() {
        return "ArticleForm [title=" + title + ", content=" + content + "]";
    }
    */

    public Article toEntity() {

        // Build 패턴으로 객체 생성! 입력순서 일치하지 않아도 됨
        // return new Article(null, title, content);
        return Article.builder() //@Builder 어노테이션 적용
                .id(null)
                .title(title)
                .content(content)
                .build();
    }

    
}
```


## 14.수정 폼 만들기
> Mission : 상세 페이지에서 수정 버튼을 클릭하여 수정 페이지가 나오게 하고,수정한 내용을 서버로 전달하시오. Ajax 사용 할 것.

[클라우드스터디 강좌](https://cloudstudying.kr/lectures/444)




## 15.무언가에 고수가 되는 법 (세 가지 일의 감각)
> Mission : 

