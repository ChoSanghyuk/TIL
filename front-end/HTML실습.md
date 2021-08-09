

# HTML 실습



## 강조

```html
<em>  </em>
```



## 링크

```html
<!-- Default. -->
<a href="https://ssafy.com">SSAFY</a>

<!-- 새탭에서 열기-->
<a href="https://ssafy.com" target=_blank>SSAFY</a>

<!-- # == placeholder -->
<a href="#">Nth</a>

<!--같은 폴더 안 파일 이동하기-->
<a href="02_text" >02_text</a>

<!--메일 보내기-->
<a href="mailto:ssafy@ssafy.com">MAIL</a>
```



## 목록

```html
<!-- ordered list -->
<ol>

</ol>

<!-- unordered list -->
<ul>

</ul>

<!-- list : ol 또는 ul 안에서 사용-->
<li>

</li>
```



## 테이블

```html
<!-- Table 속성-->

<!-- 열 병합 / 행 병합 -->
colspan="n"
rowspan="n"
```



## Form

```html
<form action="toWhere">
    <p>
        <label for="name">이름</label>
        <input name="name" id="name" placeholder="what's your name?" type = "text">
    </p>
    <p>
        <label for="age">나이</label>
        <input name="age" id="age" type = "number" required>
    </p>
    <p>
        <label for="major">전공</label>
        <input name="major" id="major" type = "text" value="CS">
    </p>
    <p>
        <label for="description">자기소개</label>
        <textarea name="description" id="description" cols="10" rows="2">
    </p>
    <input type = "submit">
</form>
```

- name : 데이터를 전송할 때 KEY 값
- required : 필수 입력
- placeholder : 실제 값이 아닌 설명처럼 미리 채워져 있는 글자 설정
- value : default 값 설정
