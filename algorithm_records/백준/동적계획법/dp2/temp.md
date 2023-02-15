```
import java.io.*;
import java.util.Arrays;
import java.util.Comparator;
import java.util.StringTokenizer;

public class Main {

    private static int[][] map;
    private static long[][] dp;
    private static int[] dx = {0,1};
    private static int[] dy = {1,0};

    public static void main(String[] args) throws IOException {

        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(bf.readLine(), " ");

        int n = Integer.parseInt(st.nextToken());
        int m = Integer.parseInt(st.nextToken());

        map = new int[n+1][m+1];
        dp = new long[n+1][m+1];

        for(int i = 1 ; i <= n ; i++){
            st = new StringTokenizer(bf.readLine(), " ");
            for(int j =1 ; j <= m ; j++){
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        dp[n][m] = 1;
        for(int i = n ; i >0 ; i--){
            for(int j = m ; j > 0 ; j--){

                for(int d = 0; d < 2 ; d++){
                    if( i+dy[d] <= n && map[i][j] > map[i+dy[d]][j]  ){
                        dp[i][j] += dp[i+dy[d]][j];
                    }
                    if( j+dx[d] <= m && map[i][j] > map[i][j+dx[d]]  ){
                        dp[i][j] += dp[i][j+dx[d]];
                    }
                }

            }
        }

        System.out.println(dp[1][1]);


    }




}
```