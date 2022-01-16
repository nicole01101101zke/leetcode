1053 Path of Equal Weight (30 分)

Given a non-empty tree with root *R*, and with weight Wi assigned to each tree node Ti. The **weight of a path from R to L** is defined to be the sum of the weights of all the nodes along the path from *R* to any leaf node *L*.

Now given any weighted tree, you are supposed to find all the paths with their weights equal to a given number. For example, let's consider the tree showed in the following figure: for each node, the upper number is the node ID which is a two-digit number, and the lower number is the weight of that node. Suppose that the given number is 24, then there exists 4 different paths which have the same given weight: {10 5 2 7}, {10 4 10}, {10 3 3 6 2} and {10 3 3 6 2}, which correspond to the red edges in the figure.

![img](https://images.ptausercontent.com/212)

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 0<*N*≤100, the number of nodes in a tree, *M* (<*N*), the number of non-leaf nodes, and 0<*S*<230, the given weight number. The next line contains *N* positive numbers where Wi (<1000) corresponds to the tree node Ti. Then *M* lines follow, each in the format:

```
ID K ID[1] ID[2] ... ID[K]
```

where `ID` is a two-digit number representing a given non-leaf node, `K` is the number of its children, followed by a sequence of two-digit `ID`'s of its children. For the sake of simplicity, let us fix the root ID to be `00`.

### Output Specification:

For each test case, print all the paths with weight S in **non-increasing** order. Each path occupies a line with printed weights from the root to the leaf in order. All the numbers must be separated by a space with no extra space at the end of the line.

Note: sequence {*A*1,*A*2,⋯,An} is said to be **greater than** sequence {*B*1,*B*2,⋯,Bm} if there exists 1≤*k*<*min*{*n*,*m*} such that Ai=Bi for *i*=1,⋯,k, and Ak+1>Bk+1.

### Sample Input:

```in
20 9 24
10 2 4 3 5 10 2 18 9 7 2 2 1 3 12 1 8 6 2 2
00 4 01 02 03 04
02 1 05
04 2 06 07
03 3 11 12 13
06 1 09
07 2 08 10
16 1 15
13 3 14 16 17
17 2 18 19结尾无空行
```

### Sample Output:

```out
10 5 2 7
10 4 10
10 3 3 6 2
10 3 3 6 2
```

```java
import java.util.*;
import java.io.*;
import java.text.*;


public class Main{
    public static int[] weight = new int[101];
    public static List<Integer>[] neighbor = new ArrayList[101];
    //public static int[] visited = new int[101];
    public static int target;
    public static List<Integer> path = new ArrayList<>();
    public static List<Integer>[] finalans = new ArrayList[101];
    public static int cnt=0;
    public static void dfs(int cur,int curw){
//visited及时止损是为了防止成环，树不需要，本身无环
//        if(visited[cur]==1){
//            return;
//        }
//        visited[cur] = 1;
        if(curw>target){
            return;
        }
        if(neighbor[cur].size()==0&&curw!=target){
            return;
        }
        //上面的内容可以不写，第一个省略了不必要的继续遍历，第二个后面的path.remove已经实现了它的功能

        path.add(weight[cur]);
        if(curw==target && neighbor[cur].size()==0){
            finalans[cnt] = new ArrayList<>(path);
            cnt++;
        }
        for(int i=0;i<neighbor[cur].size();i++){
            dfs(neighbor[cur].get(i),curw+weight[neighbor[cur].get(i)]);
        }
        path.remove(path.size()-1);
    }
    public static void main(String[] args) throws Exception{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        String[] s1 = br.readLine().split(" ");
        int N = Integer.valueOf(s1[0]).intValue();
        int M = Integer.valueOf(s1[1]).intValue();
        target = Integer.valueOf(s1[2]).intValue();

        String[] s2 = br.readLine().split(" ");
        for(int i=0;i<s2.length;i++){
            weight[i] = Integer.valueOf(s2[i]).intValue();
        }
        for(int i=0;i<N;i++){
            neighbor[i] = new ArrayList<Integer>();
        }

        for(int i=0;i<M;i++){
            String[] s3 = br.readLine().split(" ");
            int index = Integer.valueOf(s3[0]).intValue();
            for(int j=2;j<s3.length;j++){
                neighbor[index].add(Integer.valueOf(s3[j]));
            }
        }
        dfs(0,weight[0]);
        String[] res = new String[cnt];
        for(int i=0;i<cnt;i++){
            StringBuilder sb = new StringBuilder();
            for(int j=0;j<finalans[i].size();j++){
                if(j>0){
                    sb.append(" ");
                }
                sb.append(finalans[i].get(j));
            }
            res[i] = sb.toString();
        }
        Arrays.sort(res,new comp());
        for(int i=0;i<cnt;i++){
            System.out.println(res[i]);
        }
    }
}

//第二个测试点不通过因为比较函数有bug
//按题意，3 11 5 应当在 3 2 14前面，不能进行简单的字符串比较
class comp implements Comparator<String>{
//     public int compare(String s1,String s2){
//         return s2.compareTo(s1);
//     }
     public int compare(String o1, String o2) {
        String[] temp1 = o1.split(" ");
        String[] temp2 = o2.split(" ");
        int edge = Math.min(temp1.length, temp2.length);
        for(int i = 1 ; i < edge ; i++) {
            if(temp1[i].equals(temp2[i]));
            else {
                int int1 = Integer.parseInt(temp1[i]);
                int int2 = Integer.parseInt(temp2[i]);
                if(int1 > int2) {
                    return -1;
                }
                else
                    return 1;
            }
        }
        return 0;
    }
}
```

