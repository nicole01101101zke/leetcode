1001 Battle Over Cities - Hard Version (35 分)

It is vitally important to have all the cities connected by highways in a war. If a city is conquered by the enemy, all the highways from/toward that city will be closed. To keep the rest of the cities connected, we must repair some highways with the minimum cost. On the other hand, if losing a city will cost us too much to rebuild the connection, we must pay more attention to that city.

Given the map of cities which have all the destroyed and remaining highways marked, you are supposed to point out the city to which we must pay the most attention.

### Input Specification:

Each input file contains one test case. Each case starts with a line containing 2 numbers `N` (≤500), and `M`, which are the total number of cities, and the number of highways, respectively. Then `M` lines follow, each describes a highway by 4 integers: `City1 City2 Cost Status` where `City1` and `City2` are the numbers of the cities the highway connects (the cities are numbered from 1 to `N`), `Cost` is the effort taken to repair that highway if necessary, and `Status` is either 0, meaning that highway is destroyed, or 1, meaning that highway is in use.

Note: It is guaranteed that the whole country was connected before the war.

### Output Specification:

For each test case, just print in a line the city we must protest the most, that is, it will take us the maximum effort to rebuild the connection if that city is conquered by the enemy.

In case there is more than one city to be printed, output them in increasing order of the city numbers, separated by one space, but no extra space at the end of the line. In case there is no need to repair any highway at all, simply output 0.

### Sample Input 1:

```in
4 5
1 2 1 1
1 3 1 1
2 3 1 0
2 4 1 1
3 4 1 0
```

### Sample Output 1:

```out
1 2
```

### Sample Input 2:

```in
4 5
1 2 1 1
1 3 1 1
2 3 1 0
2 4 1 1
3 4 2 1
```

### Sample Output 2:

```out
0
```

```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

int N,M;

struct edge{
    int v1,v2;
    int cost;
    int status;
};

edge e[200006];
int parent[501];
int clustersize[501];

void initfind(){
    for(int i=1;i<=N;i++){
        parent[i] = i;
        clustersize[i] = 1;
    }
}

int Find(int cur){
    if(parent[cur]==cur) return cur;
    else return parent[cur] = Find(parent[cur]);
}

bool unite(int a,int b){
    int aa = Find(a);
    int bb = Find(b);
    if(aa==bb){
        return false;
    }else{
        if(clustersize[aa]<clustersize[bb]){
            parent[bb] = aa;
            clustersize[aa] += clustersize[bb];
        }else{
            parent[aa] = bb;
            clustersize[bb] += clustersize[aa];
        }
        return true;
    }
}


bool comp(edge e1,edge e2){
    if(e1.status!=e2.status){
        return e1.status>e2.status;
    }
    return e1.cost<e2.cost;
}

int main(){
    cin>>N>>M;
    for(int i=0;i<M;i++){
        scanf("%d %d %d %d",&e[i].v1,&e[i].v2,&e[i].cost,&e[i].status);
    }
    sort(e,e+M,comp);

    int maxcost=0;
    vector<int> ans;

    for(int i=1;i<=N;i++){
        initfind();
        int cost=0;
        int cnt=0;
        for(int j=0;j<M;j++){
            int a=e[j].v1;
            int b=e[j].v2;
            if(a==i||b==i) continue;
            if(unite(a,b)){
                if(e[j].status==0) cost += e[j].cost;
                cnt++;
                if(cnt==N-2){
                    break;
                }
            }
        }
        if(cnt==N-2){
            if(cost>maxcost){
                maxcost = cost;
                ans={i};
            }else if(cost==maxcost){
                ans.push_back(i);
            }
        }else{
            cost = 100000000;
            if(cost>maxcost){
                maxcost = cost;
                ans={i};
            }else if(cost==maxcost){
                ans.push_back(i);
            }
        }
    }
    if(maxcost==0){
        cout<<0;
    }else{
        sort(ans.begin(),ans.end());
    	for(int i=0;i<ans.size();i++){
        	if(i>0) cout<<' ';
        	cout<<ans[i];
    	}
    }   
}
```

