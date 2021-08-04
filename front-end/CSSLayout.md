# CSS Layout 이론



## 목차

- float
- flexbox
- bootstrap
- bootstrap grid system





## float



### float 이란

- 한 요소가 정항 흐름으로부터 빠져 **텍스트** 및 **인라인 요소**가 그 주위를 감싸, 요소의 좌우를 따라 배치

- 블록 요소는 float 요소 아래쪽으로 들어가게 됨 (겹침)

  => float clear 필요

- 기존 이미지 배치 사용 -> 레이아웃 기술에 적용 -> 새로운 Layout 기술 등장 -> 다시 이미지 용으로 주로 사용



### float 속성

- none : 기본값
- left : 요소를 왼쪽으로
- right : 요소를 오른쪽으로



### float clear

- float 했을 때, 아래에 있는 블록 요소들의 레이아웃이 깨지지 않게 가상의 벽을 설정함

- 방법

  - float 요소의 부모 요소 생성

  - 부모 요소의 클래스 지정 (보통 `clearfix`)

  - `clearfix` 클래스 설정

    ``` css
    .clearfix::after{
        content:"" ;
        display: block;
        clear: both;
    }
    ```

    - ::after  : clearfix 클래스 적용 객체 뒤에 가상 요소를 만든다는 뜻
    - 기본값은 inline -> block으로 변경해줌
    - clear : float 된 걸 무시하겠다. (left, right 보다 both 사용)



## Flexbox



### CSS Flexible Box Layout

- 요소 간 **공간 배분**과 **정렬** 기능을 위한 1차원(단방향) 레이아웃

- `요소` 
  - Flex Container (부모 요소)
  - Flex Item (자식 요소)
- 축
  - main axix (메인축)
  - cross axis (교차축)



### Flexbox 구성 요소

- Flex Container (부모 요소)
  - Flexbox 레아아웃을 형성하는 가장 기본적 모델
  - Flex Item들이 놓여있는 영역
  - display : flex (or inline-flex)
    - flex : 부모의 전체 영역만큼 container
    - inline-flex : 자식 요소들 만큼 container
- Flex Item (자식 요소)
  - 컨테이너의 컨텐츠



### Flex에 적용하는 속성



@ justify& align

- justify : 메인축
- align  : 교차축

#### @ content & items & self

- content : 여러 줄
- items : 한 줄
- self : flex item 개별 요소



- 배치 방향 설정
  - flex-direction : 메인축의 방향 설정
    - row (default) : **→**
    - row-reverse : **←**
    - column : **↓**
    - column-reverse :  **↑**
- 메인축 방향 정렬
  - justify-content
    - flex-start : 메인축의 시작쪽 정렬
    - center : 메인축 가운데 정렬
    - flex-end : 메인축의 끝 정렬
    - space-between : 아이템간 간격 동일하게 좌우 정렬
    - space-around : 외부 요소 간격보다 내부 요소 간격이 2배
    - space-evenly : 균등 좌우 정렬
  - justify-items , justify-self 는 필요 없음
    - 개별 요소에 margin을 주는 방식으로 개별 요소 배치 가능
- 교차축 방향 정렬
  - align-items
    - stretch (default) : 교차축 크기만큼 요소 늘어남
    - flex-start : 교차축 시작쪽 정렬
    - center : 교차축 가운데 정렬
    - flex-end : 교차축 끝쪽 정렬
    - baseline : 컨테이너의 시작 위치에 정렬
  - align-self
    - 다른 속성들과 다르게 self 속성은 개별 요소에 적용시킴 (container x)
    - flex-start
    - center
    - flex-end
  - align-content
- 기타
  - flex-wrap : 요소를 강제로 한줄로 배치할 지 
    - nowrap : 넘쳐도 한 줄로
    - wrap : 넘치면 아래 칸
    - wrap-reverse : 넘치면 위칸
  - flex-flow : flex-direction & flex-wrap의 shorthand
    - column wrap : column 방항으로  wrap 적용
  - flex-grow
    - 개별 요소에 적용시킴 (container x)
    - 메인축에서 **남은 공간**을 각 항목에게 분배 (숫자별 비율로)
  - order
    - 개별 요소에 적용시킴 (container x)
    - 정렬 순서를 숫자로 지정 (오름차순 정렬)
    - 기본값은 0 (즉, `order : 1 ` 해주면 맨 끝으로 이동)



## Bootstrap



### 개요 : The most popular HTML, CSS, and JS libaray in the world

- 오픈 소스 프론트엔드 라이브러리

- 웹 페이제에서 많이 쓰이는 요소 거의 전부를 내장

- 별도 디자인 시간 줄어듦 + 여러 웹 브라우저 지원하기 위한 크로스 브라우징 지원

  : One source multi use ( `반응형 웹 디자인` )

#### @ user agent stylesheet

- 각 브라우저 마다 고유의 html 기본 스타일링 코드가 있음 -> 모든 브라우저에서 `동등한` 경험 제공하려면 제거 필요
- 제거 방법
  - Normalize CSS
    - gentle solution
    - W3C 표준을 기준으로, 브라우저 하나가 불일치 한다면, 나머지 브라우즈들을 downgrade해서 맞춤
  - Rest CSS
    - 브라우저의 기본 스타일이 전혀 필요 없다는 방식
    - reset 과정이 복잡하여 디버깅 시 힘듦



### CDN

- Content Delivery Network
- 컨텐츠를 효율적으로 전달하기 위해, 서버와 사용자 사이의 물리적 거리를 줄여 컨텐츠 로드 지연을 최소화
- 분산 된 서버로 이루어진 플랫폼 => 전 세계 사용자들의 로딩 시간 낮춤 + 동일 품질 
- 장점
  - 사용자와 가까운 서버를 통해 빠르게 전달
  - `외부 서버` 활용 -> 본인 서버 부하 적어짐



## bootstrap grid system



### Grid system

- Bootstrap Grid system은 flexbox로 제작
- `container`, `rows`, `column` 으로 컨텐츠를 배치하고 정렬
- 중요!!
  - 12개의 column
  - 6개의 grid breakpoints



## Grid system class



### 주요 클래스

- row
  - column의 wrapper
- col, col-*
  - row당 가능한 12개 중 사용하려는 columns 수를 나타냄
  - 너비는 백분율로 설정 => 부모 요소를 기준으로 유동적으로 조정
  - 내용은 반드시 columns 안에 있어야 하며, row의 하위 자식은 col 밖에 없음
  - 줄을 나누고 싶은 col 클래스 사이에 `<div class="w-100"></div>` 넣어줌
- gutters
  - grid 시스템에서 반응적으로 공간을 확보하고 컨텐츠를 정렬하는데 사용되는 column 사이의 padding
  - ex) `gx-5` `g-0`
- breakpoints
  - 반응형 웹 디자인 ( 웹의 크기에 따른 디자인 설정)
  - 6개의 point : xs, sm, md, lg, xl, xxl (xs는 사용 x )
  - ex) `col-md-4`

- offset
  - 지정한 만큼의 column 공간을 무시하고 다음 공간부터 컨텐츠 적용
  - ex) `offset-4` ,  `offset-md-4`
- Nesting
  - row -> col-* -> row -> col-* 의 방식으로 중첩 사용 가능



### Grid breakpoints

- 다양한 디바이스에서 적용하기 위해 특정 px 조건에 대한 지점을 설정
- viewport 너비가 픽셀 단위이고, 크기에 따라 폰트가 변하지 않기에 px 기준





### 코드



#### row나누기

- 줄을 나누고 싶은 col 클래스 사이에 넣어줌

```css
<div class="w-100"></div>
```

