# 🔔 새로운 뷰 추가하기

- `poll` 앱에서 다음과 같은 4개의 `view` 를 만들어볼 것입니다.
    ```
        1. 질문 색인 페이지: 최근의 질문들을 표시합니다.
        2. 질문 세부 페이지: 질문의 내용과 투표할 수 있는 서식을 표시합니다.
        3. 질문 결과 페이지: 특정 질문에 대한 결과를 표시합니다.
        4. 투표 기능: 특정 질문에 대해 특정 선택을 할 수 있도록 투표 기능을 제공합니다.
    ```
    <br/><br/>

- 사용자에게 보여줄 `view` 를 `views.py` 에 함수로 정의합니다. 
    ```python
        # polls/views.py
        from django.shortcuts import render
        from django.http import HttpResponse

        # Create your views here.
        def index(request):
            return HttpResponse("Hello, world. You're at the polls index.")

        # 질문 세부 페이지
        def detail(request, question_id):
            return HttpResponse('You\'re looking at question %s' % question_id)

        # 질문 결과 페이지
        def results(request, question_id):
            response = 'You\'re looking at the results of question %s'
            return HttpResponse(response % question_id)

        # 투표 기능
        def vote(request, question_id):
            return HttpResponse('You\'re voting on question %s' % question_id)
    ```
    - 질문 색인 페이지는 `index()` 에서 맡게 됩니다.
    <br/><br/><br/>

- `view` 에 대한 함수를 정의했으므로 `URL` 에 대한 정보를 `urls.py` 에 작성해줍니다.
    ```python
        # polls/urls.py
        from django.urls import path
        from . import views

        urlpatterns = [
            # ex: /polls
            path('', views.index, name='index'),

            # ex: /polls/5
            path('<int:question_id>/', views.detail, name='detail'),

            # ex: /polls/5/results
            path('<int:question_id>/results/', views.results, name='results'),

            # ex: /polls/5/vote
            path('<int:question_id>/vote/', views.vote, name='vote'),
        ]
    ```
    - `path parameter` 로 `question_id` 를 받게 됩니다. 해당 `question_id` 는 `views.py` 에 정의한 함수들의 매개변수로 사용됩니다.
    - 만약 숫자값이 아닌 문자열에 대한 `path parameter` 를 받게 된다면 다음과 같이 표시해줄 수 있습니다.
        ```python
            # ex: /test/:name/
            path('<str:name>/', views.test_name, name=test_name)

            # views.py 의 함수 부분
            def test_name(request, name):
                return HttpResponse('Hello %s' % name)
        ```
        <br/><br/><br/><br/>


# 🔔 새로운 뷰가 제대로 동작하는지 확인하기

- 새로운 뷰를 추가했다면 django 프로젝트를 실행시키고 views.py 에서 정의한대로 제대로 작동하는지 확인해봐야 합니다. 