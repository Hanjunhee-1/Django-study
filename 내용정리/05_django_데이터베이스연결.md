# MySQL 을 기준으로...
- `django` 프로젝트를 생성하면 기본으로 사용하는 `database` 는 `sqlite3` 입니다. 하지만 저는 `MySQL` 을 사용할 것이기 때문에 `database` 에 대한 설정값을 바꿔주어야 합니다.<br/><br/>
- 우선은 아래의 명령어를 통해 `MySQL 드라이버` 를 설치해줍니다.
    ```bash
        pip install mysqlclient
    ```
    <br/>
- 설치가 제대로 된 것을 확인했다면 `django_tutorial/settings.py` 를 열어서 `DATABASES` 의 부분을 아래와 같이 고쳐줍니다.
    ```python
        DATABASES = {
            "default": {
                "ENGINE": "django.db.backends.mysql",
                "NAME": 'database_name',
                "USER": 'username',
                "PASSWORD": 'password',
                "HOST": 'host_address',
                "PORT": 'port_number',
            }
        }
    ```
    - `ENGINE`: 어떤 `database` 를 사용할 것인지를 나타냅니다.
    - `NAME`: 사용할 `database` 의 이름을 나타냅니다. `mysql` 서버에 `django` 프로젝트에서 사용하고자 하는 `database` 가 이미 생성되어 있어야 합니다.
    - `USER`: `database` 의 사용자 이름입니다.
    - `PASSWORD`: 사용자에 대한 비밀번호입니다.
    - `HOST`: `host` 주소입니다. 보통 `local` 에서 작동시키므로 `localhost` 를 입력합니다.
    - `PORT`: 해당 `database` 의 서버가 동작하고 있는 `port` 번호입니다. `mysql` 의 경우 일반적으로 `3306` 을 사용합니다. 
    <br/><br/><br/>
- 올바르게 `settings.py` 를 수정해주었다면 이제 `migration` 을 진행하면 됩니다. 아래의 순서에 따라 `migration` 을 진행해주세요<br/><br/>
    - 이미 연결했던 `database` 에 대해서 변경사항이 생겼고 이를 `migration` 하려는 것인가요?
        - Yes
            ```bash
                python manage.py makemigrations
            ```
            ```bash
                python manage.py migrate
            ```
        - No
            ```bash
                python manage.py migrate
            ```
            <br/><br/><br/>

# 잘 연결되었는지 확인하기
- 잘 연결이 되었다면 터미널에 명령어를 입력했을 때 별다른 에러 없이 `OK` 가 마구 뜨면서 연결되었을 것입니다.
- `MySQL Command Line Client` 에서 `django` 프로젝트와 연결한 `database` 의 `table` 들을 봅니다. (여러 개의 `table` 이 생겼을 것입니다.)