# Generics



## 개념

### 역할

- 다양한 타입의 객체를 다루는 메소드, 컬렉션 메서드에서 컴파일 시에 타입 체크

  => 미리 사용할 타입을 명시해서 형 변환 작업 x

  => 객체의 타입에 대한 안전성 향상 및 형 변환의 번거로움 감소

### 표현

- 클래스 또는 인터페이스 선언 시 <>에 타입 파라미터 표시
  - Class_Name : Raw Type
  - Class_Name<T> : Generic Type
- 타입 파라미터
  - 특별한 의미의 알파벳 보다는 단순히 임의의 참조형 타입을 말함
  - T : reference Type, E : Element, K : Key, V : Value
- 객체 생성
  - 변수 쪽과 생성 쪽의 타입은 반드시 같아야 함

### type parameter 제한

- 필요에 따라 구체적인 타입 제한 필요
  - type parameter 선언 뒤 extends와 함께 상위 타입 명시
  - `<T extends Number>`
  - 클래스와 함께 인터페이스 제약 조건을 이용할 경우 &로 연결
  
    `Test<T extends Class1 & Interface1 & Interface2> ` 

### Comparable 비교

```java
public class Test<T extends Class1> implements Comparable<Test<T>> {	
```

- Comparable의 타입 지정에서도 T 타입의 Test 클래스 지정하여, 같은 Generice Type의 클래스끼리만 비교
- `compareTo 메서드 Override`

### Generic Method

- 파라미터와 리턴타입으로 type parameter을 갖는 메소드

  - 매서드 리턴 타입 앞에 타입 파라미터 변수 선언

```java
public class class1<T>{
    public <P> void method1(P p){
    ~~
    }
    public <P> P method1(P p){
    ~~
    }
}
```

- T와 P는 엄연히 다름
- T는 클래스 객체 생성 시점에서 결정
- P는 메서드 호출 시점에서 결정



## 실습



### 요구사항

```java
        // Create a generic class to implement a league table for a sport.
        // The class should allow teams to be added to the list, and store
        // a list of teams that belong to the league.
        //
        // Your class should have a method to print out the teams in order,
        // with the team at the top of the league printed first.
        //
        // Only teams of the same type should be added to any particular
        // instance of the league class - the program should fail to compile
        // if an attempt is made to add an incompatible team.
```



### League.java

```java
package generics.league;

import generics.sports.Sports;
import generics.team.Team;

import java.util.Collections;
import java.util.List;

public class League <T extends Sports> {

    List<Team<T>> teams;

    public void add(Team<T> team){
        teams.add(team);
    }

    public void printTeamsInOrder(){
        Collections.sort(teams);
        for(Team<T> team : teams){
            System.out.println(team.getName());
        }

    }
}
```



### Team.java

```java
package generics.team;

import generics.sports.Sports;

public class Team<T extends Sports> implements Comparable<Team<T>> {

    String name;
    int point;

    public Team(int n, int point){
        this.name = "Team" + n;
        this.point = point;
    }

    public int getPoint() {
        return point;
    }

    public String getName(){
        return this.name;
    }

    public void setPoint(int point) {
        this.point = point;
    }

    @Override
    public int compareTo(Team<T> o) {
        return Integer.compare(this.point, o.getPoint());
    }

}
```



### Soccer.java

```java
package generics.sports;

import generics.team.Team;

public class Soccer extends Sports {

}

```



### Baseball.java

```java
package generics.sports;

import generics.team.Team;

public class Baseball extends Sports {
}

```



### GenericExec.java

```java
package generics;

import generics.league.League;
import generics.sports.Baseball;
import generics.sports.Soccer;
import generics.team.Team;

public class GenericExec {

    public static void main(String[] ars){

        Team<Soccer> soccer1 = new Team<>(1,10);
        Team<Soccer> soccer2 = new Team<>(2,20);
        Team<Soccer> soccer3 = new Team<>(3,30);
        Team<Soccer> soccer4 = new Team<>(4,40);

        Team<Baseball> baseball1 = new Team<>(1,10);
        Team<Baseball> baseball2 = new Team<>(2,20);
        Team<Baseball> baseball3 = new Team<>(3,30);
        Team<Baseball> baseball4 = new Team<>(4,40);

        League<Soccer> soccerLeague = new League<>();
        League<Baseball> baseballLeague = new League<>();

        soccerLeague.add(soccer4);
        soccerLeague.add(soccer2);
        soccerLeague.add(soccer3);
        soccerLeague.add(soccer1);
//         soccerLeague.add(baseball4);  실행 x

        baseballLeague.add(baseball1);
        baseballLeague.add(baseball3);
        baseballLeague.add(baseball2);
        baseballLeague.add(baseball4);
        // baseballLeague.add(soccer1);  실행 x


    }
}

```







