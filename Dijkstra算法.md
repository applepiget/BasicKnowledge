## Dijkstra算法

```c++
//普通dijkstra算法 
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 510;
int n , m;
int d[N] , g[N][N];
bool remark[N];

int dijkstra()
{
    memset(d , 0x3f , sizeof d);
    d[1] = 0;
    
    for(int i = 0 ; i < n ; i ++)
    {
        int t = -1;
        for(int j = 1 ; j < n ; j ++)
            if(!remark[j] && (t == -1 || d[t] > d[j]))
                t = j;
        
        remark[t] = true;
        
        for(int j = 1 ; j <= n ; j ++)
            d[j] = min(d[j] , d[t] + g[t][j]);
    }
    
    if(d[n] == 0x3f)
        return -1;
    return d[n];
}

int main()
{
    cin >> n >> m;
    
    memset(g , 10010 , sizeof g);   
       
    while(m --)
    {
        int v , w , info;
        cin >> v >> w >> info;
        g[v][w] = min(g[v][w] , info);
    }
    
    cout << dijkstra() << endl;
    return 0;
}

```



堆优化：

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <queue>

using namespace std;

typedef pair<int , int> ii;

const int N = 150100;
int n , m;
int h[N] , e[N] , ne[N] , w[N] , d[N] , idx;
bool mark[N];

int dijkstra()
{
    memset(d , 0x3f , sizeof d);
    d[1] = 0;
    
    priority_queue<ii , vector<ii> , greater<ii>> heap;     //在堆中放入编号 和 距离
    heap.push({0 , 1});
    
    while(heap.size())
    {
        auto t = heap.top();
        heap.pop();
        
        int vex = t.second , dis = t.first;
        
        if(mark[vex])
            continue;                           //如果该点已被遍历过，continue
        mark[vex] = true;
        
        for(int i = h[vex] ; i != -1 ; i = ne[i])
        {
            int j = e[i];
            if(d[j] > dis + w[i])
            {
                d[j] = dis + w[i];
                heap.push({d[j] , j});
            }
        }
    }
    if(d[n] == 0x3f3f3f3f)
        return -1;
    return d[n];
}

void insert_head(int x , int y , int z)
{
    e[idx] = y;
    w[idx] = z;
    ne[idx] = h[x];
    h[x] = idx ++;
}

int main()
{
    cin >> n >> m;
    
    memset(h , -1 , sizeof h);
    
    while(m --)
    {
        int x , y , z;
        cin >> x >> y >> z;
        insert_head(x , y , z);
    }
    
    cout << dijkstra() << endl;
    return 0;
}
```



**注意：**在小根堆中存入*pair<int , int>*时，注意是**距离在前，编号在后**，因为是利用小根堆的特性来直接找到当前遍历中距离最小且未被遍历到的顶点