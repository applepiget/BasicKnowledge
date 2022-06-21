## Trie树

**用于快速存储和查找字符串集合**

trie树的根结点为空结点，如图：

![trie树](D:\笔记\数据结构\思维导图\trie树.png)

```c++
#include <iostream>
#include <cstdio>

using namespace std;

const int N = 100010;
char s[N];
int son[N][26] , count[N] , idx;    //0既是根结点也是空结点

void  insert(char str[])
{
    int p = 0;
    for(int i = 0 ; str[i] ; i ++)
    {
        int u = str[i] - 'a';   
        if(!son[p][u])              //如果不存在这个字母，则加入这个字母
            son[p][u] = ++ idx;
        p = son[p][u];              //将p移动到下一个字母
    }
    count[p] ++;                    //遍历结束后记录最后一个字母，代表以该字母为结尾的字符									串+1
}

int query(char str[])
{
    int p = 0;
    for(int i = 0 ; str[i] ; i ++)
    {
        int u = str[i] - 'a';
        if(!son[p][u])				//如果不存在当前遍历到的字母，返回0
            return 0;
        p = son[p][u];
    }
    return count[p];		
}

int main()
{
    int n;
    cin >> n;
    while(n --)
    {
        char q;
        cin >> q;
        scanf("%s" , s);
        
        if(q == 'I')
            insert(s);
        else
            cout << query(s) << endl;
    }
    return 0;
}
```

