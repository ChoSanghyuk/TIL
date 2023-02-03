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



### Multi Thread

- 시분할 방식
  - **하나의 프로세서는 한번에 스레드 1개만 실행 가능**
  - 여러 쓰레드를 번갈아 처리 => 병렬 처리와 같은 느낌을 줌
- 독립적인 프로세스에 비해, 스레드는 자원과 데이터를 공유
  - 데이터 전송에 있어 이점
- 적용 조건
  - 병행성 (concurrency) : 다수 스레드 생성 방법 O
  - 동기화 (synchronization) : 작업에 방해 X, 각 스레드 동기화 방법 O
  - 통신 (communication) : 서로 다른 스레드가 정보 교환 방법 O




| 용어 | 멀티 프로세싱                                | 멀티 태스킹                             | 멀티 스레드                               |
| ---- | -------------------------------------------- | --------------------------------------- | ----------------------------------------- |
| 관점 | 시스템 관점                                  | 프로그램 외부 과점                      | 프로그램 내부 관점                        |
| 의미 | CPU 여러개에서 동시에 여러개의 프로세스 수행 | CPU 1개에서 동시에 여러 프로그램을 실행 | processor 1개가 동시에 여러 스레드를 실행 |



## 사용



### 생성

- 생성자
  - Thread()
  - Thread(Runnable target)

- Thread 클래스 상속받아 생성

  ```java
  public class Test01 extends Thread{
      @Override
      public void run(){
          // 실행 코드
      }
  }
  ```

- `Runnable 인터페이스 구현하여 생성`

  ```java
  public class Test02 implements Runnable{
      @Override
      public void run(){
          // 실행 코드
      }
  }
  ```

  - 이점
    - Functional Interface  => lambda 식으로 생성 O
    - 다른 클래스 상속에 있어서 이점



### 재사용불가

- Thread instance는 재사용 X

  - ```java
    new Thread(){
        public void run(){
            //...
        }
    }.start();
    ```

- Thread start 시, null이 아니라면 IllegalThreadStateException 



### run vs start

- start
  - Thread를 새로 생성하여 start.
  - Thread start 시 run() 호출됨
- run
  - 단순히 run 메서드만 실행
  - 싱글쓰레드 환경에서 동작



### 쓰레드 이름 설정

- 생성 시
  - `Thread(String name)`
  - `Thread(Runnable target, String name)`
- `setName(String name)` 메소드 활용



### 실행순서


- 기본적으로 thread의 실행 순서 보장 X

- 우선순위 설정 O

  

### Interrupt & Join


- interrupt
  - 다른 쓰레드의 작업에 개입
  - 어떠한 스레드에게, 지금 그 스레드가 하고 있는 일을 멈추고 다른 일을 하라고 명령
  - 주로 스레드 작업 중지 명령
- join
  - 하나의 스레드가 다른 스레드의 완료를 기다림
  - 현재 스레드(A)에서 타 스레드(B)를 기다리고자 한다면, `B.join();`
  - B.join( 5000)과 같이 최대 대기 시간 설정 O



## Synchronization



### 동기화

- 베타적 실행
  - 락
- 스레드간 안정적 통신
  - 동기화없이는 한 스레드가 만든 온전한 변화를 다른 스레드에서 확인 X



### 원자적 연산

- 중단이 불가능한 연산
  - 하나의 연산을 위해 바이트 연산을 할 때, 다른 스레드가 끼어들어 연산의 결과가 올바르지 않다면, 원자적 연산 X
- primitive 타입 수정은 기본적으로 원자적 연산
  - 예외
    - long, double
    - var++


:bulb: JVM 실행 비트수는 32비트. 64비트인 long과 double은 원자적 데이터 X => volatile 필요

:bulb:원자적 데이터라도 한 스레드의 수정이 완전히 반영된 값을 얻는다고 보장하지 않음 => 동기화 필요



### synchronized

- 고유 락 (Intrinsic Lock)

  - 자바의 모든 객체가 소유
  - Primitive Type은 없음 (long, double 제외)

- 메소드 적용 

  ```java
  public synchronized void doSth(){
      //
  }
  ```

  - 한 스레드가 해당 메소드를 종료하기 전까지 다른 스레드는 사용 X
  - 메소드 level의 Synchronized는 한 Class 내에 정의된 메소드들에 동일하게 적용됨
  - constructor에는 사용 X

- 변수 적용

  ```java
  synchronized(var1){
      //
  }
  ```

  

- Thread safe
  - critical point가 모두 synchronized
  - synchronized 수는 absolutely minimun으로 유지해야 성능 up





### wait, notify, notifyAll

- wait()
  - lock을 소유한 thread가 자신의 제어권을 양보 => WAITING or TIMED_WAITING 상태에서 대기
  - lock을 소유하고 있어야만 실행 O
- notify()
  - WAIT SET에서 대기중인 다른 한 개의 Thread 깨움
  - 깨어난 Thread는 다시 Runnable 상태로 변경되어 실행
- nofifyAll()
  - WAIT SET에서 대기중인 모든 Thread 깨움

:bulb: 스레드 메소드 X, Object의 메소드 O



## Lock



### ReentrantLock

- ReentrantLock 변수를 통해서 동기화 구현 가능

- 사용

  ```java
  Reentrantlock reentrantlock = new Reentrantlock();
  reentrantlock.lock();
  try{
    	// do something
  } finally{
      reentrantlock.unlock();
  }
  ```

- 장점 (vs Synchronized)

  - 코드가 단일 블록 형태를 넘어가는 경우에도 사용 O
  - Lock Polling 지원
  - Condition을 적용해 선별적인 스레드 wake가능
  - 타임아웃 지정 O























