# 🔔 URL 이름 구분해주기

- 현재는 django 프로젝트에 앱이 딱 한개만 있어서 `{% url %}` 템플릿 태그를 사용할 때 겹칠일이 없습니다. 하지만 앱이 여러개이고 뷰가 겹치게 된다면 django 는 어떤 앱의 뷰에서 url 을 생성해야 하는지 알수 없습니다. 때문에 url 의 이름을 구분해주어야 합니다.

- 각 앱의 urls.py 에서 `app_name` 을 추가해주면 됩니다!
    ```python
        # polls/urls.py
        from django.urls import path
        from . import views

        app_name = 'polls'
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
    - 이렇게 해주면 앱의 이름을 통해서도 구분을 해줄 수 있기 때문에 뷰의 이름이 겹친다고 해도 문제가 없습니다. <br/><br/>

- 그리고 `{% url %}` 템플릿태그를 사용하는 곳에서 앱의 이름까지 포함하여 `url` 을 생성할 수 있도록 수정해줍니다.
    ```html
        <!-- polls/templates/polls/index.html -->
        {% if latest_question_list %}
            <ul>
            {% for question in latest_question_list %}
                <li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
            {% endfor %}
            </ul>
        {% else %}
            <p>No polls are available.</p>
        {% endif %}
    ```
    - `app_name:view_name` 형태로 수정해주면 됩니다.