# 🔔 뷰가 실제로 무엇인가를 하도록 만들기

- 지금까지 작성한 뷰는 데이터베이스에 있는 데이터를 활용하지 않고 문자열 값을 출력해주는 역할을 하고 있었습니다. 그래서 `index()` view 에 대해서 실제로 저장되어있는 `Question` 데이터의 최근 5개를 보여주도록 로직을 변경해주겠습니다. 
    ```python
        # polls/views.py
        from django.shortcuts import render
        from django.http import HttpResponse

        def index(request):
            latest_question_list = Question.objects.order_by('-publish_date')[:5]
            output = '<br>'.join([q.question_text for q in latest_question_list])
            return HttpResponse(output)

        # ...
    ```
    - 위의 코드는 Question 테이블에 존재하는 데이터를 생성된 날짜를 내림차순 기준으로 5개를 보여주는 코드입니다.
    - `-` 를 `field` 이름 옆에 적어주면 내림차순 정렬이 됩니다. <br/><br/><br/><br/>


# 🔔 템플릿 사용하기

- 위의 코드처럼 간단하게 해줄수도 있지만 보통은 템플릿을 사용합니다.

- 템플릿을 사용하기 위해서는 자신이 작업하고 있는 앱 디렉토리 위치에 `templates` 라는 디렉토리를 만들어주어야 합니다. 
    ```
        polls/
            ...
            templates/  <-- 새로 생성한 templates 폴더입니다.
    ```

- 그리고 해당 `templates` 디렉토리에 앱 이름과 동일한 폴더를 만들고 `.html` 파일을 만들어주세요.
    ```
        polls/
            ...
            templates/
                polls/
                    index.html
    ```
    - django 에서 템플릿을 찾을 때 프로젝트의 `setting.py` 를 참고합니다. `INSTALLED_APPS` 에 명시된 디렉토리의 `templates` 라고 되어있는 하위 디렉토리를 탐색하기 때문에 위와 같은 구조를 띄게 됩니다. <br/><br/>


-  `polls/templates/polls/index.html` 에 다음과 같이 html 내용을 작성해줍니다.
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
    <br/><br/><br/><br/>


# 🔔 뷰에서 템플릿을 사용하기

- `views.py` 에서 만들어둔 템플릿을 사용해보도록 하겠습니다. 
    ```python
        from django.shortcuts import render
        from django.http import HttpResponse
        from django.template import loader
        from .models import Question, Choice

        def index(request):
            latest_question_list = Question.objects.order_by('-publish_date')[:5]
            template = loader.get_template('polls/index.html')
            context = {
                'latest_question_list': latest_question_list,
            }
            return HttpResponse(template.render(context, request))
    ```
    - `loader` 를 통해 템플릿을 불러오고 `context` 를 통해 템플릿에 넘겨줄 정보를 명시합니다. 그리고 `template.render()` 를 통해 사용자에게 정보를 보여줍니다. <br/><br/><br/><br/>

- 위와 같이 `HttpResponse` 를 사용하는 방법도 있지만 `render` 를 사용하는 방법도 있습니다.  
    ```python
        from django.shortcuts import render
        from django.http import HttpResponse
        from django.template import loader
        from .models import Question, Choice

        def index(request):
            latest_question_list = Question.objects.order_by('-publish_date')[:5]
            context = {
                'latest_question_list': latest_question_list,
            }
            return render(request, 'polls/index.html', context)
    ```
    - `render` 를 사용한다면 `HttpResponse` 와 `loader` 는 사용하지 않아도 되고 코드가 조금 더 간결해집니다. 