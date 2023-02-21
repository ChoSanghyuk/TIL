



```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    static int weightCnt;
    static int[] weights;
    static int ballCnt;
    static int[] balls;
    static boolean[] dp;
    static int maxW;



    public static void main(String[] args) throws IOException {

        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));

        weightCnt = Integer.parseInt(bf.readLine());
        weights = new int[weightCnt+1];

        StringTokenizer st = new StringTokenizer(bf.readLine(), " ");
        for(int i =1; i <= weightCnt ; i++){
            weights[i] = Integer.parseInt(st.nextToken());
        }

        ballCnt = Integer.parseInt(bf.readLine());
        balls = new int[ballCnt];

        maxW = 0;

        st = new StringTokenizer(bf.readLine(), " ");
        for(int i =0; i < ballCnt ; i++){
            balls[i] = Integer.parseInt(st.nextToken());
            maxW = Math.max(balls[i], maxW);
        }

        dp = new boolean[maxW+1];

        dpSearch(0,0);

        StringBuilder sb = new StringBuilder();
        for(int ball : balls){
            sb.append(dp[ball] ? "Y" : "N" ).append(" ");
        }

        System.out.println(sb.toString());

    }

    public static void dpSearch(int idx, int w){

        if(w <= maxW ){
            dp[w] = true;
        }

        if(++idx <= weightCnt){
            dpSearch(idx, w+weights[idx]);
            dpSearch(idx, w);
            dpSearch(idx, Math.abs(w-weights[idx]));
        }

    }





}
```

