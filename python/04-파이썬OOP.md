# 파이썬 OOP



## 객체



### 정의

- 객체는 특정 타입의 인스턴스(instance) 이다.

### 특징

- 타입(type) : 어떤 연산자와 조작이 가능한가
- 속성(attribute) : 어떤 상태(data)를 가지는가
- 조작(method) : 어떤 행위를 할 수 있는가



## 연산자

- `is` : 객체의 아이덴티티를 검사

  ```python
  type(10) is int  # True
  type(0) is bool  # False
  ```

- `isinstance(object , classinfo)`

  - classinfo의 instance거나 subclass인 경우 True.
  - classinfo가 tuple인 경우, 하나라도 일치하면 True

  ```python
  isinstance(10, int)   #True
  isinstance(10, (bool , int, complex)) # True
  ```
  

 @ `issubclass(class , classinfo)`

```python
issubclass(Professor , Person) 	# True
# 단, Professor 는 Person 클래스의 자식 클래스
```



## 객체지향 프로그래밍(OOP)

### 정의

- 객체 지향 프로그래밍은 컴퓨터 프로그래밍을 여러 개의 독립된 단위, 즉 `객체` 들의 모임으로 파악



### 구성

- 속성 : 특정 데이터 / 클래스의 객체들이 가지게 될 상태 / 데이터를 의미

- 메서드 : 특정 데이터 타입 / 클래스의 객체에 공통적으로 적용 가능한 행위

- self : 인스턴스 자기자신. 메서드 호출 시 첫번째 인자로 인스턴스 자신이 전달되도록 설계됨

- 생성자 : 인스턴스 객체가 생성될 때 호출되는 메서드

  ```python
  def __init__(self, name):
      print(f"인스턴스가 생성되었습니다. {name}")
  ```

  

- 소멸자 : 인스턴스 객체가 소멸될 때 호출되는 메서드

  ```python
  def __del__(self):
      print("인스턴스가 사라졌습니다.")
  ```

- 매직 매서드 : Double underscore(__)가 있는 메서드는 특수한 동작을 위해 만들어진 메서드

  ```python
  # 해당 객체의 출력 형태 지정
  def __str__(self) :
      return f"~~~"
  
  # 부등호 연산자 처리
  def __gt__(self , other):
      return self.~ > other.~
  ```

  

## 클래스와 인스턴스



### 인스턴스 변수

- 인스턴스의 속성
- 각 인스턴스의 고육한 변수 : self.<이름>

### 클래스 변수

- 모든 인스턴스가 공유
- 클래스 선언 내부에서 정의
- `<classname>.<name>`으로 접근 및 할당



### 인스턴스와 클래스 간의 이름 공간

- 클래스 정의 -> 클래스와 해당하는 이름 공간 생성
- 인스턴스 생성 -> 객체 생성되고 이름 공간 생성
- 인스턴스에서 특정 속성에 접근하면 , 인스턴스 -> 클래스 순으로 탐색



### 메서드의 종류

- 인스턴스 메서드
  - 호출 시, 첫번째 인자로 self가 전달됨
- 클래스 메서드
  - @classmethod 데코레이터를 사용하여 정의
  - 호출 시, 첫번째 인자로 cls(클래스)가 전달됨
- 스태틱 메서드
  - 클래스가 사용할 메서드
  - @staticmethod 데코레이터를 사용하여 정의
  - 호출 시, 어떠한 인자로 전달 x (클래스 정보에 접근/수정 불가)



## 상속

```python
class ChildClass(ParentClass):
    pass
```

- super() : 자식클래스에서 부모클래스 사용
- 메서드 오버라이딩 
  - 상속 받은 메서드 재정의
  - 부모 클래스의 메서드 실행시키고 싶은 경우 super 사용
