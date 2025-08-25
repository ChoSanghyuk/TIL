

# CSS 실습 정리



## 클래스 이름 지정 (BEM)

- 블록 : 독립적으로 있을 수 있는 태그
  - 해당 태그를 클래스 이름으로 지정
- 요소 : 종속되어야 의미를 가지는 태그
  - `종속태그__요소태크` 형태로 지정
- modifier : 보여지는 상태에 대한 설정 하고 싶을 때
  - `기존클래스이름--상태설명` 형태로 지정



## 배치

### box-sizing

```css
box-sizing: content-box ;
```

- 기본 값
- content에 사이즈 조절 맞춤

```css
box-sizing: border-box ;
```

- box 전체에 사이즈 조절 맞춤 (패딩포함)



### display

```css
display: inline;
```

- 블록 요소도 inline 처럼 만듦
- 여러개의 블록 요소를 한줄에 배치할 때



## Icon 

- Bootstrap incon & Font Awesome 사용

### Bootstrap icon 사용법

- bootstrap icon 가서 원하는 icon 클릭
- 옆에  Icon font 복사 => html에 붙



## Favicon

- 인터넷 웹 브라우저의 주소창에 표시되는 웹 사이트를 대표하는 아이콘
- 일반적으로 32px * 32px
- `Favicon Generator` 사용





## Media-Query



### 너비에 따른 조정

```css
@media (min-width: 700px) {
    h2 { color: red;}
}
```

- 최소한 너비가 700px 일 때, 지정 부분의 출력 결과가 빨간색

```css
@media (width: 600px) {
    h2 { color: blue;}
}
```

- 너비가 600px 일 때, 지정 부분의 출력 결과가 파란색



### 가로 세로 비율에 따른 조정

```css
@media (orientation: landscape){
    p.orientation::after{ content: '가로입니다.' ;}
}
```

- 가로 비율이 더 클 때, p.orientation 클래스 뒤에 '가로입니다.' 추가



```css
@media (orientation: portrait){
    p.orientation::after{ content: '세로입니다.' ;}
}
```

- 세로 비율이 더 클 때, p.orientation 클래스 뒤에 '세로입니다.' 추가



### 출력 지정

```css
@media print {
    * {color : black;}
}
```





## Selector

### nth-child(n) vs nth-of-type(n)

```css
#ssafy > p:nth-child(2){
    color:red;
}
```
- ssafy id의 2번째 자식이 p 요소 ~> red 로 

```css
#ssafy > p:nth-of-type(2){
    color:blue;
}
```

- ssafy id의 자식 중 2번째 p요소 ~> blue로



### 속성 셀렉터

```css
/* a 태그에 target 속성 있음 */
a[target] { color: red; }

/* a 태그에 target 속성 값이 _blank */
a[target="_blank"] { color: red; }
```



### 사용자 동작 가상 클래스 셀렉터

```css
div:hover {
  background: #e3e3e3;
}
```

- 마우스가 올라오면 적용



## Position

- fixed는 block 요소여도 가로가 content 길이로 제한함









