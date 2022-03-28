# HTML



## 목차

- 사전 작업
- HTML 기본구조
- HTML 문서 구조화



## 사전 작업

### HTML, CSS 검색

​	: 검색 keyword와 함께 `MDN` 넣어서 검색



### Visiual Studio Code 확장 프로그램

- Open in browser
- Auto rename tag
- Highlight Matching Tag



## HTML 기본구조

### 정의

- Hypter Text Markup Language
- Hyper
  - 텍스트 등의 정보가 동일 선상에 아니라 다중으로 연결되어 있는 상태
- Hyper Text
  - 참조(hyper link)를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
  - Hyper text가 쓰인 기술들 중 가장 중요한 2가지 : http / html
- Markup
  - 특정 텍스트에 역할을 부여 (제목, 본문에 역할 마킹)
  - 태그 등을 이용하여 문서와 데이터의 구조를 명시
  - 단순하게 데이터를 표현하기만 함

**=> 웹 컨텐츠의 `의미`와 `구조` 정의**



## 구조

### html 구조

- HTML 문서의 최상위 요소(문서의 root)
- head와 body로 구분
- head
  - 문서 제목, 인코딩 등 문서 정보를 담고있음
  - 브라우저에 나타나지 x
  - ex) Open Graph Protocol : HTML 문서의 메타 데이터를 통해 문서의 정보 전달
- body
  - 브라우저 화면에 나타나는 정보



### DOM(Document Object Model) 트리

- 구조화된 표현을 제공 -> 문서 구조, 스타일 내용 등을 변경 할 수 있게 도움
- 부모 관계, 형제 관계
- Web page의 객체 지향 표현



### 요소

- 구성 : 시작 태그 & 내용 & 종료 태그
  - 태그 : 내용(컨텐츠)을 감싸는 것으로 그 정보의 성격과 의미를 정의
- 내용 x 태그
  - br , hr, img, input, link, meta
- 요소는 중첩될 수 있음
  - 요소의 중첩을 통해 하나의 문서를 구조화
  - 여는 태그와 닫는 태그의 쌍을 확인
  - 오류 반환 x (단순히 레이아웃이 깨짐)



### 속성

- 속성을 통해 태그의 부가적인 정보를 설정
- 이름과 값이 하나의 쌍으로 존재 : 태그이름="{값}"
  - 띄어쓰기 X
  - " " 사용
- 태그별로 사용할 수 있는 속성이 다름
- 태그와 상관없이 사용 O 한 속성도 있음



### 시맨틱 태그

- 의미론적 요소를 담은 태그 => 코드의 가독성 높임 & 유지보수 쉬워짐
- 기존의 단순한 데이터의 집합 ~> `의미`와 `관련성`을 가지는 데이터베이스로 구축
- 종류
  - **header** : 머릿말 부분
  - **nav**  : 내비게이션
  - **aside**: 사이드에 위치한 공간. 메인 콘텐츠와 관련성 적음
  - **section** : 문서의 일반적인 부분. 콘텐츠의 그룹
  - **article** : 문서, 페이지, 사이트 안에서 독립적으로 구분
  - **footer** : 문서 전체나 섹션의 푸터
- 논시맨틱
  - div
  - span



## HTML 문서 구조화



### 인라인 / 블록 요소

- 인라인 요소
  - 해당 요소의 길이만큼의 공간만 차지함
  - 세로 x -> 넓이 x
- 블록 요소
  - 공간을 차지함
  - 넓이 o



### 그룹 컨텐츠

- `<p>`
- `<hr>`
- `<ol>`, `<ul>`
- `<pre>`, `<blockquote>`
- `<div>` 



### 텍스트 관련 요소

- `<a>`  : 페이지 이동
- `<b> </b>`  vs `<strong> </strong>` vs `<em> </em>`
  - b : 표현상 굵게
  - em : 강조 (이탤릭체로)
  - strong : 강한 강조 (볼드)
- `<i>` vs `<em>`
  - i : 이탤릭체로

- `<span>`, `<br>`, `<img>`
  - span : 인라인 요소
  - div : 블록 요소

- `<br> </br>` : 줄바꿈



### 이미지

- `<img src='~~'> ` 
- width , height로 크기 조절



### table

- 테이블 태그
  - `<table></table>` 
  - `<tr> </tr>`
  - `<th> </th>` : 제목 셀
  - `<td> </td>`  : 데이터 셀
- 속성
  - width
  - height
  - border
  - cellspacing
    - 셀 사이 간격
    - 픽셀 혹은 %로 지정
  - cellpadding
    - 셀 안의 간격
  - rowspan
    - 행 통합
  - colspan
    - 열 통합



### form

- `<form>` : 서버에서 처리될 데이터를 제공
- 기본 속성
  - name
    - form 태그에 이름 부여
  - action : 어디로
  - method



### input

- <u>다양한 타입</u>을 가지는 데이터입력 데이터 필드
- form 태그 안에 소속
- `<label>` : 서식 입력 요소의 캡션
- `<input>` 공통 속성
  - name, placeholder
  - required
  - autofocust
- input 컨트롤 종류
  - button
  - checkbox
  - file
  - hidden
  - image
  - password
  - radio
  - reset
  - submit
  - text


