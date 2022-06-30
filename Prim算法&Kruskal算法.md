## Prim算法&Kruskal算法

### Prim

```c++
//858. Prim算法求最小生成树(ACWing)
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

const int N = 510 , INF = 0x3f3f3f3f;
int n , m;
int g[N][N] , d[N];
bool mark[N];

int prim()
{
    memset(d , 0x3f , sizeof d);
    
    int res = 0;
    for(int i = 0 ; i < n ; i ++)
    {
        int t = -1;
        for(int j = 1; j <= n ; j ++)
            if(!mark[j] && (t == -1 || d[t] > d[j]))
                t = j;
                
        if(i && d[t] == INF)                            //如果的d[t] == INF,说明不连通
            return INF;
        if(i)                                           //如果是第一个顶点，则不计入距离
            res += d[t];
            
        for(int k = 1 ; k <= n ; k ++)                  //加入新的顶点后更新距离
            d[k] = min(d[k] , g[t][k]); 
        
        mark[t] = true;
    }
    return res;
}

int main()
{
    memset(g , 0x3f , sizeof g);
    
    cin >> n >> m;
    while(m --)
    {
        int a , b , w;
        cin >> a >> b >> w;
        g[a][b] = g[b][a] = min(g[a][b] , w);       //每一条边对应的两个顶点都需要存储
    }
    
    int t = prim();
    if(t == INF)
        cout << "impossible" << endl;
    else 
        cout << t << endl;
    return 0;
}
```



---



### Kruskal

1. 将所有边按权值从小到大排序
2. 枚举每条边，如果找到一条边不在集合中，将其加入集合中

```c++
//859. Kruskal算法求最小生成树(ACWing)
#include <iostream>
#include <algorithm>

using namespace std;

struct graph{
    int a , b , w;
    
    bool operator< (const graph &t) const
    {
        return w < t.w;
    }
};

const int N = 200010;
int p[N];                               //并查集
graph g[N];
int n , m;

int find(int x)
{
    if(p[x] != x)
        p[x] = find(p[x]);
    return p[x];
}

int main()
{
    cin >> n >> m;
    
    for(int i = 0 ; i < m ; i ++)
    {
        int a , b , w;
        cin >> a >> b >> w;
        g[i] = {a , b , w};
    }
    
    for(int i = 1 ; i <= n ; i ++)
        p[i] = i;                       //初始化并查集
    
    //Kruskal
    sort(g , g + m);
        
    int res = 0 , count = 0;
    for(int i = 0 ; i < m ; i ++)
    {
        int a = g[i].a , b = g[i].b , w = g[i].w;
        
        a = find(a);    b = find(b);
        if(a != b)
        {
            p[a] = b;
            res += w;
            count ++;
        }
    }
    
    if(count < n - 1)
        cout << "impossible" << endl;
    else
        cout << res << endl;
    return 0;
}
```

