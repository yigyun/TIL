# Django Project Overview

## 프로젝트 배경
Django를 사용하게 된 배경은 팀원 간 논의 후 결정되었습니다. 팀원 중 Python을 주로 사용하는 인원이 많았고, Django는 **풀 스택 프레임워크**로서 Python 기반의 개발에 적합했기 때문입니다. 이를 통해 Python에 대한 기본적인 학습뿐만 아니라 Django 활용법도 함께 익히게 되었습니다.

## Django의 아키텍처 패턴
Django는 **MVT (Model-View-Template)** 패턴을 사용합니다. 이는 다음과 같은 구조로 동작합니다:

- **Model**: 데이터베이스 구조를 정의하는 곳으로, Python의 객체로 다룰 수 있습니다.
- **View**: HTTP 요청을 수신하고 응답을 반환하는 함수입니다. Model을 통해 데이터를 가져오거나 처리한 후, Template에게 응답 서식을 지정하도록 요청합니다.
- **Template**: HTML 파일의 구조와 레이아웃을 정의하여, 동적으로 콘텐츠를 생성합니다.

이 패턴은 Spring의 MVC 패턴과 유사한 점이 많습니다. **MVC의 View는 MVT의 Template에 해당**하고, **MVC의 Controller는 MVT의 View에 해당**한다고 보면 됩니다.

## Django 기본 기능
- **Admin 기능**: Django는 기본적으로 관리자 페이지를 제공하여 데이터와 모델을 손쉽게 관리할 수 있습니다.
- **SQLite 내장**: SQLite 데이터베이스가 기본으로 설정되어 있어 가볍고 빠르게 프로젝트를 시작할 수 있습니다.
- **ORM 지원**: Django는 Object-Relational Mapping(ORM)을 지원하여, Python 코드로 데이터베이스 연동을 쉽게 구현할 수 있습니다. 이를 통해 `Users.objects.all()`과 같은 직관적인 쿼리 작성이 가능합니다.

## 가상환경(Virtual Environment)
Django 프로젝트에서는 각 프로젝트가 **독립적인 환경**을 갖도록 가상환경을 사용합니다. 가상환경을 통해 **라이브러리 버전을 관리**하고, 프로젝트 간 충돌을 방지할 수 있습니다.

## 주요 파일 구조와 역할

- **manage.py**: Django의 여러 명령어 실행 진입점으로, 프로젝트 생성 시 기본적으로 제공되는 스크립트 파일입니다.
- **settings.py**: 프로젝트의 설정 파일로, 데이터베이스 연결 정보, 서버 포트, 미들웨어, 파일 연결 정보 등을 관리합니다. Spring의 `application.yml` 또는 `application.properties`와 유사한 역할을 합니다.
- **urls.py**: URL 라우팅을 담당하는 파일로, 특정 URL에 요청이 들어올 때 연결될 뷰(View)를 지정합니다. 앱 별로 세분화하여 관리할 수 있습니다.
- **views.py**: 요청을 처리하고 응답을 반환하는 로직을 작성하는 파일입니다.
- **models.py**: 데이터베이스와 연동되는 모델을 정의하는 파일로, ORM을 통해 데이터베이스와 쉽게 연결할 수 있습니다.

## Django 기본 사용법

### 프로젝트 생성
```bash
django-admin startproject [프로젝트 이름]
```

### 앱 생성
```bash
python manage.py startapp [앱 이름]
```

### 서버 실행
```bash
python manage.py runserver [포트번호]
```
Note: 이 명령어로 실행되는 내장 웹 서버는 개발 환경에서만 사용하도록 권장됩니다. 멀티 스레드나 성능 최적화가 필요한 프로덕션 환경에서는 별도의 서버 설정이 필요합니다.

### 데이터베이스 마이그레이션
* makemigrations: 모델 변경 사항을 바탕으로 마이그레이션 파일을 생성합니다.

```bash
python manage.py makemigrations
```
* migrate: 생성된 마이그레이션 파일을 기반으로 데이터베이스에 변경 사항을 적용합니다.

```bash
python manage.py migrate
```

### 관리자 계정 생성
```bash
python manage.py createsuperuser
```
### 추가
Django는 User 모델을 통해 기본적인 인증 시스템을 쉽게 구현할 수 있습니다. 이를 활용하면 사용자 등록, 로그인/로그아웃 등의 인증 기능을 빠르게 구축할 수 있습니다.

