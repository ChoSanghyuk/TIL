



```
import java.io.*;
import java.util.Arrays;
import java.util.Comparator;
import java.util.StringTokenizer;

public class Main {

    static int[] arr;
    static int[][] dp;

    public static void main(String[] args) throws IOException {

       BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));

       int c = Integer.parseInt(bf.readLine());
       for(int t = 0; t < c ; t++){

           int n = Integer.parseInt(bf.readLine());
           arr = new int[n+1];
           dp = new int[n+1][n+1];

           StringTokenizer st = new StringTokenizer(bf.readLine(), " ");
           for(int i = 1; i <= n ; i++){
               arr[i] = Integer.parseInt(st.nextToken());
           }
           for(int i = n ; i > 0 ; i--){
               for(int j = n ; j>=i ; j--){

                   int min = Integer.MAX_VALUE;
                   for(int s = j ; s >=i ; s--){
                       min = Math.min(min, dp[i][s] + dp[i +(j-s)][j]);
                   }

                   dp[i][j]= simpleSum(i,j) + min;

               }
           }



       }


    }

    public static int simpleSum(int i , int j){
        int sum = 0;
        for(int x = i ; x <=j; x++){
            sum += arr[x];
        }
        return sum;
    }

    public static int dpSum(int i, int j, int s){

        return dp[i][s] + dp[s+1][j];


    }





}
```

