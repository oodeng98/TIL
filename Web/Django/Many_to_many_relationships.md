# Many to many relationships
한 테이블의 0개 이상의 레코드가 다른 테이블의 0개 이상의 레코드와 관련된 경우
- 양쪽 모두에서 N:1관계를 가지는 것

## N:1의 한계
한 명의 의사에게 여러 환자가 예약할 수 있도록 설계
```python
class Doctor(models.Model):
    name = models.TextField()
    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    name = models.TextField()
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'
```
1번 환자 carol이 두 의사 모두에게 진료를 받고자 한다면 환자 테이블에 1번 환자 데이터가 중복으로 입력될 수 밖에 없음
![N:1 duplicate](./img/N_1_duplicate.PNG)

- 동일한 환자지만 다른 의사에게도 진료 예약을 하기 위해서는 객체를 하나 더 만들어서 진행해야 함
- 또한 외래 키 컬럼에 '1, 2' 형태로 저장하는 것은 DB 타입 문제로 불가능  

=> 예약 테이블을 따로 만들어서 관리

## 중개 모델
환자의 외래 키를 삭제하고 별도의 예약 모델을 새로 생성
- 예약 모델은 의사와 환자에 각각 N:1 관계를 가짐  
```python
# 외래키 삭제
class Patient(models.Model):
    name = models.TextField()
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'


# 중개모델 작성
class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    def __str__(self):
        return f'{self.doctor_id}번 의사의 {self.patient_id}번 환자'
```
![relation table](./img/relation_table.PNG)

**Django에서는 'ManyToManyField'로 중개 모델을 자동으로 생성**

## ManyToManyField()
M:N 관계 설정 모델 필드
1. 환자 모델에 ManyToManyField 작성
- 의사 모델에 작성해도 상관 없으며 참조/역참조 관계만 잘 기억할 것
```python
class Doctor(models.Model):
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 의사 {self.name}'


class Patient(models.Model):
    # ManyToManyField 작성
    doctors = models.ManyToManyField(Doctor)
    name = models.TextField()

    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'


# Reservation Class 주석 처리
```
2. Migration 진행 후 생성된 테이블 확인
![relation table schema](./img/relation_table_schema.PNG)

3. 의사, 환자 생성 및 관계 설정
```python
doctor1 = Doctor.objects.create(name='allie')
patient1 = Patient.objects.create(name='carol')
patient2 = Patient.objects.create(name='duke')

# patient1이 doctor1에게 예약
patient1.doctors.add(doctor1)
# patient1이 자신이 예약한 의사 목록 확인
patient1.doctors.all()
# doctor1이 자신에게 예약된 환자 목록 확인
doctor1.patient_set.all()
# doctor1이 patient2를 예약
doctor1.patient_set.add(patient2)
# 예약 취소하기
# doctor1이 patient1 진료 예약 취소
doctor1.patient_set.remove(patient1)
# patient2가 doctor1의 진료 예약 취소
patient2.doctor.remove(doctor1)
```
만약 예약 정보에 병의 증상, 예약일 등 추가 정보가 포함되어야 한다면?


## 'through' argument
중개 테이블에 추가 데이터를 사용해 M:N관계를 형성하려는 경우에 사용  
Reservation Class 재작성 및 through 설정
```python
class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, through='Reservation')
    name = models.TextField()
    def __str__(self):
        return f'{self.pk}번 환자 {self.name}'


class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    symptom = models.TextField()
    reserved_at = models.DateTimeField(auto_now_add=True)
    def __str__(self):
        return f'{self.doctor.pk}번 의사의 {self.patient.pk}번 환자'
```

예약 생성 방법
```python
# 방법 1
reservation1 = Reservation(doctor=doctor1, patient=patient1, symptom='headache')
reservation1.save()

# 방법 2
patient2.doctors.add(doctor1, through_defaults={'symptom': 'flu'})
```

M:N 관계 주요 사항
- M:N 관계로 맺어진 두 테이블에는 물리적인 변화가 없음
- ManyToManyField는 중개 테이블을 자동으로 생성
- ManyToManyField는 M:N 관계를 밎는 두 모델 어디에 위치해도 상관 없음
  - 단, 필드 작성 위치에 따라 참조와 역참조 방향을 주의할 것
- N:1은 완전한 종속의 관계였지만 M:N은 종속적인 관계가 아니며 의사에게 진찰받는 환자, 환자를 진찰하는 의사 이렇게 2가지 형태 모두 표현 가능

## ManyToManyField(to, **options)
M:N 관계 설정 시 사용하는 모델 필드  
ManyToManyField의 대표 인자 3가지
1. related_name
- 역참조시 사용하는 manager name을 변경
```python
class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, related_name='patients')
    name = models.TextField()
# 변경 전
doctor.patient_set.all()
# 변경 후
doctor.patients.all()
```
2. symmetrical
- 관계 설정 시 대칭 유무 설정
- ManyToManyField가 동일한 모델을 가리키는 정의에서만 사용
- 기본 값: True
  - True인 경우
    - source 모델의 instance가 target 모델의 instance를 참조하면 자동으로 target 모델의 instance도 source 모델의 instance를 참조하도록 함(대칭)
    - 즉, 내가 누군가의 친구라면 누군가도 내 친구가 됨
  - False인 경우
    - True와 반대(대칭되지 않음)
```python
class Person(models.Model):
    friends = models.ManyToManyField('self')
```
3. through
- 사용하고자 하는 중개 모델 지정
- 일반적으로 추가 데이터를 M:N관계와 연결하려는 경우에 활용
```python
class Patient(models.Model):
    doctors = models.ManyToManyField(Doctor, through='Reservation')

class Reservation(models.Model):
    doctor = models.ForeignKey(Doctor, on_delete=models.CASCADE)
    patient = models.ForeignKey(Patient, on_delete=models.CASCADE)
    ...
```

M:N에서의 대표 methods
- add()
  - 지정된 객체를 관련 객체 집합에 추가
  - 이미 존재하는 관계에 사용하면 관계가 복제되지 않음
- remove()
  - 관련 객체 집합에서 지정된 모델 객체를 제거

## 좋아요 기능 구현
Article(M) - User(N)  
0개 이상의 게시글은 0명 이상의 회원과 관련
- 게시글은 회원으로부터 0개 이상의 좋아요를 받을 수 있고, 회원은 0개 이상의 게시글에 좋아요를 누를 수 있음

### 모델 관계 설정
1. Article class에 ManyToManyField 작성
```python
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL)
    ...
```
2. related_name 설정
그냥 Migration을 진행하면 에러 발생  
- N:1
  - 유저가 작성한 게시글
  - user.article_set.all()
- M:N
  - 유저가 좋아요 한 게시글
  - user.article_set.all()  
역참조 매니저 충돌
- like_users 필드 생성 시 자동으로 역참조 매니저 .article_set이 생성됨
- 그러나 이전 N:1(Article-User) 관계에서 이미 같은 이름의 매니저를 사용 중
- user와 관계된 ForeignKey 혹은 ManyToManyField 둘 중 하나에 related_name 작성 필요

```python
class Article(models.Model):
    user = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    like_users = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='like_articles')
    ...
```

User-Article 간 사용 가능한 전체 related manager
- article.user
  - 게시글을 작성한 유저 - N:1
- user.article_set
  - 유저가 작성한 게시글(역참조) - N:1
- article.like_users
  - 게시글을 좋아요 한 유저 - M:N
- user.like_articles
  - 유저가 좋아요 한 게시글(역참조) - M:N