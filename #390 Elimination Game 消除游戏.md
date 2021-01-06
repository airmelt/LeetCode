# 390 Elimination Game 消除游戏

__Description__:
There is a list of sorted integers from 1 to n. Starting from left to right, remove the first number and every other number afterward until you reach the end of the list.

Repeat the previous step again, but this time from right to left, remove the right most number and every other number from the remaining numbers.

We keep repeating the steps again, alternating left to right and right to left, until a single number remains.

Find the last number that remains starting with a list of length n.

__Example:__

Input:
n = 9,

~~1~~ 2 ~~3~~ 4 ~~5~~ 6 ~~7~~ 8 ~~9~~

2 ~~4~~ 6 ~~8~~

~~2~~ 6

6

Output:
6

__题目描述__:
给定一个从1 到 n 排序的整数列表。
首先，从左到右，从第一个数字开始，每隔一个数字进行删除，直到列表的末尾。
第二步，在剩下的数字中，从右到左，从倒数第一个数字开始，每隔一个数字进行删除，直到列表开头。
我们不断重复这两步，从左到右和从右到左交替进行，直到只剩下一个数字。
返回长度为 n 的列表中，最后剩下的数字。

__示例 :__

输入:
n = 9,

~~1~~ 2 ~~3~~ 4 ~~5~~ 6 ~~7~~ 8 ~~9~~

2 ~~4~~ 6 ~~8~~

~~2~~ 6

6

输出:
6

__思路__:

~~1~~ 2 ~~3~~ 4 ~~5~~ ... 2k ~~2k + 1~~

  2   4   ... 2k

  k  k-1  ...  1

假设删除后留下的数为 f(k)
第一行为 f(2k)或者 f(2k + 1)
第二行为第一步去掉 k个数之后, 对应起来应该像上图这样, 因为第二次是从右往左删除
第三行为 f(k)
可以找到规律, f(2k) + 2 \* f(k) = 2k + 2
即 f(2k) = 2 \* (k + 1 - f(k))
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int lastRemaining(int n) 
    {
        return n == 1 ? 1 : 2 * ((n >> 1) + 1 - lastRemaining(n >> 1));
    }
};
```

__Java__:

```Java
class Solution {
    public int lastRemaining(int n) {
        return n == 1 ? 1 : 2 * ((n >> 1) + 1 - lastRemaining(n >> 1));
    }
}
```

__Python__:

```Python
class Solution:
    def lastRemaining(self, n: int) -> int:
        return 1 if n == 1 else 2 * ((n >> 1) + 1 - self.lastRemaining(n >> 1))
```
