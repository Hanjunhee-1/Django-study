# 🔔 템플릿에서 하드코딩된 URL 제거하기 

- polls/templates/polls/index.html 을 보면 링크 부분이 하드코딩되어있습니다.
    ```html
        {% if latest_question_list %}
            <ul>
            {% for question in latest_question_list %}
                <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
            {% endfor %}
            </ul>
        {% else %}
            <p>No polls are available.</p>
        {% endif %}
    ```
    - 위와 같은 코드의 단점은 나중에 라우팅 경로를 수정할 때 굉장히 힘들다는 것입니다. 때문에 이러한 불편한 점을 해결하기 위해 다음과 같이 수정해줍니다. <br/><br/>

    ```html
        {% if latest_question_list %}
            <ul>
            {% for question in latest_question_list %}
                <li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
            {% endfor %}
            </ul>
        {% else %}
            <p>No polls are available.</p>
        {% endif %}
    ```
    - {% url %} 이라는 태그를 사용하여 하드코딩된 부분을 수정해줄 수 있습니다. url 다음에 오는 'detail' 이라는 것은 polls/urls.py 에서 name 값으로 정해준 것을 사용한 것입니다. 해당 부분이 만약 다르다면 에러가 나니 주의하세요.

    - 이렇게 수정해준 덕분에 라우팅 경로를 수정해주는 것이 쉬워졌습니다.
        
        - 예를 들어, 현재는 polls/question_id 로 되어있는 라우팅 경로를 polls/specifics/question_id 로 바꿔주고 싶다면 polls/urls.py 에서 해당 라우팅 경로를 수정해주면 됩니다.
        ```python
            # 수정 예시
            path('specifics/<int:question_id>/', views.detail, name='detail')
        ```