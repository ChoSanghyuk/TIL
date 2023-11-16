# SWEA 4874 forth 문제 풀이(파이썬)



## 문제 링크

https://swexpertacademy.com/main/learn/course/lectureProblemViewer.do



## 문제 풀이

후위표기법으로 된 식을 입력받아서 식을 계산하는 문제이다. 후위표기법의 과정을 스택을 통해 다음과 같은 순서로 구현하였다.

1. 숫자면 스택에 넣음
2. 사칙연산이면 숫자 두개를 꺼내서, 연산 후 다시 스택에 넣음
3. . 이면 종료하고 스택 맨 위의 값을 반환함

여기에 해당 후위표기법이 올바르지 않은 경우도 있기 때문에 다음 2가지의 오류 처리를 하였다.

- 사칙연산 시 스택에 2개 이상의 수가 들어있지 않음
- .으로 연산이 종료되었을 때, 스택에 1개보다 많은 수의 숫자가 들어있음.

```python
import sys

sys.stdin = open("input.txt")

T = int(input())

for t in range(1, T+1):
    postfix = input().split(' ')
    stack = []

    for s in postfix:
        # 숫자면 스택에 넣음
        if s.isdecimal():
            stack.append(s)
        # .이면 스택 맨위 반환. 단, 스택의 수가 2개 이상이면 오류
        elif s == '.':
            if len(stack) > 1:
                print(f"#{t} error")
            else:
                print(f"#{t} {stack.pop(-1)}")
            break
        # 사칙연산 기호일 경우 스택에서 두 개 뽑아서 연산 수행하고 결과 다시 스택에 넣음. 단 스택에 있는 개수가 2개 이하면 오류
        else:
            if len(stack) < 2:
                print(f"#{t} error")
                break
            b = int(stack.pop(-1))
            a = int(stack.pop(-1))
            if s == "+":
                stack.append(a + b)
            elif s == "*":
                stack.append(a * b)
            elif s == "/":
                stack.append(a // b)
            else:
                stack.append(a - b)

```

