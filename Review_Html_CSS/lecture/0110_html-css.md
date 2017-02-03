CSS
:Cascading Style Sheet, 마크업 언어가 실제 표시되는 방법을 기술하는 언어; 레이아웃과 스타일을 정의할 때 사용

HTML5의 등장으로 인해 과거 HTML의 복잡성을 깔끔하고 화려하게 표현해주는 것이 가능하게 됨

CSS문법
선언블럭{selector(규칙을 어디에 적용할 지 결정) {
  property(속성): value;
}

ex)
#body-title {
  font-size: 14px;
  font-weight: bold;
  color: DarkSlateGrey;
}
---
CSS 사용법 3가지
: 내부 스타일 시트, 인라인 스타일 시트, 외부 스타일 시트
---
내부 스타일 시트
```
<!DOCTYPE html>
<html lang="en">
<head>
<style type="text/css">
#body-title {
  font-size: 20px;
  font-weight: bold;
  color: DarkSlateGrey;
}
</style>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Document</title>
</head>
<body>
<p id="body-title">Internal Style Sheet</p>
</body>
</html>
```
---
인라인 스타일 시트
```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Document</title>
</head>
<body>
<p id="body-title" style="font-size:14px; font-weight: bold; color: DarkSlateGrey">Inline style sheet</p>
</body>
</html>
```
**인라인 스타일은 내용과 스타일이 분리되지 않으므로 권장되지 않는다.**

---
외부 스타일 시트(External Style Sheet)
```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Document</title>
<link rel="stylesheet" href="/css/style.css">
</head>
<body>
<p id="body-title">External Style Sheet</p>
</body>
</html>
```
**link태그를 사용하여 외부 CSS파일을 HTML문서에 연결, 현업에서 가장 많이 사용**

---

개발자 도구
: 브라우저에서 HTML/CSS요소를 디버깅하는 법

요소(Elements)
ctrl+shift+c 으로 검사기 사용 가능
요소-스타일 : New style Rule버튼을 누르면, 현재 요소를 특정하는 선택자를 자동으로 생성해줌
콘솔 : 실시간으로 내부의 자바스크립트를 테스트하거나, 로그를 출력해줌
---
CSS 선택자
전체 선택자: HTML페이지 내부의 모든 요소에 같은 CSS속성을 적용한다.
```* {}```
  태그 선택자: 해당하는 모든 HTML태그 요소를 지정함
  h1 {
    color: red;
  }
  p {
    color: gray;
    margin-bottom: 10px;
  }

  그룹 선택자: 여러 선택자에 같은 스타일을 적용하는 경우, 쉼표로 구분해 선택자를 나열해 사용한다
  ```
  #index-title {
    font-size: 20x;
  }
  p#index-description {
    font-size: 30px;
    color: #999;
  }
  #index-title, #index-description {
    text-align: center;
  }
  ```

  복합 선택자
  하위 선택자와 자식 선택자
  : 하위 선택자는 부모요소에 포함된 '모든' 하위 요소를 지정하며,
  자식 선택자는 부모요소의 '바로아래' 자식 요소만을 지정함
  ```
  하위선택자
  section ul {
    border: 1px solid black;
  }
  자식 선택자
  section > ul {
    border: 1px solid black;
  }
  ```
  인접 형제 선택자 : 조건을 충족하는 '첫 번째' 동생 요소만을 지정
  ```
  h1 + ul {
  background: Azure;
  color: DarkBlue;
}
  ```
  일반 형제 선택자 : 조건을 충족하는 '모든' 동생 요소를 지정
  ```
  h1 ~ ul {
  background: Azure;
  color: DarkBlue;
}
  ```
  속성 선택자(Attribute Selector)
  ```
  E[attr] 'attr'속성이 포함된 요소 E
  E[attr="val"] 'attr' 속성의 값이 'val'인 요소 E
  E[attr~="val"] 'attr' 속성의 값에 'val'이 포함되는 요소 E(공백으로 분리된 값이 일치해야 함)
  E[attr|="val"] 'attr' 속성의 값에 'val'이 포함되거나(공백분리), 'val-'로 시작하는 요소 E
  E[attr^="val"] 'attr' 속성의 값이 'val'로 시작하는 요소 E
  E[attr$="val"] 'attr' 속성의 값이 'val'로 끝나는 요소 E
  E[attr*="val"] 'attr' 속성의 값에 'val'이 포함되는 요소 E(공백이나 Dash(-)에 영향받지 않음)
  ```
  가상 클래스 선택자, 가상 엘리먼트 선택자
  :HTML소스에는 존재하지 않지만 필요에 의해 가상의 선택자를 지정
  E:link 방문하지 않은 링크 E
  E:visited 방문한 링크 E
  E:active E 요소에 마우스 클릭 또는 키보드 엔터가 눌린동안
  E:hover E 요소에 마우스가 올라가 있는 동안
  E:focus E 요소에 포커스가 머물러 있는 동안
  E:first-line E 요소의 첫 번째 라인
  E:first-letter E요소의 첫 번째 문자
  E::before E 요소의 시작 지점에 생성된 요소
  E::after E요소의 끝 지점에 생성된 요소
  ```

---
**CSS 우선순위**
: 특정도(specify)는 CSS구문에서 해당하는 스타일의 수*특정도 값을 모두 더한 값
  특정도 계산식
  Inline A
  ID Selector B
  Class Selector C
  TAG Selector D
  우선순위의 흑마법
  important; A~D에 우선한다
  ```
  p {
    font-size: 30px !important;
    color: green !important;
}
  ```

---
CSS 서체
  서체 색상
```
  h1 {
    color: #ff0000;
}
```
HEX코드, rgb(),rgba(), 색상명이 올 수 있다.

  서체 지정
```
  body {
    font-family: “돋움”, dotum, “굴림”, gulim, arial, helvetica, sans-serif;
}
```
웹 페이지를 방문한 사용자가 없을 경우를 대비해, 다양한 서체들을 선언해 놓는다.

  서체의 종류 : Serif, Monospace, Sans-Serif, cursive

  글자 크기
```
  body {
    font-size: 14px;
}
  h1 {
    font-size: 28px(2em);
}
```
**em은 부모 요소로부터의 비율이다.

  글자 스타일
```
  p {
    font-style: italic;
}
```

  글자 굵기
```
  p {
    font-weight: bold;
}
  p {
    font-weight: 700;
}
```

  줄 간격
```
  p {
    line-height: 1.5;
}
```

  문자 꾸미기
```
  p.item1 {
    text-decoration: none;
}
  p.item2 {
    text-decoration: underline;
}
  p.item3 {
    text-decoration: overline;
}
  p.item4 {
    text-decoration: line-through;
}
```

  문자 정렬
```
  p.item1 {
    text-align: left;
}
  p.item2 {
    text-align: center;
}
  p.item3 {
    text-align: right;
}
  p.item4 {
    text-align: justify;
}
```

  문자 들여쓰기
```
  p {
    text-indent: 1em;
}
```  
  대소문자 변환
```
    첫 글자만 대문자로
  p.item1 {
    text-transform: capitalize;
}
    모두 대문자로
  p.item2 {
    text-transform: uppercase;
}
    모두 소문자로
  p.item3 {
    text-transform: lowercase;
}
```

  자간
```
  p {
    letter-spacing: 5px;
}
```

  단어 간격
```
  p {
    word-spacing: 5px;
}
```

  요소간 수직 정렬
  : 박스 내에서의 수직 정렬이 아닌, 나란히 오는 인라인 요소간의 정렬

  줄 바꿈 설정
```
줄바꿈이 없음
  p.item1 {
    white-space: nowrap;
}
줄바꿈, 띄어쓰기, 공백 등 모든것이 보여지며, 박스를 벗어나나도 줄 바꿈이 일어나지 않는다.
  p.item2 {
    white-space: pre;
}
줄 바꿈만 보여주며, 띄어쓰기는 무시한다. 자동으로 줄바꿈이 된다.
  p.item3 {
    white-space: pre-line;
}
줄 바꿈과 띄어쓰기를 모두 보여주며 자동으로 줄바꿈이 된다.
  p.item4 {
    white-space: pre-wrap;
}
```
```
---

**CSS 배경 스타일**

배경색
```
div {
  background-color: #eee;
  background-color: #efefef;
  background-color: rgb(230,222,120);
  background-color: rgba(230, 22, 120, 0.4);
}
Hex code, rgb, alpha(투명도)로 표현 가능함
```

배경 이미지
```
div {
  background-image: url('../images/icon.png');
}
```
이미지의 주소는 상대경로, 절대경로 모두 사용 가능

배경 이미지 반복
```
div {
  background-repeat: no-repeat; 반복하지 않음
  background-repeat: repeat; 가로세로 바둑판 형식으로 반복
  background-repeat: repeat-x; 가로(x축)으로만 반복
  background-repeat: repeat-y; 세로(y축)으로만 반복
}
```

배경 이미지 위치
```
div {
  background-position: 50% 16px;
  background-position: center bottom;
}
center는 가운데(x,y모두 가능) left, right는 x축에
top, bottom은 y축에만 사용
```
%값을 사용할 경우, 이미지 사이즈를 기준으로 움직임.(x축이 100%일 경우, 이미지의 너비만큼 우측에서 보이게 됨)

배경 이미지 고정
```
div {
  background-attachment: local; 요소 안 내용에 고정(스크롤할 때 이미지가 같이 움직임)
  background-attachment: scroll; 요소에 고정(스크롤에 무관)
  background-attachment: fixed; background-position좌표를 웹 페이지 화면 전체기준으로 합니다
}
```

배경 속기법
  background 관련 속성을 한번에 줄여 쓸 수 있으나 잘 쓰이지 않음

---
**CSS 테두리 스타일(CSS broder)**

요소의 방향
시계방향, Top -> Right -> Bottom -> Left 방향으로 값을 정할 수 있다
테두리를 구성하는 요소
  선 굵기(border-width)
```
div {
  border-width: 3px;  상하좌우 모두 3px
  border-top-width: 4px;  상단만 4px
  border-width: 3px 4px 5px 6px;  상 우 하 좌 순서대로
  border-width: 5px 10px; 상하 5px, 좌우 10px
}
```
값을 2개만 적을 경우, 첫 번째 값은 상&하의 값, 두 번째 값은 자&우의 값을 나타냄

  선 형태(border-style)
```
  div {
    border-style: solid;  실선
    border-top-style: double; 점선
    border-style: solid double dashed dotted;
}
```
주로 사용: solid(실선), dotted(점선), dashed(바느질선), double(이중선)

  선 색상(border-color)
```
  div {
    border-color: #aaa;
    border-color: red blue green yellow;
}
```

  속기법
```
  div {
    border: 1px solid red;
}
```

---
**CSS 박스 모델**

박스 모델 : css요소를 이루는 형태
css 요소는 박스 형태를 이루며, 박스는 콘텐츠, 패딩, 보더, 마진의 4가지로 이루어진다

블록 요소와 인라인 요소의 차이
  인라인 요소는 가로마진만 가질 수 있다. 인라인 요소의 길이/높이는 지정이 불가능하다.(내용에 자동으로 맞추어짐)
  블록 요소는 가로/세로 마진을 모두 가진다

바깥 여백(margin)
```
div {
  margin-top: 10px;
  margin-bottom: 30px;
  margin: 10px 0;
  margin: 40px 20px 30px 50px;
}
```

마진 겹침(margin collapse)
```
<div style="margin-bottom:10px;">Box 1</div>
<div style="margin-top:30px;">Box 2</div>
```
두 블록요소의 마진이 서로 겹칠 경우, 해당하는 마진값이 더해지는 것이 아니라 둘 중 큰 값만이 적용된다. 서로 위/아래로 겹치는 마진값을 준 경우, 한 쪽에만 값을 몰아주거나, padding을 활용하는 방식으로 해결해야 한다.

내부 여백(padding)
```
div {
  padding-top: 10px;
  padding-bottom: 10px;
  padding: 10px 0;
  padding: 10px 20px 30px 40px;
}
```

가로, 세로(width, height)
```
div {
  width: 100px;
  height: 50px;
}
```

box-sizing
; box-sizing에 border-box를 지정하면, 요소의 width값에 맞추어 padding, border, margin값을 제외한 width가 새로 적용된다
```
div {
  width: 200px;
  height: 50px;
  padding: 10px;
  border: 1px solid black;
  margin: 15px;
  box-sizing: border-box;
}
```

---
**CSS 리스트 스타일**

리스트 앞 Bullet 타입 설정
```
ul {
  list-style-type: disc;
  list-style-type: circle;
  list-style-type: square;

  list-style-type: decimal; 아라비아숫자
  list-style-type: lower-alpha; 소문자
  list-style-type: upper-alpha; 대문자
  list-style-type: lower-roman; 로마숫자소
  list-style-type: upper-roman; 로마숫자대
}
```

리스트 블릿에 이미지 지정
```
ul {
  list-style-image: url('images/mic.png');
}
```

리슽 스타일 속기법
ul {
  list-style: square url('images/mic.png') inside;
}

---
CSS의 테이블 스타일

테두리 합치기(border-collapse)
```
<table>
  <tr>
    <td>A</td>
    <td>B</td>
    <td>C</td>
  </tr>
  <tr>
    <td>1</td>
    <td>2</td>
    <td>3</td>
  </tr>
  <tr>
    <td>4</td>
    <td>5</td>
    <td>6</td>
  </tr>
</table>

table {
border-collapse: collapse;
}
tr, td {
  border: 3px solid black;
  width: 30px;
  text-align: center;
}

테이블 셀 간격
table {
border-spacing: 10px;
}
tr, td {
  border: 1px solid black;
  width: 30px;
  text-align: center;
}

빈 셀의 표시
table {
border-spacing: 10px;
}
tr, td {
  border: 1px solid black;
  width: 30px;
  text-align: center;
  empty-cells: hide;
}
```

테이블 레이아웃
테이블의 기본 설정은 내용이 긴 쪽이 더 늘어난다
table-layout: auto;
table-layout의 속성을 fixed로 설정하면, 셀 길이가 일정하게 유지된다(이 때, table에는 width property가 설정되어 있어야 한다)

---
**CSS의 화면 표시 속성**
:CSS로 요소가 어떤 모습으로 보일지 결정한다

화면 표시방법
  display:block
```
span {
  display: block;
}
```
기존에 블럭 요소가 아닌 요소를 블럭 속성을 갖도록 한다

  display:inline
```
div {
  display: inline;
}
```
기존에 인라인 요소가 아닌 요소를 인라인 속성을 갖도록 한다

  display:inline-block
```
span {
  display: inline-block;
}
```
인라인 요소에 높이와 상하 패딩, 마진값을 줄 때 사용한다

  display:none
```
div {
  display: none;
}
```
해당 요소의 하위 요소들도 전부 보이지 않게 되며, 공간도 차지하지 않는다
```
div {
  visibility: hidden; 화면에 공간은 차지하나, 투명해진
  visibility: visible;
}
```

화면 넘침 표시방법
overflow
```
div {
  overflow: hidden; 넘침 컨텐츠를 전부 숨김
  overflow: visible;  영역 밖으로 콘텐츠가 나간 모습이 보인다
  overflow: auto; 컨텐츠가 넘칠 경우, 스크롤바가 생성됨
  overflow: scroll; 컨텐츠가 넘치지 않아도 항상 스크롤바가 있다
}
```
_높이가 고정되어 있고, 내용이 길면 스크롤 할 부분에서는 auto를 사용한다_

---
**CSS float 속성**
CSS로 요소를 띄운다

float 속성
float: left
float: right

```
p {
  clear: both;
  clear: left;
  clear: right;
}
```
float 요소와 겹치는 경우, float속성을 해제한다

---
CSS float 레이아웃
```
HTML
<div class="float-frame">
  <div class="float-unit">A</div>
  <div class="folat-unit">B</div>
  <div class="folat-unit">C</div>
  <div class="folat-unit">D</div>
</div>

CSS
.float-frame {
  width: 300px;
  background-color: #eee;
  border: 1px solid #ddd;
  padding: 10px;
}

.float-unit {
  width: 50px;
  background: #333;
  color: #fff;
  font-size: 18px;
  font-weight: bold;
  text-align: center;
  padding: 15px 0;
  margin-right: 5px;
}

span 요소에 float: left 적용
.float-unit {
  width: 50px;
  background: #333;
  color: #fff;
  font-size: 18px;
  font-weight: bold;
  text-align: center;
  padding: 15px 0;
  margin-right: 5px;
  float: left;
}

부모 요소에도 float적용하여 height인식 문제를 해결(방법은 여러가지임)
.float-frame {
width: 300px;
background-color: #eee;
border: 1px solid #ddd;
padding: 10px;
float: left;
}
```

---
**CSS 포지션**
_CSS의 요소의 위치를 설정한다_

**position**
static  기본값
relative  static과 같은 순서로 배치되지만, top, right, bottom, left 소성을 이용해 위치를 지정 가능
fixed 브라우저 창 기준으로 위치
absolute  자신과 가장 가까운 'position'이 static이 아닌 부모를 기준(없을 경우 본문(body)기준
_relative_
```
relative 포지션은 자신의 위치를 기준으로 정렬된다
html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>position</title>
  <link rel="stylesheet" href="css/position1.css">
</head>
<body>
  <div class="relative1">relative1
    <div class="relative2">relative2</div>
  </div>
</body>
</html>

css
div {
  width: 400;
  height: 200px;
  padding: 10px;
  border: : 1px solid red;
  color: yellow;
  background-color: rgba(50,50,50,0.8);
}
.relative1 {
  position: relative;
}
.relative2 {
  position: relative;
  top: 30px;
  left: 20px;
}

```

_fixed_
뷰포트(표시영역)를 기준으로 정렬
```
html
<div class="fixed">fixed element</div>
css
.fixed {
  position: fixed;
  width: 100px;
  height: 50px;
  background-color: blue;
  right: 10px;
  bottom: 10px;
}
```
_absolute_
static이 아닌 가장 가까운 부모를 기준으로 정렬됨
```
html
<div class="fixed">fixed element</div>

css
.absolute {
  position: absolute;
  width: 150px;
  height: 60px;
  background-color: red;
  color: white;
  right: 0;
  bottom: 80px;
}
```
---
**CSS 가운데 정렬**
_가로 가운데 정렬_
width: 500px;
margin: 0 auto;
전체 크기가 정해져 있지 않을 경우, 내용의 width만 지정한 후 좌/우 여백은 auto로 같게 처리해준다

_가로/세로 가운데 정렬_
