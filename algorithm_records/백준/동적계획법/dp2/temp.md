



```java
import java.io.*;
import java.util.StringTokenizer;

public class Main {

    static int weightCnt;
    static int[] weights;
    static int ballCnt;
    static int[] balls;
    static boolean[] dp;



    public static void main(String[] args) throws IOException {

        BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));

        weightCnt = Integer.parseInt(bf.readLine());
        weights = new int[weightCnt+1];

        StringTokenizer st = new StringTokenizer(bf.readLine(), " ");
        for(int i =0; i < weightCnt ; i++){
            weights[i] = Integer.parseInt(st.nextToken());
        }

        ballCnt = Integer.parseInt(bf.readLine());
        balls = new int[ballCnt];

        int maxW = 0;

        st = new StringTokenizer(bf.readLine(), " ");
        for(int i =0; i < weightCnt ; i++){
            balls[i] = Integer.parseInt(st.nextToken());
            maxW = Math.max(balls[i], maxW);
        }

        dp = new boolean[maxW+1];

        dpSearch(-1,0);

        StringBuilder sb = new StringBuilder();
        for(int ball : balls){
            sb.append(dp[ball]).append(" ");
        }

        System.out.println(sb.toString());


    }

    public static void dpSearch(int idx, int w){

        if(idx == weightCnt) return;

        System.out.println(w);
        dp[w] = true;

        dpSearch(idx+1, w+weights[idx+1]);
        dpSearch(idx+1, w);
        dpSearch(idx+1, Math.abs(w-weights[idx+1]));


    }





}
```

