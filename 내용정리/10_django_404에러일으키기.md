# 🔔 detail 에서 404 에러 일으키기

- 질문을 상세보기 할때의 경우를 생각해보면 사용자가 존재하지 않는 질문 `id` 를 통해 질문 상세보기를 할수도 있습니다. 때문에 이에 대한 예외처리가 필요합니다. 그래서 이번에는 `detail()` 의 내용을 작성해보도록 하겠습니다. 

- 우선 `polls/templates/polls/detail.html` 을 추가로 생성해줍니다. 
    ```html
        {{question}}
    ```
    - 매우 간단하게 작성해주었습니다. <br/><br/>

- 그리고 polls/views.py 의 detail() 의 내용을 수정해주었습니다.
    ```python
        from django.shortcuts import render
        from django.http import HttpResponse, Http404 # Http404 를 추가해주어야 합니다.
        from django.template import loader
        from .models import Question, Choice

        # ...
        def detail(request, question_id):
            try:
                question = Question.objects.get(pk=question_id)
            except Question.DoesNotExist:
                raise Http404("Question does not exist")
            return render(request, 'polls/detail.html', {
                'question': question
            })
    ```
    - 위 코드를 간단히 해석해보자면 `question` 이라는 변수에 `question_id` 를 기준으로 찾아낸 질문을 저장합니다. 만약 존재하지 않는 질문이라면 `404 에러` 를 일으키고 존재하는 질문이라면 `render` 를 해줍니다. 
    - `render()` 의 3번째 매개변수로 들어가는 저 형태가 익숙하지 않다면 임의의 변수에 `question` 에 대한 내용을 담아서 해당 변수를 활용하는 형태로 사용할 수 있습니다. (해당 부분은 이전 내용을 참고해주세요) <br/><br/><br/>

- 위의 방식이 너무 길다면 아래와 같이 짧게 코드를 줄여줄 수도 있습니다. 
    ```python
        from django.shortcuts import render, get_object_or_404  # get_object_or_404 또는 get_list_or_404 를 사용합니다.
        from django.http import HttpResponse, Http404   # 이 방법에서는 Http404 가 필요없습니다. 
        from django.template import loader
        from .models import Question, Choice

        def detail(request, question_id):
            question = get_object_or_404(Question, pk=question_id)
            return render(request, 'polls/detail.html', {
                'question': question
            })
    ```
    - `get_object_or_404` 에서 존재하는 `object` 이면 받고 존재하지 않는 `object` 이면 `404 에러` 를 일으킵니다. 

- 이제 django 프로젝트를 실행시켜서 `404 에러` 가 제대로 작동하는지 확인해봅니다.