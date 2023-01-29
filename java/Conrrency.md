# Concurrency



## 개념 정의



### Process vs Thread

- Process
  - a unit of execution that has its own memory space (heap)
  - Each instance of a JVM runs as a process (Not all but most)
  - Process don't share memory
- Thread
  - a unit of excecution within a process
  - Every thread in a process shares the process's memory
  - Thread Stack
    - only owner thread can access



- 기본적으로 thread의 실행 순서 보장 X

  - 우선순위 설정 O

- Thread instance는 재사용 X

  - ```java
    new Thread(){
        public void run(){
            //...
        }
    }.start();
    ```

- Runnable Interface
  - Functional Interface
    - lambda 식으로 생성 O
  - 다른 클래스 상속에 있어서 이점
- Thread 이름 설정
  - setName
- run vs start
  - run은 현재 실행 중인 스레드 실행 의미
  - start를 해야지 생성된 thread 실행 O















