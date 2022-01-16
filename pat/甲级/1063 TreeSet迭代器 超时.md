1063 Set Similarity (25 分)

Given two sets of integers, the similarity of the sets is defined to be Nc/Nt×100%, where Nc is the number of distinct common numbers shared by the two sets, and Nt is the total number of distinct numbers in the two sets. Your job is to calculate the similarity of any given pair of sets.

### Input Specification:

Each input file contains one test case. Each case first gives a positive integer *N* (≤50) which is the total number of sets. Then *N* lines follow, each gives a set with a positive *M* (≤104) and followed by *M* integers in the range [0,109]. After the input of sets, a positive integer *K* (≤2000) is given, followed by *K* lines of queries. Each query gives a pair of set numbers (the sets are numbered from 1 to *N*). All the numbers in a line are separated by a space.

### Output Specification:

For each query, print in one line the similarity of the sets, in the percentage form accurate up to 1 decimal place.

### Sample Input:

```in
3
3 99 87 101
4 87 101 5 87
7 99 101 18 5 135 18 99
2
1 2
1 3
```

### Sample Output:

```out
50.0%
33.3%
```

```java
import java.util.*;
import java.io.*;
import java.text.*;


public class Main{
	public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        int N = Integer.valueOf(br.readLine()).intValue();
        TreeSet<Integer>[] hs = new TreeSet[N];

        for(int i=0;i<N;i++){
            String[] s1 = br.readLine().split(" ");
            hs[i] = new TreeSet<>();
            for(int j=1;j<s1.length;j++) {
                hs[i].add(Integer.valueOf(s1[j]));
            }

        }
        int K = Integer.valueOf(br.readLine()).intValue();
        while(K>0){
            String[] s = br.readLine().split(" ");
            int a = Integer.valueOf(s[0]).intValue()-1;
            int b = Integer.valueOf(s[1]).intValue()-1;
//            Iterator<Integer> it1 = hs[a].iterator();
//            Iterator<Integer> it2 = hs[b].iterator();
//            while(it1.hasNext()&&it2.hasNext()){
//
//            }
            TreeSet<Integer> temp = new TreeSet<>();
            for(Integer t:hs[a]){
                temp.add(t);
            }
            for(Integer t:hs[b]){
                temp.add(t);
            }
            int Nc = hs[a].size()+hs[b].size()-temp.size();
            int Nt = temp.size();
            System.out.println(String.format("%.1f",100.0*Nc/Nt)+"%");
            K--;
        }

    }
}

```

