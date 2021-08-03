

# CSS 실습 정리



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

## Selector

### nth-child(n) vs nth-of-type(n)

```css
#ssafy > p:nth-child(2){
    color:red;
}
```
- ssafy 클래스의 2번째 자식이 p 요소 ~> red 로 

```css
#ssafy > p:nth-of-type(2){
    color:blue;
}
```

- ssafy 클래스의 자식 중 2번째 p요소 ~> blue로



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
