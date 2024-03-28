# Static Files
정적 파일  
서버 측에서 변경되지 않고 고정적으로 제공되는 파일(이미지, JS, CSS 파일 등)  

웹 서버와 정적 파일
- 웹 서버의 기본 동작은 특정 위치(URL)에 있는 자원을 요청(HTTP request) 받아서 응답(HTTP response)를 처리하고 제공하는 것
- 이는 "자원에 접근 가능한 주소가 있다" 라는 의미
- 웹 서버는 요청 받은 URL로 서버에 존재하는 정적 자원을 제공함
- 정적 팡리을 제공하기 위한 경로(URL)가 있어야 함

## Static files

### 1. 기본 경로에서 제공하기
app 폴더/static/
- app_name/static/app_name/ 경로에 이미지 파일 배치
- static tag를 사용해 이미지 파일에 대한 경로 제공
```html
{% load static %}

<img src="{% static 'app_name/sample.png' %}" alt="sample">
```
STATIC_URL
- 기본 경로 및 추가 경로에 위치한 정적 파일을 참조하기 위한 URL
- 실제 파일이나 디렉토리가 아니며, URL로만 존재
```python
# settings.py

STATIC_URL = 'static/'
```
=> URL + STATIC_URL + 정적파일 경로  
=> http://127.0.0.1:8000/static/app_name/sample.png
### 2. 추가 경로에서 제공하기
STATICFILES_DIRS에 문자열 값으로 추가 경로 설정  
STATICFILES_DIRS: 정적 파일의 기본 경로 외에 추가적인 경로 목록을 정의하는 리스트
```python
# setting.py

STATIC_URL = 'static/'
STATICFILES_DIRS = [
    BASE_DIR / 'static', 
]
```
```html
<img src="{% static "sample.png" %}" alt="sample">
```

## Media Files
사용자가 웹에서 업로드하는 정적 파일(user-uploaded)

### ImageField()
이미지 업로드에 사용하는 모델 필드  
이미지 객체가 직접 저장되는 것이 아닌 '이미지 파일의 경로'가 문자열로 DB에 저장  

미디어 파일 제공을 위한 사전 준비  
1. settings.py에 MEDIA_ROOT, MEDIA_URL 설정  
MEDIA_ROOT: 실제 미디어 파일들이 위치하는 디렉토리의 절대 경로
```python
# settings.py

MEDIA_ROOT = BASE_DIR / 'media'
```
MEDIA_URL: MEDIA_ROOT에서 제공되는 미디어 파일에 대한 주소를 생성(STATIC_URL과 동일한 역할)
```python
# settings.py

MEDIA_URL = 'media/'
```
2. 작성한 MEDIA_ROOT와 MEDIA_URL에 대한 url 지정  
settings.MEDIA_URL: 업로드 된 파일을 제공하는 URL  
settings.MEDIA_ROOT: 위 URL을 통해 참조하는 파일의 실제 위치
```python
# project/urls.py
from django.conf import settings
from django.conf.urls.static import static

urlpatterns = [
    path('admin/', admin.site.urls),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
# 따로 빼주는건 큰 의미는 없고 리스트 내부에 path만 보이게 하기 위함
```

### Image upload
1. models.py 수정
```python
image = models.ImageField(blank=True)  # null 허용
```
2. migration 진행
```bash
pip install pillow  # ImageField를 사용하려면 반드시 Pillow 라이브러리가 필요

python manage.py makemigrations
python manage.py migrate

pip freeze > requirements.txt
```
3. form 요소의 enctype 속성 추가
```html
<form action="{% url "articles:create" %}" method="POST" enctype="multipart/form-data">
```
4. view 함수에서 업로드 파일에 대한 추가 코드 작성
```python
# app_name/views.py

def create(request):
    if request.method == 'POST':
        form = ArticleForm(request.POST, request.FILES)
```
5. 이미지 업로드 입력 양식 확인
6. 이미지 업로드 결과 확인(DB)

### Upload Image 제공
- 'url'속성을 통해 업로드 파일의 경로 값을 얻을 수 있음
- app_name.image.url
  - 업로드 파일의 경로
- app_name.image
  - 업로드 파일의 파일 이름
```html
<img src="{{ app_name.image.url }}" alt="img">
```
위처럼 작성하면 이미지를 업로드하지 않은 게시물을 detail 템플릿을 렌더링 할 수 없음
- 이미지 데이터가 있는 경우만 이미지를 출력할 수 있도록 처리
```html
{% if app_name.image %}
    <img src="{{ app_name.image.url }}" alt="img">
{% endif %}
```

#### 업로드 이미지 수정
```html
<!-- app_name/update.html -->

<form action="{% url "articles:update" article.pk %}" method="POST" enctype='multipart/form-data'>
```

## 참고
'upload_to' argument
- ImageField()의 upload_to 인자를 사용해 미디어 파일 추가 경로 설정
```python
# 기본 경로 설정
image = models.ImageField(black=True, upload_to='images/')
# 업로드 날짜로 경로 설정
image = models.ImageField(black=True, upload_to='%Y/%m/%d/')
# 함수 형식으로 경로 설정
def articles_image_path(instance, filename):
    return f'images/{instance.user.username}/{filename}'

image = models.ImageField(black=True, upload_to=articles_image_path)
```