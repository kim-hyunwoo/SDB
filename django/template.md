# Teplate

## 과정

1. 클라이언트의 요청
2. URL Dispatcher : 요청된 URL을 해당 view에 매칭시킴
3. View : 필요한 데이터를 모으고 template를 렌더링
4. Template : 렌더링 할 html을 정의

## setting.py

setting.py에 INSATALLED_APPS에 main_app을 정의함.

```python
INSTALLED_APPS = [
    ‘main_app’,

    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

main_app > Template 폴더에 index.html 파일 생성

```html
<!DOCTYPE html>
<html>
<head>
     <title>TreasureGram</title>
</head>
<body>
     <h1>TreasureGram</h1>
</body>
</html>
```

views.py에서 HttpResponse 대신 아래와 같이 수정

```python
from django.shortcuts import render

# Create your views here.
def index(request):
     return render(request, 'index.html')
```

## 데이터 표현

views.py

```python
def index(request):
     name = 'Gold Nugget'
     value = 1000.00
     context = {'treasure_name' : name, 'treasure_value' : value }
     return render(request, 'index.html', context)
```

index.html

```html
<!DOCTYPE html>
<html>
<head>
     <title>TreasureGram</title>
</head>
<body>
     <h1>TreasureGram</h1>
     <p>{{ treasure_name }}</p>
     <p>{{ treasure_value }}</p>
</body>
</html>
```

두 개의 {{ }} 로 감싸면 넘겨받은 context의 value를 key값으로 표현 가능

## 더 복잡한 데이터를 넘기기 위한 방법 : 클래스 정의

1. 클래스를 정의
2. 리스트를 이용해서 해당 클래스의 객체를 모두 저장
3. 해당 리스트를 view에서 template로 넘김

views.py

```python
class Treasure:
    def __init__(self, name, value, material, location):
        self.name = name
        self.value = value
        self.material = material
        self.location = location
# 생성자는 Java와 비슷한데, self가 들어감(this)

treasures = [
    Treasure('Gold Nugget', 500.00, 'gold', "Curly's Creek, NM"),
    Treasure("Fool's Gold", 0, 'pyrite', "Fool's Falls, CO"),
    Treasure('Coffee Can', 20.00, 'tin', "Acme, CA")
]

# Create your views here.
def index(request):
     return render(request, 'index.html', {'treasures' : treasures})
```

index.html

```html
<!DOCTYPE html>
<html>
<head>
     <title>TreasureGram</title>
</head>
<body>
     <h1>TreasureGram</h1>
     {% for treasure in treasures %}
          <p>{{ treasure.name }}</p>
          {% if treasure.value > 0 %}
               <p>{{ treasure.value }}</p>
          {% else %}
               <p>Unknown</p>
          {% endif %}
     {% endfor %}
</body>
</html>
```

img태그를 사용하는 법 참고

```html
<!DOCTYPE html>
<html>
<head>
     <title>TreasureGram</title>
</head>
<body>
     <h1>TreasureGram</h1>
     {% for treasure in treasures %}
          <p>{{ treasure.name }}</p>
          {% if treasure.value > 0 %}
               <p>{{ treasure.value }}</p>
          {% else %}
               <p>Unknown</p>
          {% endif %}
     {% endfor %}
</body>
</html>
```


## static 파일들 추가하기

static 폴더를 main_app안에 생성한다.
그리고 그 안에 style.css파일을 만든다.

index.html 파일에서 load 태그를 이용해서 스태틱 파일을 불러오고.

link 태그를 이용해 styel.css를 링크한다.

```html
{% load staticfiles %}

<!DOCTYPE html>
<html>
<head>
     <title>TreasureGram</title>
     <link rel="stylesheet" type="text/css" href="{% static 'style.css' %}">
</head>
```

bootstrap도 위와 같은 방식으로 적용하면 된다.

```html
{% load staticfiles %}

<!DOCTYPE html>
<html>
<head>
     <title>TreasureGram</title>
     <link rel="stylesheet" type="text/css” 
      href="{% static 'bootstrap.min.css' %}">
     <link rel="stylesheet" type="text/css" href="{% static 'style.css' %}">
</head>
```

## 이미지 저장

이미지를 저장하기 위해 static 폴더 아래에 images 폴더 생성

logo 파일을 상단 nav에 출력하는 예제

```html
<body>
     <nav class="navbar navbar-default navbar-static-top text-center">
          <a href="/">
               <img src="{% static 'images/treasure_logo.png' %}">
          </a>
     </nav>
...
</body>
```

좀 더 개선

```html
{% load staticfiles %}

<!DOCTYPE html>
<html>
<head>
     <title>TreasureGram</title>
     <link rel="stylesheet" href="{% static 'bootstrap.min.css' %}">
     <link rel="stylesheet" href="{% static 'style.css' %}">
</head>
<body>
     <nav class="navbar navbar-default navbar-static-top text-center">
          <a href="/">
               <img src="{% static 'images/treasure_logo.png' %}">
          </a>
     </nav>
     <main class="container" role='main'>
          <div class="treasure panel panel-default">
               {% for treasure in treasures %}
               <div class="panel-heading">
                    <h2 class="panel-title">{{ treasure.name }}</h2>
               </div>
               <div class="panel-body">
                    <table class="treasure-table">
                         <tr>
                              <th>
                                   <img src="{% static 'images/materials-icon.png' %}">
                              </th>
                              <td>{{ treasure.material }}</td>
                         </tr>
                         <tr>
                              <th>
                                   <img src="{% static 'images/value-icon.png' %}">
                              </th>
                              <td>
                              {% if treasure.value > 0 %}
                                   {{ treasure.value }}
                              {% else %}
                                   {{ "Unknown" }}
                              {% endif %}
                              </td>
                         </tr>
                         <tr>
                              <th>
                                   <img src="{% static 'images/location-icon.png' %}">
                              </th>
                              <td>
                                   {{ treasure.location }}
                              </td>
                         </tr>
                    </table>
               </div>
               {% endfor %}
          </div>
     </main>
</body>
</html>
```

## Treasure 클래스에서 이미지 URL 가져오기(해당 아이템의)

static 폴더가 아닌 외부 URL에서 가져오기 때문에 static 폴더에 위치하지 않음

view.py

```python
from django.shortcuts import render

class Treasure:
     def __init__(self, name, value, material, location, img_url):
          self.name = name
          self.value = value
          self.material = material
          self.location = location
          self.img_url = img_url

treasures = [
     Treasure('Gold Nugget', 500.00, 'gold', "Curly's Creek, NM", 'http://bansalnews.com/Photos/26-11-2016_11_49_17gold-bricks.png'),
     Treasure("Fool's Gold", 0, 'pyrite', "Fool's Falls, CO", 'http://bansalnews.com/Photos/26-11-2016_11_49_17gold-bricks.png'),
     Treasure('Coffee Can', 20.00, 'tin', "Acme, CA", 'http://bansalnews.com/Photos/26-11-2016_11_49_17gold-bricks.png')
]

# Create your views here.
def index(request):
     return render(request, 'index.html', {'treasures' : treasures})
```

index.html

```html
{% load staticfiles %}

<!DOCTYPE html>
<html>
<head>
     <title>TreasureGram</title>
     <link rel="stylesheet" href="{% static 'bootstrap.min.css' %}">
     <link rel="stylesheet" href="{% static 'style.css' %}">
</head>
<body>
     <nav class="navbar navbar-default navbar-static-top text-center">
          <a href="/">
               <img src="{% static 'images/treasure_logo.png' %}">
          </a>
     </nav>
     <main class="container" role='main'>
          <div class="treasure panel panel-default">
               {% for treasure in treasures %}
               <div class="panel-heading">
                    <h2 class="panel-title">{{ treasure.name }}</h2>
               </div>
               <div class="panel-body">
               <div class="treasure-photo">
                    <img src="{{ treasure.img_url }}" height="200">
               </div>
                    <table class="treasure-table">
                         <tr>
                              <th>
                                   <img src="{% static 'images/materials-icon.png' %}">
                              </th>
                              <td>{{ treasure.material }}</td>
                         </tr>
                         <tr>
                              <th>
                                   <img src="{% static 'images/value-icon.png' %}">
                              </th>
                              <td>
                              {% if treasure.value > 0 %}
                                   {{ treasure.value }}
                              {% else %}
                                   {{ "Unknown" }}
                              {% endif %}
                              </td>
                         </tr>
                         <tr>
                              <th>
                                   <img src="{% static 'images/location-icon.png' %}">
                              </th>
                              <td>
                                   {{ treasure.location }}
                              </td>
                         </tr>
                    </table>
               </div>
               {% endfor %}
          </div>
     </main>
</body>
</html>
```
































