---
layout: post
title: "django-로그인, 로그아웃 및 authentication 구현하기"
category: django
---

# Authentication?

어떤 사이트를 만들든지 필수적으로 들어가는 기능 중 하나는 **로그인/로그아웃** 기능이다. 블로그의 예를 들면 인증된 사용자를 판별하고, 그에 따라 적절한 권한을 부여해 회원만 글을 작성할 수 있고, 작성자만 글을 수정할 수 있게 하는 등의 일 등을 처리할 수 있다.

django도 방대한 라이브러리를 가진 웹프레임워크답게 간편하게 authentication을 구현할 수 있다.

일단 이 게시글에서는 간단하게 로그인과 로그아웃, 비회원의 기능 제한 등만 다룬다.

---

## 비회원의 접근 차단하기

1. **html 단에서 버튼 숨기기**

```html
{% if user.is_authenticated %}
<a href="{% url 'post_new' %}" class="top-menu"
    ><span class="glyphicon glyphicon-plus">+</span></a
>
{% endif %}
```

위와 같이 단순한 if문으로 처리할 수 있다. `uesr.is_authenticated`를 이용해 로그인한 유저에게만 글 작성 버튼이 보이도록 한다

2. **실제 url 접근 차단하기**

1번만 완료한 상태라면 버튼은 보이지 않아도 '/post/new' 이렇게 _직접 URL을 쳐서 들어가는 접근을 차단할 수는 없다._ 그러면 안되니까 **views.py에서 설정** 해줘야 한다.

장고 자체적으로 가진 auth decorators를 이용해 쉽게 만들 수 있다.

```py
from django.contrib.auth.decorators import login_required

@login_required
def post_remove(request, pk):
    post = get_object_or_404(Post, pk=pk)
    post.delete()
    return redirect('post_list')
```

위와 같이 auth decorators 중 login_required를 import해서 authentication을 적용하고 싶은 함수 바로 위에 `@login_required`을 써서 손쉽게 구현가능하다.

이제 로그인하지 않은 사용자가 강제로 URL을 통한 접근을 시도하면 404 에러를 보게 될 것이다.

---

## 로그인

로그인 기능은 authentication에 속해있기 때문에 blog 폴더를 벗어나 mysite 폴더 안에서 구현한다.

1. **URL**

```py
# mysite/urls.py

from django.contrib.auth import views

urlpatterns = [
    path('accounts/login/', views.LoginView.as_view(), name='login'),
]
```

로그인 액션도 django 자체적으로 가지고 있기 때문에 간단하게 구현할 수 있다.

우선 urls.py에서 url을 설정한다. contrib.auth에서 views를 import 해 로그인액션을 구현한다. views에 딸린 method로 `LoginView` 외에도 `LogoutView`, `ChangePassword`등이 다양하게 있다.

`as_view()`는 템플릿을 설정하게 해준다. 괄호 안을 비워놓으면 /templates/registration/login.html 이 기본으로 호출된다. 다른 이름을 설정하고 싶다면 `as_view(template_name='change-password.html')`와 같이 쓰면 된다.

2. **템플릿**

앞서 언급했듯 LoginView 액션은 자체적으로 **./blog/templates/registration/login.html** 를 렌더링한다. 따라서 규칙에 맞춰 templates 폴더 안에 registration 폴더를 만들고, 그 안에 login.html 를 만들어 로그인 폼을 작성해야 한다.

3. **settings.py**

이대로 끝내면 로그인을 하면 admin 페이지로 리다이렉트가 된다. 자체적으로 django가 설정된 건지, 내가 만들어 놓은 계정이 superuser라 그런진 잘 모르겠지만.. 아무튼 나는 루트페이지로 가고싶으니까 settings.py 파일을 바꿔줘야 한다.

```py
LOGIN_REDIRECT_URL = '/'
```

./mysite/setting.py 파일을 열어 아무데나 위와 같은 코드를 추가해주면 된다. 마찬가지로 응용해서 '/' 대신 원하는 페이지의 url을 입력하면 그쪽으로 redirect 시킬 수 있다.

---

## 로그아웃

로그아웃은 로그인과 비슷하지만 더 간단하게 구현할 수 있다.

```py
# mysite/urls.py

urlpatterns = [
    path('accounts/login/', views.LoginView.as_view(), name='login'),
    path('accounts/logout/', views.LogoutView.as_view(),
         name='logout'),
]

# mysite/settings.py

LOGIN_REDIRECT_URL = '/'
LOGOUT_REDIRECT_URL = '/'
```

mysite/urls.py에 들어가 로그인과 거의 비슷하게 구현한다. Login이 Logout으로 바뀐 것만 빼면 모두 같다.

마찬가지로 settings.py 에서 로그인 바로 아래에 logout으로 바꿔서 redirect url을 추가해주면 된다.

### 이렇게 하면 간단한 로그인, 로그아웃 및 authentication 구현 끝!
