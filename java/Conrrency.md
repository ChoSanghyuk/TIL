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
  - method를 소유한 객체를 `mutex`로 사용  (Mutual Exclusion)
    - `syncronized(this) { ... }` 처럼 동작됨
    - 해당 객체의 syncronized된 다른 메소드에 대해서도 lock이 생성
    - `it acquires a lock on the object that the method is a part of. This lock ensures that only one thread can execute the synchronized method of that object at a time` 
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

- try-finally 구문으로 unlock 보장

- Fairness

  - `Reentrantlock reentrantlock = new Reentrantlock(true);` 
  - 생성자에서 `true` 지정 시, fairness 보장
  - 가장 오래 기다린 thread에게 lock 배정

- methods

  - `public boolean tryLock()`
    - lock을 사용할 수 있다면, lock을 가지면서 true 반환
    - fairness와 상관없이 lock이 이용가능하다면 쟁취

  - `public boolean tryLock(long timeout, TimeUnit unit) throws InterruptedException`
    - 일정 시간 동안 lock을 사용할 수 있다면, lock을 가지면서 true 반환
    - fairness 지킴

  - `public final int getQueueLength()`
    - 해당 락을 차지하기 위해 기다리는 thread 수 반환



### ArrayBlockingQueue

- 생성자

  `ArrayBlockingQueue(int capacity)`

  - capacity를 넘어서는 `put` 수행 시 block처리됨

- methods

  - `boolean add(E e)`
    - 성공 시, `true` 반환. Full 일 경우, `IllegalStateException` 발생
  - `void put(E e)`
    - Full 일 경우, 대기
  - `E peek()`
    - Retrieves but not remove. empty시 `null` 반환

  - `E take()`
    - Retrieves and remove. 

- 특징

  - thread-safe
  - :bulb: thread-safe임에도, 구간을 lock을 잡기 위해서는 `synchronized` 필요


## Thread Pool



### Thread Pool

- 작업 처리에 사용되는 thread 수를 제한된 개수만큼 정해두고, 작업 큐에 들어오는 작업들을 하나씩 thread가 맡아서 처리

  ```java
  ExecutorService executorService = Executors.newFixedThreadPool(3);
  
  executorService.execute(runnable1);
  executorService.execute(runnable2);
  executorService.execute(runnable3);
  
  executorService.shutdown()
  ```

- methods

  - `public void execute(Runnable r)`
    - thread 생성하여 작업 실행.
    - 지정된 thread 수가 차있다면, 작업 대기
  - `public void shutdown()`
    - 동작중인 thread들의 처리가 모두 끝난 후 executorService 종료
  - `public void shutdownnow()`
    - 동작중과 상관없이 executorService 종료



### Future



### Dead Lock

- two or more threads are blocked indefinitely, waiting for each other to release a resource
- each thread holds a resource that the other thread wants to access
- both threads wait for each other to release the resource they are holding, creating a cycle of waiting



### Starvation





















