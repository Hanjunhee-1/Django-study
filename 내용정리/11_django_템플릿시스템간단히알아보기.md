# 🔔 템플릿 시스템 알아보기

- 템플릿 시스템에 대해 알아보기 전에 `polls/templates/polls/detail.html` 을 아래와 같이 수정해주었습니다. 
    ```html
        <h1>{{ question.question_text }}</h1>
        <ul>
            {% for choice in question.choice_set.all %}
                <li>{{ choice.choice_text }}</li>
            {% endfor %}
        </ul>
    ```
    <br/><br/>

- `{{}}` 로 묶여있는 것은 변수를 출력해주기 위해 사용한 것입니다. `views.py` 의 `detail()` 에서 `context` 를 보내주는데 `question` 이라는 이름으로 보내주었기에 html 파일에서는 `question` 의 이름에 접근해야 합니다. 

- `{% %}` 는 `반복문` 이나 `if문` 같은 제어문을 사용하기 위해 사용합니다.

- 프로그래밍의 기초적인 문법과 django 의 API 에 대해서 잘 알고 있어야 django 템플릿을 잘 사용할 수 있습니다. 