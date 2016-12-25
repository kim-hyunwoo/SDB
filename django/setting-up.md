# Django Start-kit

## Django의 구조

MVC가 아닌 MTV 구조. Template(View), View(Controller), Model의 구조를 가진다.

1. 웹브라우저의 요청
2. View(Controller) : 데이터 처리
3. Model : 데이터 저장 및 처리
4. Template(View) : HTML 렌더링

## 세팅


### virtualenv

```
$ pip install virtualenv
```

가상환경 세팅 : 가상환경을 세팅할 디렉토리로 이동

```
$ cd directory_name
$ virtualenv venv(가상환경이름)
$ source venv/bin/activate
```

터미널 명령줄에 (venv)가 표시되면 성공적으로 설정된 것

가상환경에 django 설치

```
$ pip install django
다른 버전을 설치하고 싶다면
$ pip install django==1.7
$ pip freeze
설치된 목록을 확인
```


### 프로젝트 생성

```
$ django-admin startproject Project_Name
$ deactivate
가상 환경에서 빠져나옴.
```


### refer
[virtualenv doc](https://virtualenv.pypa.io/en/stable/)




















