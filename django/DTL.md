# DTL language



## {{ }}

- 변수 출력
- render에 담겨오는 context의 key 값을 입력하면 value 출력함



## {{ 변수|filter }}

- 변수를 수정하기 위함
- filter 종류
  - lower : 변수를 모두 소문자로 출력
  - truncatewords:30  : 30개에서 자름

## {% %}

- 논리연산

- for 문

  ```html
  {% for number in lotto %}
  <li>{{ number}}</li>
  {% endfor %}
  ```

  - endfor을 명시해줘야 끝이남



## {# #}

- 주석

