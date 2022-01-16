1101 Quick Sort (25 分)

There is a classical process named **partition** in the famous quick sort algorithm. In this process we typically choose one element as the pivot. Then the elements less than the pivot are moved to its left and those larger than the pivot to its right. Given *N* distinct positive integers after a run of partition, could you tell how many elements could be the selected pivot for this partition?

For example, given *N*=5 and the numbers 1, 3, 2, 4, and 5. We have:

- 1 could be the pivot since there is no element to its left and all the elements to its right are larger than it;
- 3 must not be the pivot since although all the elements to its left are smaller, the number 2 to its right is less than it as well;
- 2 must not be the pivot since although all the elements to its right are larger, the number 3 to its left is larger than it as well;
- and for the similar reason, 4 and 5 could also be the pivot.

Hence in total there are 3 pivot candidates.

### Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer *N* (≤105). Then the next line contains *N* distinct positive integers no larger than 109. The numbers in a line are separated by spaces.

### Output Specification:

For each test case, output in the first line the number of pivot candidates. Then in the next line print these candidates in increasing order. There must be exactly 1 space between two adjacent numbers, and no extra space at the end of each line.

### Sample Input:

```in
5
1 3 2 4 5
```

### Sample Output:

```out
3
1 4 5
```

```c++
#include<iostream>
#include<vector>
#include<algorithm>

using namespace std;

int main(){
    int N;
    cin>>N;
    int a[N];
    for(int i=0;i<N;i++){
        cin>>a[i];
    }
    vector<int> res;
    int dp1[N];
    int dp2[N];
    dp1[0] = -2147483647;
    dp2[N-1] = 2147483647;
    for(int i=1;i<N;i++){
        dp1[i] = max(dp1[i-1],a[i-1]);
    }
    for(int i=N-2;i>=0;i--){
        dp2[i] = min(dp2[i+1],a[i+1]);
    }
    for(int i=0;i<N;i++){
        if(dp1[i]<a[i]&&dp2[i]>a[i]){
            res.push_back(a[i]);
        }
    }
    sort(res.begin(),res.end());
    cout<<res.size()<<endl;
    for(int i=0;i<res.size();i++)
    {
        if(i>0){
            cout<<" ";
        }
        cout<<res[i];
    }
    cout<<endl;
}
```

