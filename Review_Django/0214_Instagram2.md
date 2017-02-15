#### 유저모델, 관리자페이지
> 오류 내용 django documents 확인해가면서 수정하기

html파일에 form구현 기본 형태
login.html
```python
<form action="" method="POST">
    <input type="text" name="username">
    <input type="password" name="password">
    <button type="submit">Login</button>
</form>
```
csrf 오류 발생시
```python
<form action="" method="POST">{% csrf_token %}
    <input type="text" name="username">
    <input type="password" name="password">
    <button type="submit">Login</button>
</form>
```
>Shell에서 로그인 비밀번호 변경하는 방법
./manage.py changepassword 아이디

forms.py만들어서 구현
member/forms.py
```python
from django import forms

class LoginForm(forms.Form):
    username = forms.CharField(max_length=30)
    password = forms.CharField(max_length=30, widget=forms.PasswordInput)
```

member/views.py 수정
```python
from django.http import HttpResponse
from django.shortcuts import render, redirect
from django.contrib.auth import authenticate, login

from .forms import LoginForm


def login_fbv(request):
    if request.method == 'POST':
        # LoginForm을 사용
        form = LoginForm(data=request.POST)
        if form.is_valid():
            # 전달되어온 POST데이터에서 'username'과 'password'키의 값들을 사용
            username = form.cleaned_data['username']
            password = form.cleaned_data['password']
            # authenticate의 인자로 POST로 전달받은 username, password를 사용
            user = authenticate(username=username, password=password)

            # 만약 인증이 정상적으로 완료되었다면
            # (해당하는 username, password에 일치하는 User객체가 존재할경우)
            if user is not None:
                # Django의 인증관리 시스템을 이용하여 세션을 관리해주기 위해 login()함수 사용
                login(request, user)
                return redirect('/admin')
            # 인증에 실패하였다면 (username, password에 일치하는 User객체가 존재하지 않을 경우)
            else:
                form.add_error(None, 'ID or PW incorrect')

    # GET method로 요청이 왔을 경우
    else:
        # 빈 LoginForm객체를 생성
        form = LoginForm()

    context = {
        'form': form,
    }
    # member/login.html 템플릿을 render한 결과를 리턴
    return render(request, 'member/login.html', context)
```
templates/member/login.html 수정
```python
<form action="" method="POST">{% csrf_token %}
    {% if form.non_field_errors %}
        {{ form.non_field_errors }}
    {% endif %}

    {% for field in form %}
    <div>
        {{ field.label_tag }}
        {{ field }}
    </div>
    {% endfor %}
    <button type="submit">Login</button>
</form>
```

member/models.py 새로 작성
```python
from django.contrib.auth.base_user import AbstractBaseUser, BaseUserManager
from django.contrib.auth.models import PermissionsMixin
from django.db import models


class MyUserManager(BaseUserManager):
    def create_user(self, username, password=None):
        user = self.model(
            username=username
        )
        user.set_password(password)
        user.save()
        return user

    def create_superuser(self, username, password):
        user = self.model(
            username=username
        )
        user.set_password(password)
        user.is_staff = True
        user.is_superuser = True
        user.save()
        return user


class MyUser(PermissionsMixin, AbstractBaseUser):
    # 다중상속으로 관리자페이지에 로그인 할 수 있는 모든 속성과 메서드를 갖춤
    CHOICES_GENDER = (
        ('m', 'Male'),
        ('f', 'Female'),
    )
    # 기본값
    # password
    # last_login
    # is_active
    # username이라는 필드를 만들고 USERNAME_FIELD에 추가한 후 makemigrations시도해보기
    username = models.CharField(max_length=30, unique=True)
    email = models.EmailField(blank=True)
    gender = models.CharField(max_length=1, choices=CHOICES_GENDER)
    nickname = models.CharField(max_length=20)

    is_staff = models.BooleanField(default=False)

    USERNAME_FIELD = 'username'
    objects = MyUserManager()

    def get_full_name(self):
        return '{} ({})'.format(
            self.nickname,
            self.username
        )

    def get_short_name(self):
        return self.nickname
```
기존 db 삭제
rm db.sqlite3
setting에
AUTH_USER_MODEL = 'member.MyUser' 추가
'MyUser' has no attributes
