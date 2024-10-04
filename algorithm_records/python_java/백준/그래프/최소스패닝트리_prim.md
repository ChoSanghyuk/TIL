# 백준 최소스패닝트리 prim 알고리즘 풀이 (java)



## 문제 설명

### 문제

그래프가 주어졌을 때, 그 그래프의 최소 스패닝 트리를 구하는 프로그램을 작성하시오.

최소 스패닝 트리는, 주어진 그래프의 모든 정점들을 연결하는 부분 그래프 중에서 그 가중치의 합이 최소인 트리를 말한다.

### 입력

첫째 줄에 정점의 개수 V(1 ≤ V ≤ 10,000)와 간선의 개수 E(1 ≤ E ≤ 100,000)가 주어진다. 다음 E개의 줄에는 각 간선에 대한 정보를 나타내는 세 정수 A, B, C가 주어진다. 이는 A번 정점과 B번 정점이 가중치 C인 간선으로 연결되어 있다는 의미이다. C는 음수일 수도 있으며, 절댓값이 1,000,000을 넘지 않는다.

그래프의 정점은 1번부터 V번까지 번호가 매겨져 있고, 임의의 두 정점 사이에 경로가 있다. 최소 스패닝 트리의 가중치가 -2,147,483,648보다 크거나 같고, 2,147,483,647보다 작거나 같은 데이터만 입력으로 주어진다.

### 출력

첫째 줄에 최소 스패닝 트리의 가중치를 출력한다.



## 문제 풀이

```java
import java.util.Scanner;
import java.util.ArrayList;
import java.util.List;
import java.util.PriorityQueue;

class Main
{
    // 간선 정보를 저장할 클래스
	static class Line implements Comparable<Line>{
		
		int end;
		int weight;
		
		public Line(int end, int weight) {
			this.end = end;
			this.weight = weight;
		}
		
		@Override
		public int compareTo(Line line) {
			return Integer.compare(this.weight, line.weight);
		}
	}
	public static void main(String args[])
	{
		Scanner sc = new Scanner(System.in);
			
		int V = sc.nextInt(); int E = sc.nextInt();
		
		boolean[] mst = new boolean[V+1]; // 방문 여부 기록
		List<Line>[] table = new List[V+1];	// 노드간 이동 간선 저장
		
		for(int i = 1; i < V+1; i++) {
			table[i] = new ArrayList<Line>();
		}
		
		for(int i = 0; i < E; i++) {
			int a = sc.nextInt(); int b = sc.nextInt(); int c = sc.nextInt();
			table[a].add(new Line(b,c));
			table[b].add(new Line(a,c));
		}
		
        // prim 알고리즘 사용
		int sumWeight = 0;
		PriorityQueue<Line> heap = new PriorityQueue<>();
		heap.add(new Line(1,0));
		
		while(!heap.isEmpty()) {

			Line now = heap.poll();
            // ! 이미 방문했던 곳이면 또 탐색 x
			if(mst[now.end]) {
				continue;
			}
			mst[now.end] = true;
			sumWeight += now.weight;
			
			for(Line nextLine : table[now.end]) {
				if(!mst[nextLine.end]) {
					heap.add(nextLine);
				}
				
			}
			
			
		}
		System.out.println(sumWeight);
		
}
}
```

