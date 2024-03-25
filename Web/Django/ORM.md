# ORM
Object Relational Mapping  
객체 지향 프로그래밍 언어를 사용하여 호환되지 않는 유형의 시스템 간에 데이터를 변환하는 기술

## QuerySet API
ORM에서 데이터를 검색, 필터링, 정렬 및 그룹화 하는 데 사용하는 도구
- API를 사용하여 SQL이 아닌 Python 코드로 데이터를 처리
- python의 모델 클래스와 인스턴스를 활용해 DB에 데이터를 저장, 조회, 수정, 삭제(CRUD)하는 것
![QuerySet API](./img/queryset_api.PNG)

QuerySet API 구문
```python
Article.objects.all()
# Model class.Manager.QuerySet_API
```

### Query
- 데이터베이스에 특정한 데이터를 보여 달라는 요청
- 쿼리문 작성
  - 원하는 데이터를 얻기 위해 데이터베이스에 요청을 보낼 코드를 작성
- 파이썬으로 작성한 코드가 ORM에 의해 SQL로 변환되어 데이터베이스에 전달되며, 데이터베이스의 응답 데이터를 ORM이 QuerySet이라는 자료 형태로 변환하여 전달

### QuerySet
- 데이터베이스에게서 전달받은 객체 목록(데이터 모음)
  - 순회가 가능한 데이터로써 1개 이상의 데이터를 불러와 사용할 수 있음
- Django ORM을 통해 만들어진 자료형
- 단, 데이터베이스가 단일한 객체를 반환할 때는 QuerySet이 아닌 class의 instance로 반환

## QuerySet API 실습

### QuerySet API 실습 사전 준비
```bash
pip install ipython
pip install django-extensions
```
```python
# settings.py

INSTALLED_APPS = [
    'django_extensions'
]
```

### Django shell
Django 환경 안에서 실행되는 python shell  
입력하는 QuerySet API 구문이 Django 프로젝트에 영향을 미침  

Django shell 실행
```bash
python manage.py shell_plus
```

### Create
데이터 객체를 생성하는 3가지 방법  
1. 특정 테이블에 새로운 행을 추가하여 데이터 추가
```python
article = Article()  # Article class로부터 article instance 생성
article.title = 'first'  # 인스턴스 변수 title에 값 할당
article.content = 'django'  # 인스턴스 변수 content에 값 할당

# save를 하지 않으면 DB에 값이 저장되지 않음
article.save()
Article.objects.all() # article object를 전부 확인할 수 있음
```
<!-- Django ORM 0325 22page부터 다시 정리 -->
2. instance를 생성함과 동시에 값을 할당
```python
article = Article(title='second', content='django')

# 마찬가지로 save를 하지 않으면 DB에 값이 저장되지 않음
article.save()
```
3. QuerySet API 중 create() 메서드 활용
```python
Article.objects.create(title='third', content='django')
# save가 없어도 DB에 값이 저장된다
```

### Read
데이터 조회  
대표적인 조회 메서드
- Return new QuerySets
  - all(): 전체 데이터 조회
  ```python
  Article.objects.all()
  ```
  - filter(): 특정 조건의 데이터 조회
  ```python
  Article.objects.filter(content='django')
  ```
- Do not return QuerySets
  - get(): 단일 데이터 조회
  ```python
  Article.objects.get(pk=1)
  ```

get의 특징
- 객체를 찾을 수 없으면 DoesNotExist 예외를 발생시키고, 둘 이상의 객체를 찾으면 MultipleObjectsReturned 예외를 발생시킴
- 위와 같은 특징을 가지고 있기 때문에 primary key와 같이 고유성을 보장하는 조회에서 사용해야 함

### Update
데이터 수정  
인스턴스 변수를 변경 후 save 메서드 호출
```python
article = Article.objects.get(pk=1)
article.title = 'byebye'
article.save()
```

### Delete
데이터 삭제  
삭제하려는 데이터 조회 후 delete 메서드 호출
```python
article = Article.objects.get(pk=1)
article.delete()  # 삭제 후 반환함
Article.objects.get(pk=1)  # 삭제한 데이터는 더이상 조회할 수 없음
```

## 참고

### Field lookups
- 특정 레코드에 대한 조건을 설정하는 방법
- QuerySet 메서드 filter(), exclude() 및 get()에 대한 키워드 인자로 지정

```python
# content column에 dja가 포함된 모든 데이터 조회
Article.objects.filter(content__contains='dja')
```

### ORM, QuerySet API를 사용하는 이유
- 데이터베이스 쿼리를 추상화하여 Django 개발자가 데이터베이스와 직접 상호작용하지 않아도 되도록 함
- 데이터베이스와의 결합도를 낮추고 개발자가 더욱 직관적이고 생산적으로 개발할 수 있도록 도움