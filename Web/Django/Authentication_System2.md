# Authentication System 2

## 회원 가입
User 객체를 Create하는 과정  

UserCreationForm()
- 회원 가입시 사용자 입력 데이터를 받는 built-in ModelForm

다만, 그대로 사용하면 기존 유저 모델을 사용하기 때문에 오류가 발생
```python
from django.contrib.auth import get_user_model
from django.contrib.auth.forms import UserCreationForm, UserChangeForm


class CustomUserCreationForm(UserCreationForm):
    class Meta(UserCreationForm.Meta):
        model = get_user_model()


class CustomUserChangeForm(UserChangeForm):  # 이 부분에 대한 설명은 아래에 존재
    class Meta(UserChangeForm.Meta):
        model = get_user_model()
```

get_user_model()
- 현재 프로젝트에서 활성화된 사용자 모델(active user model)을 반환하는 함수  

왜 User 모델을 직접 참조하지 않는가?
- get_user_model()을 사용해 User 모델을 참조하면 커스텀 User 모델을 자동으로 반환하기 때문
- Django는 필수적으로 User 클래스를 직접 참조하는 대신 get_user_model()을 사용해 참조해야 한다고 강조

```python
def signup(request):
    if request.method == 'POST':
        form = CustomUserCreationForm(request.POST)
        if form.is_valid():
            user = form.save()
            auth_login(request, user)  # 회원가입 후 로그인까지 진행
            return redirect('...')
    else:
        form = CustomUserCreationForm()
    context = {
        'form': form,
    }
    return render(request, 'accounts/signup.html', context)
```

## 회원 탈퇴
User 객체를 Delete하는 과정
```python
def delete(request):
    request.user.delete()  # 1. 탈퇴
    auth_logout(request)  # 2. 로그아웃의 순서가 바뀌면 안됨
    # 먼저 로그아웃이 진행되면 해당 요청 객체 정보가 없어지기 때문에 탈퇴에 필요한 유저 정보 또한 없어지기 때문
    return redirect('...')
```

## 회원정보 수정
User 객체를 Update하는 과정  

UserChangeForm()
- 회원정보 수정 시 사용자 입력 데이터를 받는 built-in ModelForm

```python
def update(request):
    if request.method == 'POST':
        form = CustomUserChangeForm(request.POST, instance=request.user)
        if form.is_valid():
            form.save()
            return redirect('...')
    else:
        form = CustomUserChangeForm(instance=request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/update.html', context)
```

UserChangeForm 사용 시 문제점
- User 모델의 모든 정보들(fields)까지 모두 출력되어 수정이 가능하기 때문에 일반 사용자들이 접근해서는 안되는 정보는 출력하지 않도록 해야 함
- CustomUserChangeForm에서 접근 가능한 필드를 다시 조정

```python
class CustomUserChangeForm(UserChangeForm):
   class Meta(UserChangeForm.Meta):
      model = get_user_model()
      fields = ('first_name', 'last_name', 'email', )
```

## 비밀번호 변경
인증된 사용자의 Session 데이터를 Update하는 과정  

PasswordChangeForm()
- 비밀번호 변경 시 사용자 입력 데이터를 받는 built-in Form  

비밀번호 변경 페이지 작성
- Django는 비밀번호 변경 페이지를 회원정보 수정 form에서 별도 주소(/user_pk/password/)로 안내

```python
def change_password(request, user_pk):
    if request.method == 'POST':
        form = PasswordChangeForm(request.user, request.POST)
        if form.is_valid():
            user = form.save()
            update_session_auth_hash(request, user)  # 아래에서 설명
            return redirect('...')
    else:
        form = PasswordChangeForm(request.user)
    context = {
        'form': form,
    }
    return render(request, 'accounts/change_password.html', context)
```

### 세션 무효화 방지하기
- 비밀번호가 변경되면 기존 세션과의 회원 인증 정보가 일치하지 않게 되어버려 로그인 상태가 유지되지 못하고 로그아웃 처리됨
- 비밀번호가 변경되면서 기존 세션과의 회원 인증 정보가 일치하지 않기 때문  

=> update_session_auth_hash(request, user)
- 암호 변경 시 세션 무효화를 막아주는 함수
- 암호가 변경되면 새로운 password의 Sesstion Data로 기존 session을 자동으로 갱신

## 인증된 사용자에 대한 접근 제한
### 로그인 사용자에 대해 접근을 제한하는 2가지 방법
1. is_autenticated 속성(attribute)
- 사용자가 인증되었는지 여부를 알 수 있는 User model의 속성
- 모든 User 인스턴스에 대해 항상 True인 읽기 전용 속성이며, 비인증 사용자에 대해서는 항상 False
- request.user.is_authenticated 로 사용
2. login_required 데코레이터(decorator)
- 인증된 사용자에 대해서만 view 함수를 실행시키는 데코레이터
- 비인증 사용자의 경우 /accounts/login/ 주소로 redirect 시킴


```python
from django.contrib.auth.decorators import login_required


@login_required
def logout(request):
    ...
```