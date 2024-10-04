# SWEA 1220 magnetic 문제풀이 (자바)



## 문제 링크

https://swexpertacademy.com/main/code/problem/problemDetail.do?contestProbId=AV14hwZqABsCFAYD&categoryId=AV14hwZqABsCFAYD&categoryType=CODE&problemTitle=1220&orderBy=FIRST_REG_DATETIME&selectCodeLang=ALL&select-1=&pageSize=10&pageIndex=1&&&&&&&&&



## 문제 풀이

array로 스택을 구현하여 풀이하였다. 열을 기준으로 위에서부터 행을 하나씩 스택에 넣을 때, 스택이 비어있을 때 S이거나, 스택의 맨 위에 있는 자성체와 같은 종류의 자성체가 들어올 경우에는 스택에 넣지 않았다. 마지막으로 스택의 맨 위가 N극이면 맨 위가 S극이 될 때까지 제거하였다.

```java
import java.util.Scanner;
import java.io.FileInputStream;


class Solution
{
	static int[][] table;
	static int N;
	public static void main(String args[]) throws Exception
	{
        
		// 인풋 받아옴
		System.setIn(new FileInputStream("C:/Users/chosh/ssafy/6ss1-aps-hub/0827/1220_magnetic/input.txt"));
		Scanner sc = new Scanner(System.in);
		int T = 10;

		for(int test_case = 1; test_case <= T; test_case++)
		{
            // table을 입력값에 따라 채움
			int[] stack;
			int idx;
			int count = 0;
			N = sc.nextInt();
			table = new int[N][N];
			for(int i = 0 ; i < N; i++) {
				for(int j = 0; j < N; j++) {
					table[i][j] = sc.nextInt();
				}
			}
			
			for(int j = 0 ; j < N; j++) {
                // 열을 기준으로 스택 생성
				stack = new int[N];
				idx=0;
				for(int i = 0; i < N; i++) {
                    // 0인 경우 넘김
					if(table[i][j] != 0) {
						// 스택이 비어있을 때, S극이면 받지 않음
						if(idx==0 && table[i][j]==2) {
							continue;
						} else if(idx==0) {
							stack[idx] = table[i][j];
							idx++;
                        // 스택의 맨 위와 같은 종류가 오면 받지 않음
						} else if(stack[idx-1] == table[i][j] ) {
							continue;
						}else {
							stack[idx] = table[i][j];
							idx++;							
						}
						
					}
				}
                // 스택의 맨 위가 N극이라면 S극이 될 때까지 다 제거
				for(int i = idx; i >=0; i--) {
					if(stack[i] == 1) {
						idx--;
					}else {
						break;
					}
				}
                // 스택의 있는 개수 /2 만큼이 교착의 수이다.
				count += idx/2;

			}
			System.out.println("#"+test_case+" "+count);		
		}	
	}
}
```

