# 2167 Minimum Time to Remove All Cars Containing Illegal Goods 移除所有载有违禁货物车厢所需的最少时间

__Description:__

You are given a __0-indexed__ binary string `s` which represents a sequence of train cars. `s[i] = '0'` denotes that the `i ^ th` car does __not__ contain illegal goods and `s[i] = '1'` denotes that the `i ^ th` car does contain illegal goods.

As the train conductor, you would like to get rid of all the cars containing illegal goods. You can do any of the following three operations any number of times:

Return the minimum time to remove all the cars containing illegal goods.

Note that an empty sequence of cars is considered to have no cars containing illegal goods.

__Example:__

Example 1:

```text
Input: s = "1100101"
Output: 5
Explanation: 
```

One way to remove all the cars containing illegal goods from the sequence is to

- remove a car from the left end 2 times. Time taken is 2 * 1 = 2.
- remove a car from the right end. Time taken is 1.
- remove the car containing illegal goods found in the middle. Time taken is 2.

This obtains a total time of 2 + 1 + 2 = 5.

An alternative way is to

- remove a car from the left end 2 times. Time taken is 2 * 1 = 2.
- remove a car from the right end 3 times. Time taken is 3 * 1 = 3.

This also obtains a total time of 2 + 3 = 5.

5 is the minimum time taken to remove all the cars containing illegal goods.
There are no other ways to remove them with less time.

Example 2:

```text
Input: s = "0010"
Output: 2
Explanation:
```

One way to remove all the cars containing illegal goods from the sequence is to

- remove a car from the left end 3 times. Time taken is 3 * 1 = 3.

This obtains a total time of 3.

Another way to remove all the cars containing illegal goods from the sequence is to

- remove the car containing illegal goods found in the middle. Time taken is 2.

This obtains a total time of 2.

Another way to remove all the cars containing illegal goods from the sequence is to

- remove a car from the right end 2 times. Time taken is 2 * 1 = 2.

This obtains a total time of 2.

2 is the minimum time taken to remove all the cars containing illegal goods.
There are no other ways to remove them with less time.

__Constraints:__

- `1 <= s.length <= 2 * 10 ^ 5`
- `s[i]` is either `'0'` or `'1'`.

__题目描述:__

给你一个下标从 __0__ 开始的二进制字符串 `s` ，表示一个列车车厢序列。 `s[i] = '0'` 表示第 `i` 节车厢 __不__ 含违禁货物，而 `s[i] = '1'` 表示第 `i` 节车厢含违禁货物。

作为列车长，你需要清理掉所有载有违禁货物的车厢。你可以不限次数执行下述三种操作中的任意一个：

返回移除所有载有违禁货物车厢所需要的 最少 单位时间数。

注意，空的列车车厢序列视为没有车厢含违禁货物。

__示例:__

示例 1：

```text
输入：s = "1100101"
输出：5
解释：
```

一种从序列中移除所有载有违禁货物的车厢的方法是：

- 从左端移除一节车厢 2 次。所用时间是 2 * 1 = 2 。
- 从右端移除一节车厢 1 次。所用时间是 1 。
- 移除序列中间位置载有违禁货物的车厢。所用时间是 2 。

总时间是 2 + 1 + 2 = 5 。

一种替代方法是：

- 从左端移除一节车厢 2 次。所用时间是 2 * 1 = 2 。
- 从右端移除一节车厢 3 次。所用时间是 3 * 1 = 3 。

总时间也是 2 + 3 = 5 。

5 是移除所有载有违禁货物的车厢所需要的最少单位时间数。
没有其他方法能够用更少的时间移除这些车厢。

示例 2：

```text
输入：s = "0010"
输出：2
解释：
```

一种从序列中移除所有载有违禁货物的车厢的方法是：

- 从左端移除一节车厢 3 次。所用时间是 3 * 1 = 3 。

总时间是 3.

另一种从序列中移除所有载有违禁货物的车厢的方法是：

- 移除序列中间位置载有违禁货物的车厢。所用时间是 2 。

总时间是 2.

另一种从序列中移除所有载有违禁货物的车厢的方法是：

- 从右端移除一节车厢 2 次。所用时间是 2 * 1 = 2 。

总时间是 2.

2 是移除所有载有违禁货物的车厢所需要的最少单位时间数。
没有其他方法能够用更少的时间移除这些车厢。

__提示：__

- `1 <= s.length <= 2 * 10 ^ 5`
- `s[i]` 为 `'0'` 或 `'1'`

__思路:__

```text
前缀和
pre[i] 表示从前面移除 i 个车厢所需的时间
如果当前车厢为 1, 则 pre[i] = min(pre[i - 1] + 2, i)
否则 pre[i] = pre[i - 1]
suf[i] 表示从后面移除 i 个车厢所需的时间
如果当前车厢为 1, 则 suf[i] = min(suf[i + 1] + 2, n - i)
否则 suf[i] = suf[i + 1]
计算所有的 pre 和 suf 取 min(pre[i] + suf[i + 1]) 即可
优化空间复杂度可以先计算 suf 再计算 pre 的同时遍历 s, 这样可以使用滚动数组只使用一个变量 pre 来计算
另外可以把移除中间的车厢都加入到 pre 中
suf 则是直接移除所有后面的车厢即 n - i - 1
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int minimumTime(string s) 
    {
        int n = s.size(), result = n, pre = 0;
        for (int i = 0; i < n; i++) 
        {
            if (s[i] == '1') pre = min(pre + 2, i + 1);
            result = min(result, n - 1 - i + pre);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minimumTime(String s) {
        int n = s.length(), result = n, pre = 0;
        for (int i = 0; i < n; i++) {
            if (s.charAt(i) == '1') pre = Math.min(pre + 2, i + 1);
            result = Math.min(result, n - 1 - i + pre);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minimumTime(self, s: str) -> int:
        result, n, pre = len(s), len(s), 0
        for i, c in enumerate(s):
            pre = min(pre + 2, i + 1) if c == '1' else pre
            result = min(result, pre + n - 1 - i)
        return result
```
