# 第二周学习任务（2024-10-28 ~ 2024-11-03）

## 任务介绍
- **描述**: 自主学习链表，包括但不限于单链表、循环链表、双向链表、链表的反转、链表的合并、链表的排序等
- **截止日期**: 2024-11-10
- **状态**: 进行中
- **检查方式**: 不检查
- **备注**: 可以去学习一下用数组模拟链表操作的实现
## 任务介绍
- **描述**: 完成以下链表有关算法题
- **题目**: [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)
- **题目**: [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/description/)
- **题目**: [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/description/)
- **题目**: [Hey！Vergil！](http://xdzn.club/problem/2331)
- **截止日期**: 2024-11-03
- **状态**: 已结束
- **检查方式**: 提交题解至学长学姐处，我们会认真检查
- **备注**: 请在提交题解前先阅读**提交要求**，并确保提交的代码通过了**所有**测试用例
- **备注**: 所有题目请用链表的方式实现，不允许**完全使用**数组等数据结构
## 提交要求
- **描述**: 提交的题解为md格式，并且至少包括题目链接，题目描述，代码实现以及你的反思与收获
## 格式举例
### 一：结绳计数

### 题目链接：[https://www.wobuzhidaolaiziyunali/]

#### 题目描述：

##### 现在你穿越到了很久以前，发现这里的人在通过结绳交流，你希望找到回到现代的方法，于是开始学习结绳，通过不懈的努力，你发现只需要将一根长为正整数 n 的绳子分为若干节，每节长度均为正整数，必须要分为m（m>1）节，只要找到每节绳子长度的最大乘积，就可以返回现代。

#### 输入描述

输入一个正整数n（2<=n<=1000）

#### 输出描述

输出每节绳子的最大乘积，答案需要取模 1e9+7（1000000007），比如计算初始结果为：1000000008，输出1。

#### 用例输入 1

```
12
```

#### 用例输出 1

```
81
```

### 解题代码：

```java
import java.math.BigInteger;
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int n = scanner.nextInt();
        
        BigInteger[] dp = new BigInteger[1005];
        for (int i = 0; i < 1005; i++) {
            dp[i] = BigInteger.ZERO;
        }
        dp[2] = BigInteger.ONE;
        dp[3] = BigInteger.valueOf(2);
        for (int i = 4; i <= n; i++) {
            for (int j = 1; j <= i; j++) {
                BigInteger product1 = BigInteger.valueOf(j).multiply(BigInteger.valueOf(i - j));
                BigInteger product2 = BigInteger.valueOf(j).multiply(dp[i - j]);
                
                dp[i] = dp[i].max(product1).max(product2);
            }
        }
        
        System.out.println(dp[n].mod(BigInteger.valueOf(1000000007)));
    }
}
```

### 解题思路：

##### 很标准的dp，这里的dp[i]表示绳子长为i的情况下可拆成的最大乘积。我们这里先对数组的前几种情况进行初始化，然后，对于后面的任意数据x，就遍历1到它自身的每一种拆分结果，用遍历过程中的值i去乘上dp[x-i]即长度为x-i的绳子的最大乘积就可以获得一个数值，再让该数字与x-i与i的直接乘积比较获得最大值，即为该状态下dp的理想值。

#### 问题与反思：

##### 1.最典型的就是即使是long long也无法存下过程中的数据，不过我猜测可能在过程中可以优化取余，这里就直接用Java来解决这个问题了。

##### 2.之前写代码没过，主要问题是在遍历过程中，不是`dp[j]*dp[i-j]`，而应该是 `j*dp[i-j]`，道理很简单，对与dp[3]，它的值应该是2，而不是3，因为题目要求绳子必须至少被分割一次，而对于大于3的绳子，它可以切割出三这个数据，而为了保持3的分割乘积最大，那么就应该是3，而不是2。
