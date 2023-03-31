# 자신이 만든 app 에서...
- django 프로젝트 내부에 app 을 새로 만들었다면 아래와 같은 구조일 것입니다.
    ```
        polls/
            migrations/
                __init__.py
            __init__.py
            admin.py
            models.py
            tests.py
            views.py
    ```
    <br/><br/>

- 여기서 `views.py` 의 내용을 아래와 같이 작성해줍니다.
    ```python
        # django_tutorial/polls/views.py
        from django.shortcuts import render
        from django.http import HttpResponse

        # Create your views here.
        def index(request):
            return HttpResponse("Hello, world. You're at the polls index.")
    ```
    ※ 따로 찾아보니 `render()` 를 활용하는 방식도 있는것 같은데 공식문서에 나와있는대로 `django.http` 모듈의 `HttpResponse` 를 사용했습니다.
    <br/><br/><br/>

- 다음으로는 app 내부에서 `urls.py` 라는 이름의 파일을 새로 만들어준 후 내용을 아래와 같이 작성해줍니다.
    ```python
        # django_tutorial/polls/urls.py
        from django.urls import path
        from . import views

        urlpatterns = [
            path('', views.index, name='index'),
        ]
    ```
    코드를 간단하게 보자면 동일한 위치에 있는 `views.py` 의 `index()` 함수를 사용할 수 있게 해주는 것 같습니다.
    <br/><br/><br/><br/><br/><br/>


# 다시 자신의 프로젝트 폴더에서...
- 자신이 만든 프로젝트 폴더 구조를 보면 프로젝트 이름과 동일한 폴더가 있습니다. 해당 폴더의 `urls.py` 의 내용을 아래와 같이 수정해줍니다.
    ```python
        # django_tutorial/urls.py
        from django.contrib import admin
        from django.urls import path, include

        urlpatterns = [
            path('polls/', include('polls.urls')),  # 실제로 추가해주는 부분은 해당 라인 뿐입니다.
            path("admin/", admin.site.urls),
        ]
    ```
    라우팅 경로를 설정해주는 곳이라고 생각하면 될 것 같습니다. 현재 코드를 기준으로 보면 라우팅 경로는 `localhost:8000/admin` 과 `localhost:8000/polls` 가 있습니다. `polls/` 에서는 `polls/urls.py` 에서 작성해준 `urlpatterns` 를 통해 라우팅을 해주고 있습니다. 
    <br/><br/><br/><br/><br/><br/>

# 이제 다시 실행해보기
```bash
    python manage.py runserver
```
- 이제 다시 위의 명령어로 django 프로젝트를 실행시킨 뒤 `localhost:8000/polls` 로 접속하면 `Hello, world. You're at the polls index.` 라는 문구가 나타날 것입니다. 
- 해당 과정을 통해 `request` 와 `response` 의 대략적인 흐름을 알아두길 바랍니다.