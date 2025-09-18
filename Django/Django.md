# Django

## Web Application

- 인터넷을 통해 사용자에게 제공되는 소프트웨어 프로그램
- 다양한 디바이스에서 웹 브라우저를 통해 접근 가능
- Web Service 이를 통해 제공되는 서비스

## Client

- 요청

## Server

- 응답

## Web Framework

- 웹 애플리케이션을 빠르게 개발할 수 있도록 도와주는 도구
- 일반적인 웹 서비스에 필요한 도구를 라이브러리 등을 통해 제공

## 가상 환경

- 컴퓨터 안에 독립된 환경을 생성 → 여러개의 독립적 환경 생성 가능
- 버전이 다른 환경에서 각각 작업이 필요한 경우
- 필요한 패키지가 충돌이 나타나 각각 설치해야 하는 경우
- 프로젝트마다 별도의 가상 환경을 사용 → 서로 다른 프로젝트 폴더에 venv가 각각 존재
    
    → 일반적으로 venv 폴더는 관련 프로젝트와 동일 경로에 위치
    
- venv 폴더는 .gitignore로 작성되어 원격 저장소에 공유하지 않음

### 가상환경 생성

```bash
$ python -m venv venv(폴더 이름)
```

- venv 폴더 안에는 파이썬 실행 파일, 라이브러리 등이 담김
- 직접 수정 하지 말 것

### 활성화

```bash
$ source venv/Scripts/activate
```

- on - off 개념
- 파일 경로를 이동해도 꺼지지 않는다.

### 종료

```bash
$ deactivate
```

- 터미널을 꺼도 꺼지기는 하지만 완전히 종료 안되는 경우가 있을 수도 있음

## 의존성 패키지

- 하나의 소프트웨서가 동작하려먼 여러가지의 개별적인 패키지와 라이브러리가 필요함

### 패키지 목록 확인

```bash
$ pip list
```

### 의존성 기록

- 다른 환경에서 동일한 환경 새로 설치가 필요하기 때문에 텍스트 파일 형태로 의존성을 기록해야 함
- 패키지의 버전 등이 다름 → 동작이 다를 수 있음
- 패키지 세부사항의 공유 및 관리가 필수적이다.

```bash
$ pip freeze > requirements.txt
```

- 관례적으로 requirements.txt 파일명을 사용

### 의존성 패키지 기반 설치

```bash
$ pip install -r requirements.txt
```

- requirements.txt를 기반으로 새로운 환경에서 의존성 패키지를 설치한다
- → venv 파일은 공유 용도로 사용하지 않음

## Django 프로젝트 생성

### Django 설치

```bash
$ pip install django
```

### Django 프로젝트 생성

```bash
$ django-admin startproject firstpjt .
```

- 현재 디렉토리에 firstpjt라는 이름의 django 프로젝트를 생성

### Django 서버 실행

- “[manage.py](http://manage.py)” → 장고 명령어를 제공

```bash
$ python [manage.py](http://manage.py) runserver
```

- 터미널에서 cntl + c → 서버 종료

## Design Pattern

- 소프트웨어 설계에서 반복적으로 발생하는 문제에 대한 일반적 해결책
- MVC → Model - View - Controller
- Django = MTV → Model - Template(View) - View(Controller)

## 프로젝트 vs 앱

- 프로젝트 → 설정, 기반 → 앱들의 집합
- 앱 → 기능 → 독립적인 기능 단위 모듈

### 앱 생성

- 앱의 이름은 복수형으로 지정하는 것을 권장

```bash
$ python [manage.py](http://manage.py) startapp articles
```

### 앱 등록

- 반드시 앱을 생성한 후 앱을 프로젝트에 등록하여 알려줘야 함
- 반드시 <생성 → 등록> 순서로 진행 <등록 → 생성> 은 불가능
- pjt/settings.py 안 INSTALLED_APPS 리스트 안에 앱을 등록
- 사용자가 생성한 app은 리스트 최상단에  문자열 형태로 등록할 것을 약속
    
    → Django의 내부 구동 순서 때문
    

## 프로젝트 구조

### 수정 필요O

### settings.py

### urls.py

### 수정 필요X

### **__**init**__.**py

- 해당 폴더 패키지로 인식

### asgi.py

- 

### wsgi.py

### manage.py

- 프로젝트 최상단 폴더에 위치

## 앱 구조

### admin.py

- 관리자용 페이지 설정

### models.py

- MTV 패턴 중 M

### views.py

- MTV 패턴 중 V
- data(Model)과 Template(view)를 컨트롤하는 역할?

Django는 기본적으로 백엔드에 특화됨 프론트 역할의 Template이 있긴하지만 기본적으로는 생성하지 않음

### apps.py

### tests.py

## 요청 응답 과정

- views.py의 view 함수가 메인 Controller 역할

사용자가 요청 → [urls.py](http://urls.py) → [views.py](http://views.py) ←→ models.py

                            응답 ← [views.py](http://views.py) ←→ templates

- 모든 뷰 함수는 urls.py로 부터 요청을 받는다.
    
    → 첫번째 인자로 무조건 요청을 받는 매개변수가 필요하다.
    
    - 매개변수 이름은 관례적으로 request로 작성

 path('artcles/', views.index)

index()로 작성하면 그냥 탐색 도중 함수가 호출되어 버린다

→ path 요청이 확인 되었을 때만 함수를 호출하기 위해 대기상태로 만드는 index로 함수 코드를 작성한다

## templates

- 파일명은 templates로 약속