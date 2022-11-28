# 1399 Count Largest Group 统计最大组的数目

__Description:__

You are given an integer n.

Each number from 1 to n is grouped according to the sum of its digits.

Return the number of groups that have the largest size.

__Example:__

Example 1:

Input: n = 13
Output: 4
Explanation: There are 9 groups in total, they are grouped according sum of its digits of numbers from 1 to 13:
[1,10], [2,11], [3,12], [4,13], [5], [6], [7], [8], [9].
There are 4 groups with largest size.

Example 2:

Input: n = 2
Output: 2
Explanation: There are 2 groups [1], [2] of size 1.

__Constraints:__

1 <= n <= 10^4

__题目描述:__

给你一个整数 n 。请你先求出从 1 到 n 的每个整数 10 进制表示下的数位和（每一位上的数字相加），然后把数位和相等的数字放到同一个组中。

请你统计每个组中的数字数目，并返回数字数目并列最多的组有多少个。

__示例:__

示例 1：

输入：n = 13
输出：4
解释：总共有 9 个组，将 1 到 13 按数位求和后这些组分别是：
[1,10]，[2,11]，[3,12]，[4,13]，[5]，[6]，[7]，[8]，[9]。总共有 4 个组拥有的数字并列最多。

示例 2：

输入：n = 2
输出：2
解释：总共有 2 个大小为 1 的组 [1]，[2]。

示例 3：

输入：n = 15
输出：6

示例 4：

输入：n = 24
输出：5

__提示：__

1 <= n <= 10^4

__思路:__

```text
1. 哈希表
统计数位和出现的次数
找到数位和出现次数最多的值
遍历哈希表, 统计上述值出现的次数
计算数位和需要 O(lgN) 时间
时间复杂度为 O(NlgN), 空间复杂度为 O(lgN)
2. 数组模拟
由于 N <= 10 ^ 4
数位和最多为 9999 -> 36
只需要一个长度为 36 的数组就能统计所有元素的数位和
比哈希表在常数时间上略有优势
另外通过将数组和的计算分散为 
sum[i] = sum[i / 10] + i % 10
可以以空间换时间进一步缩小时间复杂度
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countLargestGroup(int n) 
    {
        vector<int> count(40), record(n + 1);
        for (int i = 1; i <= n; i++)
        {
            record[i] = record[i / 10] + i % 10;
            ++count[record[i]];
        }
        int result = 0, max_value = *max_element(count.begin(), count.end());
        for (int i : count) result += (i == max_value);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countLargestGroup(int n) {
        int result = 0, maxValue = 0, count[] = new int[40];
        for (int i = 0; i < n; i++) ++count[helper(i + 1)];
        for (int value : count) maxValue = Math.max(maxValue, value);
        for (int value : count) if (value == maxValue) ++result;
        return result;
    }
    
    private int helper(int n) {
        int result = 0;
        while (n > 0) {
            result += n % 10;
            n /= 10;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countLargestGroup(self, n: int) -> int:
        return max(Counter((Counter(map(lambda x: sum(map(int, str(x + 1))), range(n))).values())).items())[1]
```
