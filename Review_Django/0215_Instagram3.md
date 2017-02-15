#### Post List를 보여주는 화면을 구성
```
1. View에 post_list함수 작성
2. Template에 post_list.html파일 작성
3. View에서 post_list.html을 render한 결과를 리턴하도록 함
4. instagram/urls.py에
    post/urls.py를 연결시킴 (app_name은 post)
5. '/post/'로 접속했을 때 post_list View에 연결되도록
    post/urls.py에 내용을 작성
6. 전체 Post를 가져오는 쿼리셋을
    context로 넘기도록 post_list 뷰에 구현
7. post_list.html에서 {% for %}태그를 사용해
    post_list의 내용을 순회하며 표현
```

post/views.py
```python
from django.shortcuts import render, redirect

from post.models import Post, Comment


def post_list(request):
    posts = Post.objects.all()
    context = {
        'posts': posts,
    }
    return render(request, 'post/post_list.html', context)
```

2. Template에 post_list.html파일 작성
templates/post/post_list.html
```python
 def post_list(request):
    """
    :param request:
    :return:
    """
    post = Post.objects.all()
    context = {"post":post,}

    return render(request, 'post/post_list.html', context)
```

post/urls
```python
from django.conf.urls import url

from . import views

app_name = 'post'
urlpatterns = [
    url(r'', views.post_list, name='post_list'),
```

main/urls
```python
url(r'^post/', include('post.urls')),
```

templates/post/post_list.html
```html
{% extends 'common/base.html' %}

{% block content %}
	{% for post in posts %}
		{{ post.id }} | {{ post.author.username }}<br>
		<img src="{{ MEDIA_URL }}{{ post.photo }}"><br>
		{{ post.content|linebreaksbr|truncatechars:100 }}
		<hr>
		{% if post.comment_set.all %}
			{% for comment in post.comment_set.all %}
				<div>
					{{ comment.content }}
				</div>
			{% endfor %}
		{% else %}
			<p>No comments</p>
		{% endif %}
		<hr>
	{% endfor %}
{% endblock %}
```
---


#### media
>출처 : django document
**Serving files uploaded by a user during development**
During development, you can serve user-uploaded media files from MEDIA_ROOT using the django.contrib.staticfiles.views.serve() view.
This is not suitable for production use! For some common deployment strategies, see Deploying static files.
For example, if your MEDIA_URL is defined as /media/, you can do this by adding the following snippet to your urls.py:
from django.conf import settings
from django.conf.urls.static import static
urlpatterns = [
    # ... the rest of your URLconf goes here ...
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
Note
This helper function works only in debug mode and only if the given prefix is local (e.g. /media/) and not a URL (e.g. http://media.example.com/).

---
#### 미디어 파일 경로 연결
- django_app 하위에 media 폴더 생성. media 폴더 하위에 해당 폴더(ex-post) 만든 뒤에 이미지 파일 저장될 공간으로 설정함

- urls파일에 다음과 같이 추가
main/urls.py
```python
from django.conf.urls.static import static
urlpatterns = [
    url(r'^admin/', admin.site.urls),
    url(r'^member/', include('member.urls')),
    url(r'^post/', include('post.urls')),
    url(r'^post_detail/', include('post.urls')),
] + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```
하단의 +static부분 추가한 내용에 주목

- settings.py에 추가
```python
STATIC_URL = '/static/'
MEDIA_URL = '/media/'
```

- import os에 추가
main/settings.py
```python
import os
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

- MEDIA_URL을 템플릿에서 사용하려면
main.settings.py에서 templates에 추가
```python
'django.template.context_processors.media',
```
---


>_에러 메시지 세부 정보 찾기_
External Libraries
site-packages
django
templatetags

#### Comment 등록하기
```
Post Detail에 댓글작성기능 추가
1. request.method에 따라 로직 분리되도록 if/else블록 추가
2. request.method가 POST일 경우, request.POST에서 'content'키의 값을 가져옴
3. 현재 로그인한 유저는 request.user로 가져오며, Post의 id값은 post_id인자로 전달되므로 두 내용을 사용
4. 위 내용들과 content를 사용해서 Comment객체 생성 및 저장
5. 다시 아까 페이지 (Post Detail)로 redirect해줌
```

- templates/post/post_list.html
```html
{% if post.comment_set.all %}
  {% for comment in post.comment_set.all %}
	 <div>
		  {{ comment.content }}
	 </div>
	{% endfor %}
{% else %}
	<p>No comments</p>
{% endif %}
```

- post/admin.py
from .models import Post, Comment
admin.site.register(Comment)

- post_detail 구성
post/views.py
```python
def post_detail(request, post_id):
    # 인자로 전달된 post_id와 일치하는 id를 가진 Post객체 하나만 조회
    post = Post.objects.get(id=post_id)
    context = {
        'post': post,
    }
    return render(request, 'post/post_detail.html', context)
```

- connmet_add 구성
post/views.py
```python
def comment_add(request, post_id):
    if request.method == 'POST':
        # 전달받은 POST데이터에서 'content'값을 할당
        content = request.POST['content']
        # HttpRequest에는 항상 User정보가 전달된다
        user = request.user
        # URL인자로 전달된 post_id값을 사용
        post = Post.objects.get(id=post_id)

        # post의 메서드를 사용해서 Comment객체 생성
        # post.add_comment(user, content)

        # Comment객체를 만들어준다
        Comment.objects.create(
            author=user,
            post=post,
            content=content
        )
        # 다시 해당하는 post_detail로 리다이렉트
        return redirect('post:detail', post_id=post_id)
```

- templates/post/post_detail.html 만들기
```html
<div>{{ post.created_date }}</div>
<div>
	<img src="{{ MEDIA_URL }}{{ post.photo }}" alt="">
	<p>
		{{ post.content|linebreaksbr }}
	</p>
</div>
<hr>
<div>
	<p>Comments</p>
	{% if post.comment_set.all %}
		<ul>
			{% for comment in post.comment_set.all %}
			<li>{{ comment.author.username }} : {{ comment.content }}</li>
			{% endfor %}
		</ul>
	{% else %}
		<p>No comments</p>
	{% endif %}

	<!-- Comment Form -->
	<form action="{% url 'post:comment_add' post_id=post.id %}" method="POST">{% csrf_token %}
		<input type="text" name="content">
		<button type="submit">Write comment</button>
	</form>
</div>
```
-post/urls.py
```python
app_name = 'post'
urlpatterns = [
    url(r'^$', views.post_list, name='list'),
    url(r'^(?P<post_id>[0-9]+)/$', views.post_detail, name='detail'),
    url(r'^(?P<post_id>[0-9]+)/comment/add/$', views.comment_add, name='comment_add'),
]
```
