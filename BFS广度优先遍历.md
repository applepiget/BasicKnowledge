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

```c++
//845.八数码
#include <iostream>
#include <queue>
#include <unordered_map>
#include <algorithm>

using namespace std;

int bfs(string start)
{
    string end = "12345678x";                                   //需要搜索的状态
    queue<string> q;                                            //存放每个状态的网格
    unordered_map<string , int> dis;                            //存放每个状态已经经过的距离
    int dx[4] = {0 , 1 , 0 , -1} , dy[4] = {-1 , 0 , 1 , 0};    //上右下左
    
    q.push(start);
    dis[start] = 0;
    
    while(!q.empty())
    {
        auto t = q.front();
        q.pop();
        
        int distance = dis[t];
        
        if(t == end)
            return distance;
        
        //状态改变
        int k = t.find('x');                                    //找到当前状态中x的位置
        int x = k / 3 , y = k % 3;                              //将其对应到字符串当中的位置
        
        for(int i = 0 ; i < 4 ; i ++)
        {
            int a = x + dx[i] , b = y + dy[i];
            if(a >= 0 && b >= 0 && a < 3 && b < 3)
            {
                swap(t[k] , t[a * 3 + b]);
                if(!dis.count(t))                                 //如过没有走到过这个位置
                {
                    dis[t] = distance + 1;
                    q.push(t);
                }
                swap(t[k] , t[a * 3 + b]);
            }
        }
    }
    return -1;
}

int main()
{
    string start;
    for(int i = 0 ; i < 9 ; i ++)
    {
        char c;
        cin >> c;
        start += c;
    }
    
    cout << bfs(start) << endl;
    return 0;
}
```

