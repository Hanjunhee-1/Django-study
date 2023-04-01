# Model 정의하기
- 데이터베이스를 연결했다면 모델을 생성해줄 차례입니다.

- `polls/models.py` 에 아래와 같이 작성해주었습니다.
    ```python
        from django.db import models

        # Create your models here.
        class Question(models.Model):
            question_text = models.CharField(max_length=200)
            publish_date = models.DateTimeField('date published')


        class Choice(models.Model):
            question = models.ForeignKey(Question, on_delete=models.CASCADE)
            choice_text = models.CharField(max_length=200)
            votes = models.IntegerField(default=0)
    ```
    - `Question` 과 `Choice` 라는 모델이 존재합니다. 
    - `Question` 에는 200자로 제한되어 있는 `question_text` 라는 필드와 생성된 날짜를 기록하는 `publish_date` 라는 필드가 존재합니다.   
    - `Choice` 에는 200자로 제한되어 있는 `choice_text` 라는 필드와 투표수를 기록할 수 있는 `votes` 라는 필드를 0 으로 초기화하여 갖고 있습니다. 뿐만 아니라 `Question` 모델의 기본키를 참조하고 있습니다. <br/><br/><br/><br/>


# Model 생성하기
- 지금까지의 과정을 따라왔다면 `polls` 의 모델만 정의되어 있고 생성은 하지 않고 있을 것입니다. 

- 우리의 눈에는 django 프로젝트에 `polls` 앱이 있다는 것을 알고 있지만 명시를 해주지 않았기 때문에 django 프로젝트에서는 `polls` 앱이 있는지 모릅니다. 때문에 아래와 같이 `django_tutorial/settings.py` 를 수정해주어야 합니다. 
    ```python
        INSTALLED_APPS = [
            'polls.apps.PollsConfig', # 해당 부분만 추가해주면 됩니다.
            "django.contrib.admin",
            "django.contrib.auth",
            "django.contrib.contenttypes",
            "django.contrib.sessions",
            "django.contrib.messages",
            "django.contrib.staticfiles",
        ]
    ```
    - `polls.apps` 에 `PollsConfig` 라는 메소드가 있다는 것을 유추해볼 수 있습니다. 내용을 확인해보면 `polls` 의 `model` 에 대한 것과 `polls` 앱의 이름이 `polls` 라는 것을 알려주고 있습니다. <br/><br/>

- 이제 아래의 명령어를 통해 `migration` 을 진행해주면 됩니다. 
    ```bash
        python manage.py makemigrations
    ```
    ```bash
        python manage.py sqlmigrate polls 0001
    ```
    - 위의 명령어는 실제로 데이터베이스에 `migration` 을 해주는 것이 아닌 확인용이며 `polls/migrations/0001_initial.py` 파일에 터미널로 출력된 것에 대한 내용이 존재하는 것을 확인할 수 있을 것입니다. <br/><br/>

    ```bash
        python manage.py migrate
    ```
    - 위의 명령어를 입력하고 나면 생성한 `model` 과 같은 변경 사항이 데이터베이스에 반영이 될 것입니다. <br/><br/><br/>

# API 갖고놀기
- django 에서 제공해주는 API 를 통해 데이터베이스에 값을 생성하고 읽어오는 것을 해볼 것입니다.

- 우선 아래의 명령어를 입력해서 API 를 사용할 수 있는 환경을 만듭니다.
    ```bash
        python manage.py shell
    ```
    <br/>

- 그러고 난 뒤에 모듈을 `import` 해주어야 합니다.
    ```python
        from polls.models import Choice, Question
    ```

- 테이블에 존재하는 모든 레코드 가져오기
    ```python
        [테이블 이름].objects.all()
    ```

- 테이블에 새로운 레코드 생성하기
    ```python
        from django.utils import timezone as tz

        q = Question(question_text="What\'s new?", publish_date=tz.now())
    ```

- 생성한 레코드를 테이블에 저장하기
    ```python
        q.save()
    ```
    <br/><br/>


- 생성된 레코드에 대해 값을 보기 전에 polls/models.py 에서 아래와 같이 코드를 추가해주는 것이 보기 편합니다.
    ```python
        from django.db import models

        # Create your models here.
        class Question(models.Model):
            question_text = models.CharField(max_length=200)
            publish_date = models.DateTimeField('date published')

            def __str__(self):
                return self.question_text


        class Choice(models.Model):
            question = models.ForeignKey(Question, on_delete=models.CASCADE)
            choice_text = models.CharField(max_length=200)
            votes = models.IntegerField(default=0)

            def __str__(self):
                return self.choice_text
    ```
    - `__str__(self)` 이라는 메소드를 각 모델에 추가해주었습니다. 이렇게 하면 나중에 테이블의 레코드에 접근할 때 object 형식이 아닌 string 형식으로 값을 볼 수 있습니다. 
    - 혹시라도 해당 부분이 이해가 되지 않으신다면 `Python-study` 를 보고 오시는 것을 추천합니다. <br/><br/><br/>

- 생성된 레코드 보기
    ```python
        q
    ```
    - `__str__(self)` 메소드를 따로 만들지 않았을 때는 id 값만 object 형태로 나오기 때문에 불편했는데 해당 메소드 덕분에 이렇게 하면 생성한 q 에 대한 question_text 를 볼 수 있습니다. <br/><br/>

- 특정 id 에 대한 레코드 보기
    ```python
        Question.ojbects.get(id=1)
    ```

- 자식관계 레코드 생성하기
    ```python 
        q.choice_set.create(choice_text='test', votes=0)
    ```
    - 부모 테이블을 통해 자식 테이블에 접근할 때는 `[자식테이블 이름]_set` 이런 식으로 되는 것 같습니다. <br/><br/>

- 외래키를 통해 조회하기
    ```python
        # Question 에 대한 새로운 레코드를 생성하고 저장합니다.
        q = Question(question_text='코딩이 좋다? 싫다?', publish_date=tz.now())
        q.save()

        # Choice 에 대한 새로운 레코드 2개를 생성하고 저장합니다.
        c1 = Choice(choice_text='좋다', votes=0, question=q)
        c2 = Choice(choice_text='싫다', votes=0, question=q)
        c1.save()
        c2.save()

        # Question 이 q 와 같은 Choice 만을 조회합니다. 
        Choice.objects.filter(question=q)

        # r1 과 r2 라는 변수에 결과를 저장합니다. 
        r1, r2 = Choice.objects.filter(question=q)

        # 각각의 투표수를 확인합니다. 
        r1.votes
        r2.votes
    ```

- 위의 API 말고도 더 많은 API 가 존재합니다. 공식문서를 참고하여 알아보는 것을 추천합니다. 