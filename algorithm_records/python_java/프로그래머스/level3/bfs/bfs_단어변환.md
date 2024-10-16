# 프로그래머스 단어변환 문제 풀이 (java)



## 문제 설명

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

```
1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.
```

예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

##### 입출력 예

| begin | target | words                                      | return |
| ----- | ------ | ------------------------------------------ | ------ |
| "hit" | "cog"  | ["hot", "dot", "dog", "lot", "log", "cog"] | 4      |
| "hit" | "cog"  | ["hot", "dot", "dog", "lot", "log"]        | 0      |



## 문제 풀이

```java
import java.util.Queue;
import java.util.LinkedList;
class Solution {
    public int solution(String begin, String target, String[] words) {
        
        // 큐의 입력 시기를 기록하는 변수이자, 몇번 만에 변환되는지 카운트
        int height = 0;
        Queue<String> queue = new LinkedList<>();
        boolean[] visited = new boolean[words.length];
        
        queue.add(begin);
        String temp;
        LinkedList<String> floor;
        
        while(!queue.isEmpty()){
            
            height++;
            floor = new LinkedList<>();
            // 같은 층의 있는 변수들 한번에 처리
            while(!queue.isEmpty()){           
                temp = queue.poll();
                for(int i = 0 ; i < words.length; i++){
                    // 아직 변환해본적 없고, 변환할 수 있는 단어 확인
                    if( !visited[i] && isConvertable(temp, words[i]) ){
                        // 해당 단어가 타켓 단어면 현재 층 반환하며 종료
                        if(words[i].equals(target)){
                            return height;
                        }
                        // 아닐 경우 층에 넣어두고, 방문 표시
                        floor.add(words[i]);
                        visited[i] = true;
                    }

                }
            }
            // 같은 층에서 들어온 친구들은 한번에 큐에 입력
            queue.addAll(floor);
        }
        
        // 탐색 완료했음에도 타겟 단어로 이동이 안되면 0 반환
        return 0;
    }
    // 현재 단어와 글자 차이가 1 밖에 안 나는지 여부 확인
    public boolean isConvertable(String b, String t){
        
        for(int i =0 ; i < t.length(); i++){
            if( b.matches( t.substring(0,i) + "[a-z]" + t.substring(i+1) )  ){
                return true;
            }
        }
        return false;
        
    }
}
```

