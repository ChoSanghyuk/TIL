# 프로그래머스 여행경로 문제 풀이 (java)



## 문제 설명

주어진 항공권을 모두 이용하여 여행경로를 짜려고 합니다. 항상 "ICN" 공항에서 출발합니다.

항공권 정보가 담긴 2차원 배열 tickets가 매개변수로 주어질 때, 방문하는 공항 경로를 배열에 담아 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 모든 공항은 알파벳 대문자 3글자로 이루어집니다.
- 주어진 공항 수는 3개 이상 10,000개 이하입니다.
- tickets의 각 행 [a, b]는 a 공항에서 b 공항으로 가는 항공권이 있다는 의미입니다.
- 주어진 항공권은 모두 사용해야 합니다.
- 만일 가능한 경로가 2개 이상일 경우 알파벳 순서가 앞서는 경로를 return 합니다.
- 모든 도시를 방문할 수 없는 경우는 주어지지 않습니다.

##### 입출력 예

| tickets                                                      | return                                     |
| ------------------------------------------------------------ | ------------------------------------------ |
| [["ICN", "JFK"], ["HND", "IAD"], ["JFK", "HND"]]             | ["ICN", "JFK", "HND", "IAD"]               |
| [["ICN", "SFO"], ["ICN", "ATL"], ["SFO", "ATL"], ["ATL", "ICN"], ["ATL","SFO"]] | ["ICN", "ATL", "ICN", "SFO", "ATL", "SFO"] |



## 문제 풀이

```java
import java.util.Stack;
import java.util.Arrays;
import java.util.Comparator;


class Solution {
    // dfs에 사용할 스택과 티켓 사용 기록 배열을 생성
    static Stack<String> route;
    static boolean[] visited;
    public String[] solution(String[][] tickets) {
        
        // 알파벳 순서대로 탐색하기 위해 도착지 기준 정렬
        Arrays.sort(tickets, new Comparator<String[]>(){
            @Override
            public int compare(String[] o1, String[] o2){
                return o1[1].compareTo(o2[1]);
            }
        });
        visited = new boolean[tickets.length];
        
        route = new Stack<>();
        // 시작지는 인천
        route.push("ICN");
        // dfs 시작
        dfs(tickets);
        
        // 스택에서 하나씩 경로를 배열로 옮긴 후 반환
        String[] answer = new String[tickets.length +1];
        for(int i = answer.length-1 ; i>=0; i--){
            answer[i] = route.pop();
        }
        return answer;
    
    }
    // 탐색에 사용할 dfs 함수 구현
    public static boolean dfs(String[][] tickets){
        // 탐색 종료 조건 : 티켓에 있는 모든 경로 탐색 완료
        if(route.size() == tickets.length +1 ){
            return true;
        }
        // 스택의 제일 위 도시를 출발지로 삼음
        String startPoint = route.peek();
        for(int i = 0; i < tickets.length; i++ ){
            // 해당 출발지에서 아직 사용하지 않은 티켓을 순서대로 탐색
            if( !visited[i] && startPoint.equals(tickets[i][0]) ){
                // 해당 티켓의 도착지를 스택에 넣고 사용 체크
                route.push(tickets[i][1]);
                visited[i] = true;
                // 해당 티켓으로 끝까지 탐색이 완료 되었다면 종료
                if(dfs(tickets)){
                    return true;
                }
                // 해당 티켓일 때 탐색이 완료되지 못한다면, 티켓 사용 취소 후 다음 티켓 탐색
                route.pop();
                visited[i] = false;
            }
        }
        return false;
    }
}
```

