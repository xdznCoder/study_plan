## 优先队列 priority_queue

* 简介

  优先队列是一种**容器适配器**，其底部是由堆构成的队列，优先队列的进出顺序由**权**来决定，优先队列会根据当前队列权值的状态进行排序，使得权值最大（或最小）的永远排在前面。

  优先队列不提供迭代器，只能访问头元素

  ### 优先队列的初始化

  ```c++
  #include <queue>
  priority_queue<T, Container, Compare>
  priority_queue<T>        //默认是一个最大堆,容器为vector，最大的元素始终在最前面
  ```

  - `empty()`: 检查队列是否为空。队列为空时为`true`
  - `size()`: 返回队列中的元素数量。
  - `top()`: 返回队列顶部的元素（不删除它）。
  - `push()`: 向队列添加一个元素。
  - `pop()`: 移除队列顶部的元素。

  ### 自定义优先级

  如果需要一个最小堆，我们可以使用**结构体**来重载一个操作符

  ```
  struct compare {
      bool operator()(int a, int b) {
          return a > b; // 定义最小堆
      }
  };
  
  int main() {
      // 创建一个自定义类型的优先队列，使用最小堆，最小的数在前面
      std::priority_queue<int, std::vector<int>, compare> pq_min;
  }
  ```

  也可以这样定义：

  ```c++
  struct Num
  {
  	long var;
  	bool operator<(const Num& b) const
  	{
  		return var < b.var;
  	}
  };
  
  std::priority_queue<Num> nums; // 定义一个最大堆
  ```
  
  或者使用Lambda表达式定义
  
  ```
  auto cmp = [&](const int& u, const int& v) {
              return nums[u][next[u]] > nums[v][next[v]]; // 最小堆
          };
          // decltype 返回 cmp 的类型
          priority_queue<int, vector<int>, decltype(cmp)> pq(cmp);
  ```
  
  如果没有使用struct定义的类型，可以这样定义
  
  ```
  priority_queue<T, vector<T>, greater<T>> pq; // 最小堆
  ```
  
  
  
  ### 应用
  
  优先队列通常被用于在滑动窗口中寻找最值，或者是A*算法等。

## 例题

有一个长为 $n$ 的序列 $a$，以及一个大小为 $k$ 的窗口。现在这个从左边开始向右滑动，每次滑动一个单位，求出每次滑动后窗口中的最大值和最小值。

例如，对于序列 $[1,3,-1,-3,5,3,6,7]$ 以及 $k = 3$，有如下过程：

$$\def\arraystretch{1.2}
\begin{array}{|c|c|c|}\hline
\textsf{窗口位置} & \textsf{最小值} & \textsf{最大值} \\ \hline
\verb![1   3  -1] -3   5   3   6   7 ! & -1 & 3 \\ \hline
\verb! 1  [3  -1  -3]  5   3   6   7 ! & -3 & 3 \\ \hline
\verb! 1   3 [-1  -3   5]  3   6   7 ! & -3 & 5 \\ \hline
\verb! 1   3  -1 [-3   5   3]  6   7 ! & -3 & 5 \\ \hline
\verb! 1   3  -1  -3  [5   3   6]  7 ! & 3 & 6 \\ \hline
\verb! 1   3  -1  -3   5  [3   6   7]! & 3 & 7 \\ \hline
\end{array}
$$

### 输入格式

输入一共有两行，第一行有两个正整数 $n,k$。
第二行 $n$ 个整数，表示序列 $a$

### 输出格式

输出共两行，第一行为每次窗口滑动的最小值   
第二行为每次窗口滑动的最大值

### 样例 #1

#### 样例输入 #1

```
8 3
1 3 -1 -3 5 3 6 7
```

#### 样例输出 #1

```
-1 -3 -3 -3 3 3
3 3 5 5 6 7
```

#### 提示

【数据范围】    
对于 $50\%$ 的数据，$1 \le n \le 10^5$；  
对于 $100\%$ 的数据，$1\le k \le n \le 10^6$，$a_i \in [-2^{31},2^{31})$。

### 示例代码

```c++
#include<bits/stdc++.h>
using namespace std;

struct num1
{
	int xu, zhi;
	bool operator <(const num1 x) const
	{
		return (zhi > x.zhi) || (zhi == x.zhi && xu < x.xu);
	}
} p1; 

struct num2
{
	int xu, zhi;
	bool operator <(const num2 x) const
	{
		return (zhi < x.zhi) || (zhi == x.zhi && xu < x.xu);
	}
} p2;

priority_queue<num1> q1; // 按升序排列（小为先）
priority_queue<num2> q2; // 按降序排列（大为先）

int n, k, top, a[1000001], ans1[1000001], ans2[1000001]; // 用于保存答案

int main()
{
	scanf("%d %d", &n, &k);
	for (int i = 1; i <= n; i++) scanf("%d", &a[i]);
	for (int i = 1; i <= k; i++)
	{
		p1.xu = i, p1.zhi = a[i];
		q1.push(p1);
		p2.xu = i, p2.zhi = a[i];
		q2.push(p2);
	}
	ans1[++top] = q1.top().zhi;
	ans2[top] = q2.top().zhi;
	for (int i = k + 1; i <= n; i++)
	{
		p1.xu = i, p1.zhi = a[i];
		q1.push(p1);
		p2.xu = i, p2.zhi = a[i];
		q2.push(p2); 
		while (i - q1.top().xu >= k) q1.pop();
		while (i - q2.top().xu >= k) q2.pop(); 
		ans1[++top] = q1.top().zhi;
		ans2[top] = q2.top().zhi;
	}
	for (int i = 1; i <= top; i++) 
		printf("%d ", ans1[i]);
	printf("\n");
	for (int i = 1; i <= top; i++) 
		printf("%d ", ans2[i]);
}
```



其他例题：[632. 最小区间 - 力扣（LeetCode）](https://leetcode.cn/problems/smallest-range-covering-elements-from-k-lists/description/)

