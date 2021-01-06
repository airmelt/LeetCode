# 335 Self Crossing 路径交叉

__Description__:
You are given an array x of n positive numbers. You start at point (0,0) and moves x[0] metres to the north, then x[1] metres to the west, x[2] metres to the south, x[3] metres to the east and so on. In other words, after each move your direction changes counter-clockwise.

Write a one-pass algorithm with O(1) extra space to determine, if your path crosses itself, or not.

__Example:__

Example 1:

```text
┌───┐
│   │
└───┼──>
    │
```

Input: [2,1,1,2]
Output: true

Example 2:

```text
┌──────┐
│      │
│
│
└────────────>
```

Input: [1,2,3,4]
Output: false

Example 3:

```text
┌───┐
│   │
└───┼>
```

Input: [1,1,1,1]
Output: true

__题目描述__:
给定一个含有 n 个正数的数组 x。从点 (0,0) 开始，先向北移动 x[0] 米，然后向西移动 x[1] 米，向南移动 x[2] 米，向东移动 x[3] 米，持续移动。也就是说，每次移动后你的方位会发生逆时针变化。

编写一个 O(1) 空间复杂度的一趟扫描算法，判断你所经过的路径是否相交。

__示例 :__

示例 1:

```text
┌───┐
│   │
└───┼──>
    │
```

输入: [2,1,1,2]
输出: true

示例 2:

```text
┌──────┐
│      │
│
│
└────────────>
```

输入: [1,2,3,4]
输出: false

示例 3:

```text
┌───┐
│   │
└───┼>
```

输入: [1,1,1,1]
输出: true

__思路__:

1. 只有 3条边或者以下不可能相交
2. 当边为 4时只有一种情况会相交, 就是示例 1的样子, 这时满足 x[i] >= x[i - 2] && x[i - 3] >= x[i - 1]
3. 当边为 5时, 可以考虑当边为 4且不相交, 加上第 5条边才相交, 其中有几种其实和 2中的有重复, 只有一种例外, 就是第 1条边和第 5条边共线时, 这时满足  x[i - 1] == x[i - 3] && x[i] >= x[i - 2] - x[i - 4
4. 当边为 6时, 只有第 1条边和第 6条边相交是新出现的情况, 其余情况都可以归结到 23中的情况, 这时满足 x[i - 2] >= x[i - 4] && x[i - 3] >= x[i - 1] && x[i - 1] >= x[i - 3] - x[i - 5] && x[i] >= x[i - 2] - x[i - 4]
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool isSelfCrossing(vector<int>& x) 
    {
        for (int i = 3; i < x.size(); i++) if ((x[i] >= x[i - 2] and x[i - 3] >= x[i - 1]) or (i >= 4 and x[i - 1] == x[i - 3] and x[i] >= x[i - 2] - x[i - 4]) or (i >= 5 and x[i - 2] >= x[i - 4] and x[i - 3] >= x[i - 1] and x[i - 1] >= x[i - 3] - x[i - 5] and x[i] >= x[i - 2] - x[i - 4])) return true;
        return false;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean isSelfCrossing(int[] x) {
        for (int i = 3; i < x.length; i++) if ((x[i] >= x[i - 2] && x[i - 3] >= x[i - 1]) || (i >= 4 && x[i - 1] == x[i - 3] && x[i] >= x[i - 2] - x[i - 4]) || (i >= 5 && x[i - 2] >= x[i - 4] && x[i - 3] >= x[i - 1] && x[i - 1] >= x[i - 3] - x[i - 5] && x[i] >= x[i - 2] - x[i - 4])) return true;
        return false;
    }
}
```

__Python__:

```Python
class Solution:
    def isSelfCrossing(self, x: List[int]) -> bool:
        return len(x) >= 4 and any(((x[i] >= x[i - 2] and x[i - 3] >= x[i - 1]) or (i >= 4 and x[i - 1] == x[i - 3] and x[i] >= x[i - 2] - x[i - 4]) or (i >= 5 and x[i - 2] >= x[i - 4] and x[i - 3] >= x[i - 1] and x[i - 1] >= x[i - 3] - x[i - 5] and x[i] >= x[i - 2] - x[i - 4])) for i in range(3, len(x)))
```
