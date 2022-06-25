## BFS/广度优先遍历

特点：可以搜索到最短路，只有同边权的图才可以用BFS

基本框架（用队列实现）：

```c++
1.初始化队列
2.while(!queue.empty())
{
	t = q.front();
    拓展t
}
```

```c++
//844.走迷宫
#include <iostream>
#include <queue>
#include <cstring>

using namespace std;

typedef pair<int , int> ii;

const int N = 110;
int p[N][N] , d[N][N];          //p存放图，d存放每个点到起点的距离
int n , m;
queue<ii> q;

int bfs()
{
    memset(d , -1 , sizeof d);  //初始化d中所有元素的值为-1
    d[0][0] = 0;
    
    q.push({0 , 0});
    
    while(!q.empty())
    {
        auto t = q.front();
        q.pop();
        
        int dx[4] = {0 , 1 , 0 , -1} , dy[4] = {1 , 0 , -1 , 0};       //向上右下左四个方向移动时下标的改变量
        for(int i = 0 ; i < 4 ; i ++)
        {
            int x = t.first + dx[i] , y = t.second + dy[i];
            if(x >= 0 && y >= 0 && x < n && y < m && p[x][y] == 0 && d[x][y] == -1)
            {
                d[x][y] = d[t.first][t.second] + 1;
                q.push({x , y});
            }
        }
    }
    return d[n - 1][m - 1];
}

int main()
{
    cin >> n >> m;
    for(int i = 0 ; i < n ; i ++)
        for(int j = 0 ; j < m ; j ++)
            cin >> p[i][j];
            
    cout << bfs() << endl;
    
    return 0;
}
```

