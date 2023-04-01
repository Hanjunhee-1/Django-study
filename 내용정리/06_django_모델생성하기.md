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
    - 위의 명령어를 입력하고 나면 생성한 `model` 과 같은 변경 사항이 데이터베이스에 반영이 될 것입니다. 