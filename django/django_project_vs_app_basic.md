# Django Projects vs. Apps

Project : Pinterest

App : pinterest.com, blog.pinterest.com, careers.pinterest.com

하나의 프로젝트 안에서 개별 앱 형식으로 동작한다.

우선 로컬 서버를 한 번 돌려보고 django가 제대로 동작하는지 확인해보자.

migrate
```
$ python manage.py migrate
```

서버 실행, 8000번 포트
```
$ python manage.py runserver
```

앱 실행을 위한 디렉토리 생성
```
$ python manage.py startapp main_app
```

## VIEW

View는 요청을 받고 응답

view
```python
from django.shortcuts import render
from django.http import HttpResponse

# Create your views here.
def index(request):
    return HttpResponse('<h1>Hello World!</h1>')
```


URLs Dispatcher가 있어야 연결 가능(urls.py)

```python
from django.conf.urls import url
from django.contrib import admin
from main_app import views

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    # localhost/index
    url(r'index/', views.index),
]
```

localhost:8000/index/로 접속하면 view.py를 확인 가능

## index/ 없애고, 그냥 localhost:8000으로 접속하기

urls.py

```python
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    # localhost/index
    url(r'^', views.index),
]
```


프로젝트 url dispatcher와 앱 url dispatcher가 따로 있는게 best practice이다.

프로젝트 아래에 있는 urls.py는 이렇게 수정

```python
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    url(r'^admin/', admin.site.urls),
    # localhost/index
    url(r'^', include('main_app.urls')),
]
```

앱 아래에 urls.py 작성
```python
from django.conf.urls import url
from . import views

urlpatterns = [
    url(r'^$', views.index),
]
```

파일구조
```
- NewDjangoProject(virtualenv)
    - Project110
        - main_app
            - urls.py
            - admin.py
            - view.py
        - Project110
            - urls.py
```















































