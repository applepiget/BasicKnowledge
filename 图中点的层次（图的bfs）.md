## 图中点的层次

```c++
//847. 图中点的层次(ACWing)
#include <iostream>
#include <algorithm>
#include <queue>
#include <cstring>

using namespace std;

const int N = 100010;
int h[N] , e[N] , ne[N] , idx;          //数组模拟邻接表
int d[N];                               //表示距离
int n , m;
queue<int> q;

void add(int v , int w)
{
    e[idx] = w;
    ne[idx] = h[v];
    h[v] = idx;
    idx ++;
}

int bfs()
{
    memset(d , -1 , sizeof(d));
    d[1] = 0;
    q.push(1);
    
    while(!q.empty())
    {
        int t = q.front();
        q.pop();
        for(int i = h[t] ; i != -1 ; i = ne[i])
        {
            int j = e[i];
            if(d[j] == -1)						//若该点是第一次被遍历到
            {
                d[j] = d[t] + 1;
                q.push(j);
            }
        }
    }
    return d[n];
}

int main()
{
    cin >> n >> m;
    
    memset(h , -1 , sizeof(h));
    
    while(m --)
    {
        int v , w;
        cin >> v >> w;
        add(v , w);
    }
    
    cout << bfs() << endl;
    return 0;
}
```



1. 使用了数组模拟邻接表，就是数组模拟单链表的扩展，由于邻接表是多个链表组成的，所以设置了一个表示头结点的数组，对于数组模拟链表还需要再进行理解
2. 对于图的遍历，还需要再进行练习

