## DFS/深度优先遍历

```c++
//842. 排列数字
//给定一个整数 n，将数字 1∼n 排成一排，将会有很多种排列方法。现在，请你按照字典序将所有的排列方法输出。
#include <iostream>

using namespace std;

const int N =10;
int n;
int path[N];
bool state[N] = {0};    //表示当前位置的数字有没有被使用过

void dfs(int x)
{
    if(x == n)
    {
        for(int i = 0 ; i < n ; i ++)
            cout << path[i] << ' ';
        cout << endl;
        return;
    }
    
    for(int i = 1 ; i <= n ; i ++)
    {
        if(!state[i])
        {
            path[x] = i;
            state[i] = 1;
            dfs(x + 1);
            state[i] = 0;				//递归结束后要保证跟递归前的数字使用情况一样
        }
    }
}

int main()
{
    cin >> n;
    dfs(0);
    
    return 0;
}
```

第二次看这个这个题目，感觉比较思路清晰，这个递归还算简单，需要再加深一下印象



---



```c++
//843. n-皇后问题
//n−皇后问题是指将 n 个皇后放在 n×n 的国际象棋棋盘上，使得皇后不能相互攻击到，即任意两个皇后都不能处于同一行、同一列或同一斜线上。
//DFS中加判断条件 枚举每行
#include <iostream>

using namespace std;

const int N = 20;
char c[N][N];
int n;
bool col[N] , dg[N] , udg[N];   //表示每一列，对角线和反对角线是否已经被存放

void dfs(int x)
{
    if(x == n)
    {
        for(int i = 0 ; i < n ; i ++)
            cout << c[i] << endl;
        cout << endl;
        return;
    }
    
    
    
    for(int i = 0 ; i < n ; i ++)
    {
        if(!col[i] && !dg[x + i] && !udg[n - x + i])        //n - x + i , 其中n为偏移															  量，防止数组下标出现负数
      	{
            c[x][i] = 'Q';
            col[i] = dg[x + i] = udg[n - x + i] = true;
            dfs(x + 1);
            col[i] = dg[x + i] = udg[n - x + i] = false;
            c[x][i] = '.';
        }
    }
}

int main()
{
    cin >> n;
    for(int i = 0 ; i < n ; i ++)
        for(int j = 0 ; j < n ; j ++)
            c[i][j] = '.';
    
    dfs(0);
    
    return 0;
}
```



需要注意:

```c++
c[x][i] = '.';
```

由于每个位置只遍历一次，所以在遍历每行后要将填入的字符Q进行修改，而上题中一维数组的每个位置被多次遍历，所以可以直接用更新的值覆盖掉之前的值



```c++
//枚举每个格子
#include <iostream>

using namespace std;

const int N = 20;
char c[N][N];
int n;
bool row[N] , col[N] , dg[N] , udg[N];   //表示每一列，对角线和反对角线是否已经被存放

void dfs(int x , int y , int sum)   //传入的三个参数分别是横坐标，纵坐标和已经放入Q的个数
{
    if(y == n)
        y = 0 , x ++;               //如果y = n，说明该行已经遍历完
    if(x == n)
    {
        if(sum == n)
        {
            for(int i = 0 ; i < n ; i ++)
                cout << c[i] << endl;
            cout << endl;
        }
        return;
    }
    
    //每个格子都有两个分支 放入Q / 不放入Q
    //不放入Q
    dfs(x , y + 1 , sum);
    
    //放入Q
    if(!row[x] && !col[y] && !dg[x + y] && !udg[n - x + y])
    {
        c[x][y] = 'Q';
        row[x] = col[y] = dg[x + y] = udg[n - x + y] = true;
        dfs(x , y + 1 , sum + 1);
        row[x] = col[y] = dg[x + y] = udg[n - x + y] = false;
        c[x][y] = '.';
    }
}

int main()
{
    cin >> n;
    for(int i = 0 ; i < n ; i ++)
        for(int j = 0 ; j < n ; j ++)
            c[i][j] = '.';
    
    dfs(0 , 0 , 0);
    
    return 0;
}
```

