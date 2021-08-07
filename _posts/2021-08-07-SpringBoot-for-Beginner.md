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

> 이 글은 유튜브 홍팍님의 강의를 정리한 글입니다.  
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

### ③ "Hello World" 출력하기
src/main/resources/static/index.html 파일 생성  
브라우저에서 localhost:8080/index.html 열어서 확인  
페이지 없다고 나오면 서버 재시작  
서버 실행: Spring Boot Dashboard에서 app 실행

## 03.웹서비스 동작원리
> Mission : "Hello World"가 출력되는 과정을 설명하시오

* 웹페이지는 Client가 Request하고 Server가 이에 응답(Response)하는 구조로 실행됨
* 서버 실행은 : Spring Boot Dashboard에서 실행
* 서버 실행 : <ArtifactID>Application.java 파일을 실행
* 서버 실행로그에서 서버가 실행된 포트(8080) 확인이 가능
* localhost 서버의 8080 포트에서 실행 중
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
    <h1>{{username}}님, 반갑습니다!</h1>
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
> Mission : 

## 06.뷰 템플릿과 레이아웃
> Mission : 

## 07.폼 데이터 주고 받기
> Mission : 

## 08.데이터 생성 with JPA
> Mission : 

## 09.DB 테이블과 SQL
> Mission : 

## 10.Lombok과 리팩터링
> Mission : 

## 11.데이터 조회하기 with JPA
> Mission : 

## 12.데이터 목록조회
> Mission : 

## 13.링크와 리다이렉트
> Mission : 

## 14.수정 폼 만들기
> Mission : 

## 15.무언가에 고수가 되는 법 (세 가지 일의 감각)
> Mission : 

