## Huffman树

#### 148.合并果子（ACWing）

在一个果园里，达达已经将所有的果子打了下来，而且按果子的不同种类分成了不同的堆。

达达决定把所有的果子合成一堆。

每一次合并，达达可以把两堆果子合并到一起，消耗的体力等于两堆果子的重量之和。

可以看出，所有的果子经过 n−1n−1 次合并之后，就只剩下一堆了。

达达在合并果子时总共消耗的体力等于每次合并所耗体力之和。

因为还要花大力气把这些果子搬回家，所以达达在合并果子时要尽可能地节省体力。

假定每个果子重量都为 11，并且已知果子的种类数和每种果子的数目，你的任务是设计出合并的次序方案，使达达耗费的体力最少，并输出这个最小的体力耗费值。

例如有 33 种果子，数目依次为 1，2，91，2，9。

可以先将 1、21、2 堆合并，新堆数目为 33，耗费体力为 33。

接着，将新堆与原先的第三堆合并，又得到新的堆，数目为 1212，耗费体力为 1212。

所以达达总共耗费体力=3+12=15=3+12=15。

可以证明 1515 为最小的体力耗费值。

```c++
//这段代码时间复杂度太高
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 10010;
vector<int> a , b;
int sum = 0;

int main()
{
    int n;
    cin >> n;
    for(int i = 0 ; i < n ; i ++)
    {
        int num;
        scanf("%d" , &num);
        a.push_back(num);
    }
        
    sort(a.begin() , a.end());
    
    
    for(int k = 0 ; k < n - 1 ; k ++)
    {
        b.push_back(a[0] + a[1]);
        a.push_back(a[0] + a[1]);
        a.erase(a.begin());
        a.erase(a.begin());
        sort(a.begin() , a.end());
    }
    
    for(int i = 0 ; i < b.size() ; i ++)
        sum += b[i];
    cout << sum << endl;
    
    return 0;
}
```



```c++
//堆
#include <iostream>
#include <algorithm>
#include <queue>

using namespace std;

int main()
{
    int n;
    cin >> n;
    
    priority_queue<int , vector<int> , greater<int>> heap;
    
    while(n --)
    {
        int x;
        cin >> x;
        heap.push(x);
    }
    
    int ans = 0;
    while(heap.size() > 1)
    {
        int a = heap.top(); heap.pop();
        int b = heap.top(); heap.pop();
        heap.push(a + b);
        ans += a + b;
    }
    cout << ans << endl;
    return 0;
}
```

1. 用到了小根堆

   ```c++
   priority_queue<int , vector<int> , greater<int>> heap;//小根堆
   priority_queue<int>//默认是大根堆，大的元素会放在前面
```
   
   用**priority_queue**实现小根堆的方法如下
   
   1. 将所有元素取负
   2. 直接用STL中的小根堆，如上代码

2. > priority_queue允许用户为[队列](https://so.csdn.net/so/search?q=队列&spm=1001.2101.3001.7020)中元素设置优先级，放置元素的时候不是直接放到队尾，而是放置到比它优先级低的元素前面，标准库默认使用<操作符来确定优先级关系。

