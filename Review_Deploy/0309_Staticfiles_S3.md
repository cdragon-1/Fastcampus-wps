#### Staticfiles

>Amazon S3란?
Amazon Simple Storage Service는 인터넷용 스토리지 서비스입니다. 개발자가 보다 쉽게 웹 규모 컴퓨팅 작업을 할 수 있도록 설계되었습니다.
Amazon S3에서 제공하는 단순한 웹 서비스 인터페이스를 사용하여 웹에서 언제 어디서나 원하는 양의 데이터를 저장하고 검색할 수 있습니다. 또한 개발자는 Amazon이 자체 웹 사이트의 글로벌 네트워크 운영에 사용하는 것과 같은 높은 확장성과 신뢰성을 갖춘 빠르고 경제적인 데이터 스토리지 인프라에 액세스할 수 있습니다. 이 서비스의 목적은 규모의 이점을 극대화하고 개발자들에게 이러한 이점을 제공하는 것입니다.

**AWS SDK for Python (Boto3) 사용**
settings.py와 .conf의 json파일 수정하기(Boto3 문서 참조하여)

개발용 스크립트 만들기
vi ~/.scripts/manage
1 #!/usr/bin/zsh
2 MODE='DEBUG' ./manage.py $*
chmod 755 ~/.scripts/manages

vi ~/.scripts/manage3
1 #!/usr/bin/zsh
2 MODE='DEBUG' STORAGE='S3' ./manage.py $*
chmod 755 ~/.scripts/manages3

pip install django storages
Installed apps에 storages 추가

manages3 collectstatic 실행하여 파일이 잘 올라가는지 확인한다.
아마존 S3 bucket에서 확인가능

manages3 runserver
에러 발생
staticfiles url 설정 다시하기
aws의 이미지 링크 주소를 변수값으로 할당하여 settings에 반영

pip install -r requirements.txt
MODE='DEBUG' STORAGE='S3' ./manage.py runserver 0:8080

db instance:
fc-db
cyi

FastCampus EC2 Deploy / sg-58a37030

FC EC2_Deploy rds / sg-9c67baf4

FC EC2_Deploy rds

fc-db.c9chpniabz2m.ap-northeast-2.rds.amazonaws.com:5432

❯ MODE='DEBUG' DB='RDS' STORAGE='S3' ./manage.py runserver


실행중인 runserver 확인
ps -ax | grep runserver   
런서버 강제종류
❯ sudo kill -9 20805 20936 21975            

파일 삭제후 연관 파일들까지 제거하기
sudo apt-get remove postgresql
sudo apt-get autoremove
