# 2048 Next Greater Numerically Balanced Number 下一个更大的数值平衡数

__Description:__

An integer `x` is __numerically balanced__ if for every digit `d` in the number `x`, there are __exactly__ `d` occurrences of that digit in `x`.

Given an integer `n`, return _the __smallest numerically balanced__ number __strictly greater__ than_ `n`_._

__Example:__

Example 1:

```text
Input: n = 1
Output: 22
Explanation: 
22 is numerically balanced since:
```

- The digit 2 occurs 2 times.

It is also the smallest numerically balanced number strictly greater than 1.

Example 2:

```text
Input: n = 1000
Output: 1333
Explanation: 
1333 is numerically balanced since:
```

- The digit 1 occurs 1 time.
- The digit 3 occurs 3 times.

It is also the smallest numerically balanced number strictly greater than 1000.

Note that 1022 cannot be the answer because 0 appeared more than 0 times.

Example 3:

```text
Input: n = 3000
Output: 3133
Explanation: 
3133 is numerically balanced since:
```

- The digit 1 occurs 1 time.
- The digit 3 occurs 3 times.
It is also the smallest numerically balanced number strictly greater than 3000.

__Constraints:__

- `0 <= n <= 10 ^ 6`

__题目描述:__

如果整数  `x` 满足:对于每个数位 `d` ，这个数位 __恰好__ 在 `x` 中出现 `d` 次。那么整数 `x` 就是一个 __数值平衡数__ 。

给你一个整数 `n` ，请你返回 __严格大于__ `n` 的 __最小数值平衡数__ 。

__示例:__

示例 1：

```text
输入：n = 1
输出：22
解释：
22 是一个数值平衡数，因为：
```

- 数字 2 出现 2 次

这也是严格大于 1 的最小数值平衡数。

示例 2：

```text
输入：n = 1000
输出：1333
解释：
1333 是一个数值平衡数，因为：
```

- 数字 1 出现 1 次。
- 数字 3 出现 3 次。

这也是严格大于 1000 的最小数值平衡数。
注意，1022 不能作为本输入的答案，因为数字 0 的出现次数超过了 0 。

示例 3：

```text
输入：n = 3000
输出：3133
解释：
3133 是一个数值平衡数，因为：
```

- 数字 1 出现 1 次。
- 数字 3 出现 3 次。

这也是严格大于 3000 的最小数值平衡数。

__提示：__

- `0 <= n <= 10 ^ 6`

__思路:__

```text
1. 模拟
由于 n <= 10^6, 因此可以直接枚举所有的数值平衡数
最大的数值平衡数为 1224444
计算平衡数的时间复杂度为 O(logN), 因此总的时间复杂度为 O(NlogN)
时间复杂度为 O(NlogN), 空间复杂度为 O(1)
2. 打表 ➕ 二分查找
由于 n <= 10 ^ 6, 因此可以直接打表
计算得到所有的平衡数
然后使用二分查找找到第一个大于 n 的平衡数
时间复杂度为 O(logC), 空间复杂度为 O(C), 其中 C 为平衡数的个数, 本题约为 110
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int nextBeautifulNumber(int n) 
    {
        return *upper_bound(balance.begin(), balance.end(), n);
    }
private:
    const vector<int> balance {1, 22, 122, 212, 221, 333, 1333, 3133, 3313, 3331, 4444, 14444, 22333, 23233, 23323, 23332, 32233, 32323, 32332, 33223, 33232, 33322, 41444, 44144, 44414, 44441, 55555, 122333, 123233, 123323, 123332, 132233, 132323, 132332, 133223, 133232, 133322, 155555, 212333, 213233, 213323, 213332, 221333, 223133, 223313, 223331, 224444, 231233, 231323, 231332, 232133, 232313, 232331, 233123, 233132, 233213, 233231, 233312, 233321, 242444, 244244, 244424, 244442, 312233, 312323, 312332, 313223, 313232, 313322, 321233, 321323, 321332, 322133, 322313, 322331, 323123, 323132, 323213, 323231, 323312, 323321, 331223, 331232, 331322, 332123, 332132, 332213, 332231, 332312, 332321, 333122, 333212, 333221, 422444, 424244, 424424, 424442, 442244, 442424, 442442, 444224, 444242, 444422, 515555, 551555, 555155, 555515, 555551, 666666, 1224444};
};
```

__Java__:

```Java
class Solution {
    public int nextBeautifulNumber(int n) {
        for (int i = n + 1; i <= 1224444; i++) if (check(i)) return i;
        return -1;
    }

    private boolean check(int x) {
        int[] count = new int[10];
        while (x > 0) {
            ++count[x % 10];
            x /= 10;
        }
        for (int d = 0; d < 10; d++) if (count[d] > 0 && count[d] != d) return false;
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def nextBeautifulNumber(self, n: int) -> int:
        return (d := [1, 22, 122, 212, 221, 333, 1333, 3133, 3313, 3331, 4444, 14444, 22333, 23233, 23323, 23332, 32233, 32323, 32332, 33223, 33232, 33322, 41444, 44144, 44414, 44441, 55555, 122333, 123233, 123323, 123332, 132233, 132323, 132332, 133223, 133232, 133322, 155555, 212333, 213233, 213323, 213332, 221333, 223133, 223313, 223331, 224444, 231233, 231323, 231332, 232133, 232313, 232331, 233123, 233132, 233213, 233231, 233312, 233321, 242444, 244244, 244424, 244442, 312233, 312323, 312332, 313223, 313232, 313322, 321233, 321323, 321332, 322133, 322313, 322331, 323123, 323132, 323213, 323231, 323312, 323321, 331223, 331232, 331322, 332123, 332132, 332213, 332231, 332312, 332321, 333122, 333212, 333221, 422444, 424244, 424424, 424442, 442244, 442424, 442442, 444224, 444242, 444422, 515555, 551555, 555155, 555515, 555551, 666666, 1224444])[bisect_right(d, n)]
```
