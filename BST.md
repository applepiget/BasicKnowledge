## 二叉搜索树(BST)

```c++
#include <iostream>

using namespace std;

const int N = 110;
int l[N] , r[N] , w[N] , idx;
int root;

void insert(int &u , int x)         //用&是为了直接修改root的值，方便下一次插入
{
    if(u == 0)
    {
        u = ++ idx;
        w[u] = x;
    }
    else if(x > w[u])
        insert(r[u] , x);
    else if(x < w[u])
        insert(l[u] , x);
}

void pre(int u)
{
    if(u)
    {
        cout << w[u] << ' ';
        pre(l[u]);
        pre(r[u]);
    }
}

void in(int u)
{
    if(u)
    {
        in(l[u]);
        cout << w[u] << ' ';
        in(r[u]);
    }
}

void post(int u)
{
    if(u)
    {
        post(l[u]);
        post(r[u]);
        cout << w[u] << ' ';
    }
}

int main()
{
    int n;
    cin >> n;
    while(n --)
    {
        int x;
        cin >> x;
        insert(root , x);
    }
    
    pre(root);
    cout << endl;
    in(root);
    cout << endl;
    post(root);
    
    return 0;    
}
```

