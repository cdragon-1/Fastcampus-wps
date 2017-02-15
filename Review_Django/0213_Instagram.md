#### Topic : Instagram Project

member/models.py
```python
from django.db import models, IntegrityError


class MyUser(models.Model):
    username = models.CharField('유저네임', max_length=30, unique=True)
    last_name = models.CharField('성', max_length=20)
    first_name = models.CharField('이름', max_length=100)
    nickname = models.CharField('닉네임', max_length=100)
    email = models.EmailField('이메일', blank=True)
    date_joined = models.DateField(auto_now=True)
    last_modified = models.DateTimeField(auto_now=True)
    following = models.ManyToManyField(
        'self',
        related_name='follower_set',
        blank=True,
        symmetrical=False,
    )

    def __str__(self):
        return self.username

    def follow(self, user):
        self.following.add(user)

    def unfollow(self, user):
        self.following.remove(user)

    @property
    def followers(self):
        return self.follower_set.all()

    def change_nickname(self, new_nickname):
        self.nickname = new_nickname
        self.save()

    @staticmethod
    def create_dummy_user(num):
        """
        num개수만큼 User1~User<num>까지 임의의 유저를 생성한다.
        :return
        """
        import random
        last_name_list = ['김', '이', '박', '최']
        first_name_list = ['민아', '혜리', '소진', '아영']
        nickname_list = ['빵', '리헬', '쏘지', '율곰']
        created_count = 0
        for i in range(num):
            try:
                MyUser.objects.create(
                    username='User{}'.format(i + 1),
                    last_name=random.choice(last_name_list),
                    first_name=random.choice(first_name_list),
                    nickname=random.choice(nickname_list),
                )
                created_count += 1
            except IntegrityError as e:
                print(e)

                pass

        return created_count



    @staticmethod
    def assign_global_variables():
        # sys모듈은 파이썬 인터프리터 관련 내장모듈
        import sys
        # __main__모듈을  module변수에 할당
        module = sys.modules['__main__']
        # MyUser객체 중 'User'로 시작하는 객체들만 조회하여 users변수에 할당
        users = MyUser.objects.filter(username__startswith='User')
        # users를 순회하며
        for index, user in enumerate(users):
            # __main__모듈에 'u1, u2, u3...'이름으로 각 MyUser객체를 할당
            setattr(module, 'u{}'.format(index + 1), user)

```
---
#### 실습 조건
프로젝트로 사용할 폴더 생성
pyenv virtualenv 3.4.3 <환경명>
pyenv local <환경명>
pip install django
django-admin startproject <프로젝트명>
mv <프로젝트명> django_app
Pycharm interpreter세팅
django_app폴더를 Sources root로 설정
프로젝트명
instagram

DB모델 설계

member app

MyUser모델

Attributes

username
유일한 값을 갖도록 함
last_name
first_name
nickname
email
date_joined
last_modified
following (팔로우하고 있는 User목록)
Methods

follow (팔로우 처리)
인자로 다른 MyUser객체를 받아 해당 MyUser객체를 팔로우하도록 함
unfollow (언팔로우 처리)
위와 반대 동작
followers (자신을 팔로우하고 있는 User목록, Property처리)
change_nickname
post app

Post 모델

Attributes

author (ForeignKey, User)
photo
like_users (Intermediate모델로 PostLike를 사용)
content
created_date
Methods

add_comment
like_count (property)
comment_count (property)
Comment 모델

Attributes

author (ForeignKey, User)
post (ForeignKey, Post)
content
created_date
PostLike 모델 (중간자 모델)

Attributes

user (ForeignKey, User)
post (ForeignKey, Post)
created_date

---

#### Intagram app 실습 내용 정리
INSTALLED_APPS에 등록
'members.apps.MemberConfig',
./manage.py makemigrations
./manage.py migrate
./manage.py shell
from member.models import MyUser
MyUser.objects.create(username='cyi', lastname='최', first_name='용일', nickname='cdragon')
u1 = MyUser.objects.get(id=1)
**random함수로 만들었을 경우**
MyUser.create_dummy_user(10)
MyUser.objects.all()

---

def assign_global_variables():
...
from member.models import MyUser
MyUser.assign_global_variables()
확인하려면
globals()
>pip install django-extensions
설치함
import 편의를 위해
pip install ipython
INSTALLED_APPS에 'django-extensions', 명시
django_app 폴더에서
./manage.py shell_plus 실행
model.py 내용이 자동 임포트

MyUser.assign_global_variables()
u1~u10 user가 할당됐는지 확인
u1.following
u1.following.add(2)
u1.following.add(3)
u1.following.all()
<QuerySet [<MyUser: User2>, <MyUser: User3>]>

---
u2에서 나의 팔로워를 알고 싶으면?
u2.myuser_set.all()
_ManyToManyField or ForeignKey에서 서로 참조된 값의 역참조를 할 때는,, 역참조하는 클래스명을 소문자로 바꾼뒤 \_set이 기본값이다._
following에
related_name = 'follower_set', 할당 후에
u2.follower_set
u2.follower_set.all()
역참조 확인 가능

---
```python
from django.db import models

from member.models import MyUser


class Post(models.Model):
    author = models.ForeignKey(MyUser)
    photo = models.ImageField(upload_to='post', blank=True)
    content = models.TextField(blank=True)
    created_date = models.DateTimeField(auto_now_add=True)
    like_users = models.ManyToManyField(
        MyUser,
        through='PostLike',
        related_name='like_post_set',
    )

    def __str__(self):
        return 'Post[{}]'.format(self.id)

    def toggle_like(self, user):
        pl_list = PostLike.objects.filter(post=self, user=user)
        # # 현재 인자로 전달되 user가 해당 Post(self)를 좋아요 한 적이 있는지 검사
        # if pl_list.exists():
        #     pl_list.delete()
        #     # 만약에 이미 좋아요를 했을 경우 해당 내역을 삭제
        #     pl_list.delete()
        # else:
        #     # 아직 내역이 없을 경우 생성해준다
        #     PostLike.objects.create(post=self, user=user)
        #
        # #파이썬 삼항연산자
        # # [True일 경우 실행할 구문] if 조건문 else [False일 경우 실행할 구문]
        pl_list.delete() if pl_list.exists() else PostLike.objects.create(post=self, user=user)
        # return PostLike.objects.create(post=self, user=user) if not pl_list.exists()  else  else pl_list.delete()

    def add_comment(self, user, content):
        # 자신에게 연결된 Comment객체의 역참조 매니저 (comment_set) 로부터
        # create메서드를 이용해  Comment객체를 생성
        return self.comment_set.create(
            user=user,
            content=content
        )

    @property
    def like_count(self):
        return self.like_users.count()

    @property
    def comment_count(self):
        return self.comment_set.count()


class Comment(models.Model):
    author = models.ForeignKey(MyUser)
    post = models.ForeignKey(Post, null=True)
    content = models.TextField()
    created_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return 'Post[{}]\'s Comment[{}], Author[{}]'.format(
            self.post_id,
            self.id,
            self.author_id,
        )


class PostLike(models.Model):
    user = models.ForeignKey(MyUser)
    post = models.ForeignKey(Post)
    created_date = models.DateTimeField(auto_now_add=True)

    class Meta:
        unique_together = (
            ('user', 'post'),
        )

    def __str__(self):
        return 'Post[{}]\'s Like[{}]'.format(
            self.post_id,
            self.id,
        )

```
**중간자 모델을 통하는 메쏘드의 경우 remove나 create를 쓸 수 없다.**

shell..
MyUser.assign_global_variables()
p1 = Post.objects.first()
p1
u1
p = Post.objects.create(author=u1, content='Post Content')
p
Post.objects.all()



> alias 설정 방법
vi ~/.bashrc
alias dir='ls -al'
source ~/.bashrc
