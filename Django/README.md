```
pip install --upgrade pip

python -m venv myvenv

source myvenv/bin/activate

pip install django

꼭 점을 넣어주세요! 현재 폴더에 프로젝트를 생성하겠다는 명령어 입니다.
django-admin startproject 프로젝트 이름 .

python manage.py migrate
# migrate 명령어는 하편에서 설명해 드리도록 하겠습니다. 쉽게 말해 DB에 값을 넣는 작업입니다.
```

# 장고 프로젝트 기본 세팅

이제 `settings.py`에 28번째 줄에 가셔서 `ALLOWED_HOSTS` 변수의 값을 `['*']`로 바꿔 주십시오. 모든 사용자의 접속을 허락하겠다는 것입니다. 그리고 106번째 줄의 `LANGUAGE_CODE`와 `TIME_ZONE`을 다음과 같이 수정해주세요. 언어와 지역을 한국 기준으로 바꿔줍니다.

```python
ALLOWED_HOSTS=['*']
```

```python
LANGUAGE_CODE = 'ko-kr'

TIME_ZONE = 'Asia/Seoul'
```

이제 다음 명령어로 서버를 실행시켜주세요.

```
python manage.py runserver 
```