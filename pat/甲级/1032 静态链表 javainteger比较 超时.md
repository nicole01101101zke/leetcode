1032 Sharing (25 分)

To store English words, one method is to use linked lists and store a word letter by letter. To save some space, we may let the words share the same sublist if they share the same suffix. For example, `loading` and `being` are stored as showed in Figure 1.

![fig.jpg](https://images.ptausercontent.com/ef0a1fdf-3d9f-46dc-9a27-21f989270fd4.jpg)

Figure 1

You are supposed to find the starting position of the common suffix (e.g. the position of `i` in Figure 1).

### Input Specification:

Each input file contains one test case. For each case, the first line contains two addresses of nodes and a positive *N* (≤105), where the two addresses are the addresses of the first nodes of the two words, and *N* is the total number of nodes. The address of a node is a 5-digit positive integer, and NULL is represented by −1.

Then *N* lines follow, each describes a node in the format:

```
Address Data Next
```

where`Address` is the position of the node, `Data` is the letter contained by this node which is an English letter chosen from { a-z, A-Z }, and `Next` is the position of the next node.

### Output Specification:

For each case, simply output the 5-digit starting position of the common suffix. If the two words have no common suffix, output `-1` instead.

### Sample Input 1:

```in
11111 22222 9
67890 i 00002
00010 a 12345
00003 g -1
12345 D 67890
00002 n 00003
22222 B 23456
11111 L 00001
23456 e 67890
00001 o 00010
```

### Sample Output 1:

```out
67890
```

### Sample Input 2:

```in
00001 00002 4
00001 a 10001
10001 s -1
00002 a 10002
10002 t -1
```

### Sample Output 2:

```out
-1
```

```java
import java.util.*;
import java.io.*;
import java.text.*;

class Node{
    int next;
    //char letter;
}

public class Main{
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] s0 = br.readLine().split(" ");
        int h1 = Integer.valueOf(s0[0]);
        int h2 = Integer.valueOf(s0[1]);
        int N = Integer.valueOf(s0[2]);

        List<Integer> list1 = new ArrayList<>();
        List<Integer> list2 = new ArrayList<>();

        Node[] nodes = new Node[100000];
        while(N>0){
            String[] s1 = br.readLine().split(" ");
            int i = Integer.valueOf(s1[0]);
            nodes[i] = new Node();
            nodes[i].next = Integer.valueOf(s1[2]);
            //nodes[i].letter = Charactor.valueOf(s1[1]);
            N--;
        }

        for(int i=h1;i!=-1;i=nodes[i].next){
            list1.add(i);
        }
        for(int i=h2;i!=-1;i=nodes[i].next){
            list2.add(i);
        }

//        for(int i=0;i<list1.size();i++){
//            System.out.print(String.format("%05d",list1.get(i)));
//            System.out.print(" ");
//        }
//        System.out.print("\n");
//        for(int i=0;i<list2.size();i++){
//            System.out.print(String.format("%05d",list2.get(i)));
//            System.out.print(" ");
//        }
//        System.out.print("\n");

        int j = list1.size()-1;
        int k = list2.size()-1;
//        System.out.println(j);
//        System.out.println(k);
        while(j>=0 && k>=0 && list1.get(j).intValue()==list2.get(k).intValue()){
            //while(j>=0 && k>=0 && list1.get(j)==list2.get(k)){
             j--;
             k--;
        }
//        if(list1.get(4)==list2.get(2)){
//            System.out.println("true");
//        }else{
//            System.out.println("false");
//            System.out.println(list1.get(4));
//            System.out.println(list2.get(2));
//        }
//        System.out.println(j);
//        System.out.println(k);
        if(j==list1.size()-1){
            //System.out.println("0");
            System.out.println("-1");
        }else{
            System.out.println(String.format("%05d",list1.get(j+1)));
        }
    }
}
```



![image-20210823115402321](C:\Users\nicole\AppData\Roaming\Typora\typora-user-images\image-20210823115402321.png)