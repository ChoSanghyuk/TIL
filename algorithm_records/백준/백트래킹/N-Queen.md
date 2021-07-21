# N-Queen 문제 풀이



## 문제 개요

N × N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓는 문제이다.

N이 주어졌을 때, 퀸을 놓는 방법의 수를 구하는 프로그램을 작성하시오.



## 예제

`input`

8

`output`

92



## 풀이

 기본적으로 모든 경우의 수를 탐색하는 `DFS`(깊이 우선 탐색) 방식을 사용한다. 다만, 이 문제의 경우 탐색 중간부터 `목표 지점으로의 도달` 이 불가능한 경우가 생기기 때문에, 가능성이 없는 노드의 경우 탐색을 중단하는 알고리즘을 작성해야 한다. 

 필요한 핵심 기능으로는 

1. 지나온 노드의 기록 및 다음 노드의 탐색
2. 마지막 노드 도달 시, 카운트 증가
3. 해당 노드의 가능성 판단

로 둘 수 있다.  해당 기능들을 구현하기 위해 다음과 같은 코드를 작성하였다.



0. input 받아오기 및 메소드 실행
````java
import java.util.Scanner ;

public class Main{
    static int N;
    static int ans;
    public static void main(String args[]){
        
        Scanner scanner = new Scanner(System.in);
        N = scanner.nextInt();
        scanner.close();
        
        for(int i = 0 ; i < N ; i++){
            int[] records = new int[N];
            dfs(0, i , records);
        }
        System.out.println(ans);
        
    }
````
1. 지나온 노드의 기록 및 다음 노드의 탐색
	가능성이 있는 노드로 판명 되었을 시, `records`에 기록을 남기고 다음 노드로 이동
2. 마지막 노드 도달 시, 카운트 증가
	마지막 노드 도달 시에는 static 변수인 `ans` 값을 하나 올림으로, 경우의 수를 카운트 함
````java    
    public static void dfs(int depth , int idx , int[] records){
        
    	
        if(isValid(depth, idx, records)){
            if(depth == N-1) {
                ans++;
                return;
            }
            records[depth] = idx;
            for(int i = 0 ; i < N ; i++){
                dfs(depth+1 , i , records);
            }
            
        } else{
        	records[depth] = -1; 
        }
        
        
    }
````
3. 해당 노드의 가능성 판단
	현재의 위치가 기록된 퀸들과 행이 같거나 대각선 상에 위치할 시 가능성이 없는 노드로 판단하였다.
````java
    public static boolean isValid(int depth , int idx , int[] records){
        for(int i = 0 ; i < depth ; i++){
            if(idx == records[i]) {
            	return false;
            }
            if(depth - i == idx - records[i] || depth - i == records[i] - idx) return false;
        }
        return true;
    }
}
````

## 주의 할 점

`records` 의 타입인 Array는 mutable 하기 때문에, 한 노드에서 records를 수정할 경우, 모든 노드에 영향 줌. 이 문제의 경우에는 노드의 깊이(행)에 따라 records를 기록하고, 현재 노드의 depth까지만 records에 접근 함으로써 해당 문제에서 자유로울 수 있었음.



## 전체 코드

```java
import java.util.Scanner ;

public class Main{
    static int N;
    static int ans;
    public static void main(String args[]){
        
        Scanner scanner = new Scanner(System.in);
        N = scanner.nextInt();
        scanner.close();
        
        for(int i = 0 ; i < N ; i++){
            int[] records = new int[N];
//            records[0] = i ;
            dfs(0, i , records);
        }
        System.out.println(ans);
        
    }
    
    public static void dfs(int depth , int idx , int[] records){
        
    	
        if(isValid(depth, idx, records)){
            if(depth == N-1) {
                ans++;
                return;
            }
            records[depth] = idx;
            for(int i = 0 ; i < N ; i++){
                dfs(depth+1 , i , records);
            }
            
        } else{
        	records[depth] = -1; 
        }
        
        
    }
    public static boolean isValid(int depth , int idx , int[] records){
        for(int i = 0 ; i < depth ; i++){
            if(idx == records[i]) {
            	return false;
            }
            if(depth - i == idx - records[i] || depth - i == records[i] - idx) return false;
        }
        return true;
    }
}
```

