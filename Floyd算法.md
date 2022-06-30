## Floyd算法

```c++
//854. Floyd求最短路(ACWing)
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 210 , INF = 1e9;
int n , m , k;
int g[N][N];

int main()
{
    cin >> n >> m >> k;
    
    for(int i = 1 ; i <= n ; i ++)
        for(int j = 1 ; j <= n ; j ++)
        {
            if(i == j)              //自环为0
                g[i][j] = 0;
            else
                g[i][j] = INF;
        }
    
    while(m --)
    {
        int v , w , f;
        cin >> v >> w >> f;
        g[v][w] = min(g[v][w] , f);
    }
    
    //floyd
    for(int k = 1 ; k <= n ; k ++)
        for(int i = 1 ; i <= n ; i ++)
            for(int j = 1 ; j <= n ; j ++)
                g[i][j] = min(g[i][j] , g[i][k] + g[k][j]);
    
    while(k --)
    {
        int v , w;
        cin >> v >> w;
        
        if(g[v][w] > INF / 2)
            cout << "impossible" << endl;
        else
            cout << g[v][w] << endl;
    }
    
    return 0;
}
```



需要注意最后的判断条件：

```c++
if(g[v][w] > INF / 2)
```

如果写成 == INF就会造成错误，感觉这是一个比较模糊的判断，需要看题目中具体的数据范围