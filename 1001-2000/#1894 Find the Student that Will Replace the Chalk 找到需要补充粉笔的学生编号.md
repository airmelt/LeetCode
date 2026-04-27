# 1894 Find the Student that Will Replace the Chalk 找到需要补充粉笔的学生编号

__Description:__

There are `n` students in a class numbered from `0` to `n - 1`. The teacher will give each student a problem starting with the student number `0`, then the student number `1`, and so on until the teacher reaches the student number `n - 1`. After that, the teacher will restart the process, starting with the student number `0` again.

You are given a __0-indexed__ integer array `chalk` and an integer `k`. There are initially `k` pieces of chalk. When the student number `i` is given a problem to solve, they will use `chalk[i]` pieces of chalk to solve that problem. However, if the current number of chalk pieces is __strictly less__ than `chalk[i]`, then the student number `i` will be asked to __replace__ the chalk.

Return the index of the student that will replace the chalk pieces.

__Example:__

Example 1:

```text
Input: chalk = [5,1,5], k = 22
Output: 0
Explanation: The students go in turns as follows:
- Student number 0 uses 5 chalk, so k = 17.
- Student number 1 uses 1 chalk, so k = 16.
- Student number 2 uses 5 chalk, so k = 11.
- Student number 0 uses 5 chalk, so k = 6.
- Student number 1 uses 1 chalk, so k = 5.
- Student number 2 uses 5 chalk, so k = 0.
Student number 0 does not have enough chalk, so they will have to replace it.
```

Example 2:

```text
Input: chalk = [3,4,1,2], k = 25
Output: 1
Explanation: The students go in turns as follows:
- Student number 0 uses 3 chalk so k = 22.
- Student number 1 uses 4 chalk so k = 18.
- Student number 2 uses 1 chalk so k = 17.
- Student number 3 uses 2 chalk so k = 15.
- Student number 0 uses 3 chalk so k = 12.
- Student number 1 uses 4 chalk so k = 8.
- Student number 2 uses 1 chalk so k = 7.
- Student number 3 uses 2 chalk so k = 5.
- Student number 0 uses 3 chalk so k = 2.
Student number 1 does not have enough chalk, so they will have to replace it.
```

__Constraints:__

- `chalk.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= chalk[i] <= 10 ^ 5`
- `1 <= k <= 10 ^ 9`

__题目描述:__

一个班级里有 `n` 个学生，编号为 `0` 到 `n - 1` 。每个学生会依次回答问题，编号为 `0` 的学生先回答，然后是编号为 `1` 的学生，以此类推，直到编号为 `n - 1` 的学生，然后老师会重复这个过程，重新从编号为 `0` 的学生开始回答问题。

给你一个长度为 `n` 且下标从 `0` 开始的整数数组 `chalk` 和一个整数 `k` 。一开始粉笔盒里总共有 `k` 支粉笔。当编号为 `i` 的学生回答问题时，他会消耗 `chalk[i]` 支粉笔。如果剩余粉笔数量 __严格小于__ `chalk[i]` ，那么学生 `i` 需要 __补充__ 粉笔。

请你返回需要 补充 粉笔的学生 编号 。

__示例:__

示例 1：

```text
输入：chalk = [5,1,5], k = 22
输出：0
解释：学生消耗粉笔情况如下：
- 编号为 0 的学生使用 5 支粉笔，然后 k = 17 。
- 编号为 1 的学生使用 1 支粉笔，然后 k = 16 。
- 编号为 2 的学生使用 5 支粉笔，然后 k = 11 。
- 编号为 0 的学生使用 5 支粉笔，然后 k = 6 。
- 编号为 1 的学生使用 1 支粉笔，然后 k = 5 。
- 编号为 2 的学生使用 5 支粉笔，然后 k = 0 。
编号为 0 的学生没有足够的粉笔，所以他需要补充粉笔。
```

示例 2：

```text
输入：chalk = [3,4,1,2], k = 25
输出：1
解释：学生消耗粉笔情况如下：
- 编号为 0 的学生使用 3 支粉笔，然后 k = 22 。
- 编号为 1 的学生使用 4 支粉笔，然后 k = 18 。
- 编号为 2 的学生使用 1 支粉笔，然后 k = 17 。
- 编号为 3 的学生使用 2 支粉笔，然后 k = 15 。
- 编号为 0 的学生使用 3 支粉笔，然后 k = 12 。
- 编号为 1 的学生使用 4 支粉笔，然后 k = 8 。
- 编号为 2 的学生使用 1 支粉笔，然后 k = 7 。
- 编号为 3 的学生使用 2 支粉笔，然后 k = 5 。
- 编号为 0 的学生使用 3 支粉笔，然后 k = 2 。
编号为 1 的学生没有足够的粉笔，所以他需要补充粉笔。
```

__提示：__

- `chalk.length == n`
- `1 <= n <= 10 ^ 5`
- `1 <= chalk[i] <= 10 ^ 5`
- `1 <= k <= 10 ^ 9`

__思路:__

```text
前缀和 ➕ 二分查找
先求出数组的前缀和 pre, 然后对 k 用 pre 的最后一个数, 即数组的和取模
再对 pre 进行二分查找, 找到第一个大于等于 k 的数的下标, 取其前一个数的下标即可
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int chalkReplacer(vector<int>& chalk, int k) 
    {
        int n = chalk.size(), left = 0, right = n;
        vector<long long> pre(n + 1);
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + chalk[i];
        k %= pre[n];
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1);
            if (pre[mid] > k) right = mid;
            else left = mid + 1;
        }
        return left - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int chalkReplacer(int[] chalk, int k) {
        int n = chalk.length, left = 0, right = n;
        long[] pre = new long[n + 1];
        for (int i = 0; i < n; i++) pre[i + 1] = pre[i] + chalk[i];
        k %= pre[n];
        while (left < right) {
            int mid = left + ((right - left) >> 1);
            if (pre[mid] > k) right = mid;
            else left = mid + 1;
        }
        return left - 1;
    }
}
```

__Python__:

```Python
class Solution:
    def chalkReplacer(self, chalk: List[int], k: int) -> int:
        return bisect_right(list(accumulate(chalk)), k % sum(chalk))
```
