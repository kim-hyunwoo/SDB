# Model

Model은 데이터 구조를 정의하고 데이터베이스와 통신한다.

## Treasure 클래스를 모델로 변환

models.py
```python
from django.db import models

# 모델을 상속하는 클래스 작성,
# 데이터베이스 타입과 연관된 타입으로 작성
class Treasure(models.Model):
    name = models.CharField()
    value = models.DecimalField()
    material = models.CharField()
    location = models.CharField()
    img_url = models.CharField()
```

Django vs SQL

1. CharField() == VARCHAR()
2. IntegerField() == INTEGER()
3. FloatField() == FLOAT()
4. DecimalField() == DECIMAL()

refer link : [Doc](https://docs.djangoproject.com/en/1.9/ref/models/fields/)

SQL을 직접사용하는 대신 Django ORM이 대신해준다. 때문에 migration 과정이 필요하다.

## migration

```
//마이그레이션 파일 만들기 
$ python manage.py makemigrations

// ORM이 생성하는 SQL을 preview로 확인하기 
$ python manage.py sqlmigrate main_app 001

// migrate 실행 
$ python manage.py migrate

// 최신 상태인지 확인(마이그레이션)
$ python manage.py makemigrations
$ python manage.py migrate

// 마이그레이션이 끝나면 해당 객체를 django shell에서 사용 가능
$ python manage.py shell

// django shell은 실제 모델을 작성하거나 쿼리를 작성하기 전에 실험해볼 수 있는 도구

>>> from main_app.models import Treasure
>>> Treasure.objects.all() // SELECT * FROM Treasure

// Treasure.objects는 SQL의 SELECT 문과 같은 역할이다.
// .all()은 * FROM과 같다.
// filter을 사용하면 WHERE문과 동일한 결과를 얻을 수 있다.
>>> Treasure.objects.filter(location = 'Orlando, FL')

//만약 단 1개의 매칭 결과가 있는 것을 알고 있다면 get()을 쓰면된다.
>>> Treasure.objects.get(pk = 1)

// Treasure 객체를 생성한 뒤 저장해준다.
>>> t = Treasure(name = 'gold nugget', value=20.00, material = 'gold', location = 'Seoul', img_url = ‘url')
>>> t.save()

// 다시 쿼리를 실행해본다.
>>> Treasure.objects.all()

// 객체의 정보가 출력되는 것을 알 수 있다. 하지만 충분히 자세하지 않으므로 Treasure 모델에 함수를 추가해준다.

def __str__(self):
    return self.name

>>> Treasure.objects.all()
// 다시 출력해보면 Treasure name이 출력됨을 알 수 있다.
// Treasure 객체를 여러 개 생성해서 저장해보자.
```

## view.py 수정

view.py

```python
from django.shortcuts import render
from .models import Treasure

# Create your views here.
def index(request):
     treasures = Treasure.objects.all()
     return render(request, 'index.html', {'treasures' : treasures})
```


## The Admin

장고의 강력한 기능 중 하나는 자동으로 생성되는 admin 인터페이스이다.

admin을 이용하기 위해서는 superuser를 생성해야 한다.

```
$ python manage.py createsuperuser
```

하지만 로그인해보면 Treasure 모델이 보이지 않는다.

admin.py에 등록을 해야 한다.

admin.py
```python
from django.contrib import admin
from .models import Treasure

# Register your models here.
admin.site.register(Treasure)
```

admin 인터페이스를 이용해서 모델에 데이터를 추가/수정/삭제할 수 있다.
















