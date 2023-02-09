



```
import java.io.*;
import java.util.Arrays;
import java.util.Comparator;
import java.util.StringTokenizer;

public class Main {

    static int[] lis;

    public static void main(String[] args) throws IOException {

       BufferedReader bf = new BufferedReader(new InputStreamReader(System.in));

       int c = Integer.parseInt(bf.readLine());
       for(int t = 0; t < c ; t++){

           int n = Integer.parseInt(bf.readLine());
           int[] arr = new int[n];

           StringTokenizer st = new StringTokenizer(bf.readLine(), " ");
           for(int i = 0; i < n ; i++){
               arr[i] = Integer.parseInt(st.nextToken());
           }



       }


    }

    public static int simpleSum(int[] arr, int i , int j){
        int sum = 0;
        for(int x = i ; x <=j; x++){
            sum += arr[x];
        }
        return sum;
    }

    public static int dpSum(int[][] dp, int i, int j, int d){
        int sum = 0;

        for(int x = i-)

    }





}
```

