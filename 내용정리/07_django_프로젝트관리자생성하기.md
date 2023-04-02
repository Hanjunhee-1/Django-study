# 🔔 관리자 계정 생성하기
- django 프로젝트를 실행시키고 `localhost:8000/admin` 으로 접속하면 관리자로써 현재 프로젝트의 app 에 대한 모델 및 정보들을 한 눈에 볼 수 있습니다. 이를 위해서는 관리자 계정을 생성해주어야 합니다. 

- 아래의 단계를 따라 관리자 계정을 생성해줍니다.

    1. django 프로젝트의 폴더가 위치하는 곳에서 아래의 명령어를 입력해줍니다.
        ```bash
            python manage.py createsuperuser
        ```

    2. `Username`, `Email`, `Password` 등의 정보를 입력해줍니다.

    3. django 프로젝트를 실행시키고 `localhost:8000/admin` 으로 접속하여 생성해둔 관리자 계정으로 로그인합니다. <br/><br/><br/><br/>

# 🔔 관리자 계정으로 데이터 관리하기

- 관리자 계정을 통해 모델의 데이터를 관리할 수 있습니다. 하지만 그러기 위해서는 관리자 계정으로 조작할 수 있게 django 프로젝트에 존재하는 앱의 모델들을 관리자가 접근할 수 있게 해주어야 합니다. 

- 저는 `polls` 의 `Question` 과 `Choice` 를 관리자가 접근할 수 있게 `polls/admin.py` 를 아래와 같이 수정해주었습니다. 
    ```python
        from django.contrib import admin
        from .models import Question, Choice

        admin.site.register(Question)
        admin.site.register(Choice)
    ```

- 이제 관리자가 모델 및 데이터에 접근할 수 있게 되었습니다. 해당 작업 덕분에 데이터베이스에 따로 접근하지 않아도 django 프로젝트를 실행시킴으로써 간단하게 데이터에 대한 작업을 해줄 수 있게 되었습니다. CRUD 작업을 해보시길 바랍니다.