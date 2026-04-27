# 2281 Sum of Total Strength of Wizards 巫师的总力量和

__Description:__

As the ruler of a kingdom, you have an army of wizards at your command.

You are given a __0-indexed__ integer array `strength`, where `strength[i]` denotes the strength of the `i ^ th` wizard. For a __contiguous__ group of wizards (i.e. the wizards' strengths form a __subarray__ of `strength`), the __total strength__ is defined as the __product__ of the following two values:

- The strength of the __weakest__ wizard in the group.
- The __total__ of all the individual strengths of the wizards in the group.

Return _the __sum__ of the total strengths of __all__ contiguous groups of wizards_. Since the answer may be very large, return it __modulo__ `10 ^ 9 + 7`.

A subarray is a contiguous non-empty sequence of elements within an array.

__Example:__

Example 1:

```text
Input: strength = [1,3,1,2]
Output: 44
Explanation: The following are all the contiguous groups of wizards:
```

- [1] from [1,3,1,2] has a total strength of min([1]) \* sum([1]) = 1 \* 1 = 1
- [3] from [1,3,1,2] has a total strength of min([3]) \* sum([3]) = 3 \* 3 = 9
- [1] from [1,3,1,2] has a total strength of min([1]) \* sum([1]) = 1 \* 1 = 1
- [2] from [1,3,1,2] has a total strength of min([2]) \* sum([2]) = 2 \* 2 = 4
- [1,3] from [1,3,1,2] has a total strength of min([1,3]) \* sum([1,3]) = 1 \* 4 = 4
- [3,1] from [1,3,1,2] has a total strength of min([3,1]) \* sum([3,1]) = 1 \* 4 = 4
- [1,2] from [1,3,1,2] has a total strength of min([1,2]) \* sum([1,2]) = 1 \* 3 = 3
- [1,3,1] from [1,3,1,2] has a total strength of min([1,3,1]) \* sum([1,3,1]) = 1 \* 5 = 5
- [3,1,2] from [1,3,1,2] has a total strength of min([3,1,2]) \* sum([3,1,2]) = 1 \* 6 = 6
- [1,3,1,2] from [1,3,1,2] has a total strength of min([1,3,1,2]) \* sum([1,3,1,2]) = 1 \* 7 = 7

The sum of all the total strengths is 1 + 9 + 1 + 4 + 4 + 4 + 3 + 5 + 6 + 7 = 44.

Example 2:

```text
Input: strength = [5,4,6]
Output: 213
Explanation: The following are all the contiguous groups of wizards: 
```

- [5] from [5,4,6] has a total strength of min([5]) \* sum([5]) = 5 \* 5 = 25
- [4] from [5,4,6] has a total strength of min([4]) \* sum([4]) = 4 \* 4 = 16
- [6] from [5,4,6] has a total strength of min([6]) \* sum([6]) = 6 \* 6 = 36
- [5,4] from [5,4,6] has a total strength of min([5,4]) \* sum([5,4]) = 4 \* 9 = 36
- [4,6] from [5,4,6] has a total strength of min([4,6]) \* sum([4,6]) = 4 \* 10 = 40
- [5,4,6] from [5,4,6] has a total strength of min([5,4,6]) \* sum([5,4,6]) = 4 \* 15 = 60

The sum of all the total strengths is 25 + 16 + 36 + 36 + 40 + 60 = 213.

__Constraints:__

- `1 <= strength.length <= 10 ^ 5`
- `1 <= strength[i] <= 10 ^ 9`

__题目描述:__

作为国王的统治者，你有一支巫师军队听你指挥。

给你一个下标从 __0__ 开始的整数数组 `strength` ，其中 `strength[i]` 表示第 `i` 位巫师的力量值。对于连续的一组巫师（也就是这些巫师的力量值是 `strength` 的 __子数组__），__总力量__ 定义为以下两个值的 __乘积__ :

- 巫师中 __最弱__ 的能力值。
- 组中所有巫师的个人力量值 __之和__ 。

请你返回 __所有__ 巫师组的 __总__ 力量之和。由于答案可能很大，请将答案对 `10 ^ 9 + 7` __取余__ 后返回。

子数组 是一个数组里 非空 连续子序列。

__示例:__

示例 1：

```text
输入：strength = [1,3,1,2]
输出：44
解释：以下是所有连续巫师组：
```

- [1,3,1,2] 中 [1] ，总力量值为 min([1]) \* sum([1]) = 1 \* 1 = 1
- [1,3,1,2] 中 [3] ，总力量值为 min([3]) \* sum([3]) = 3 \* 3 = 9
- [1,3,1,2] 中 [1] ，总力量值为 min([1]) \* sum([1]) = 1 \* 1 = 1
- [1,3,1,2] 中 [2] ，总力量值为 min([2]) \* sum([2]) = 2 \* 2 = 4
- [1,3,1,2] 中 [1,3] ，总力量值为 min([1,3]) \* sum([1,3]) = 1 \* 4 = 4
- [1,3,1,2] 中 [3,1] ，总力量值为 min([3,1]) \* sum([3,1]) = 1 \* 4 = 4
- [1,3,1,2] 中 [1,2] ，总力量值为 min([1,2]) \* sum([1,2]) = 1 \* 3 = 3
- [1,3,1,2] 中 [1,3,1] ，总力量值为 min([1,3,1]) \* sum([1,3,1]) = 1 \* 5 = 5
- [1,3,1,2] 中 [3,1,2] ，总力量值为 min([3,1,2]) \* sum([3,1,2]) = 1 \* 6 = 6
- [1,3,1,2] 中 [1,3,1,2] ，总力量值为 min([1,3,1,2]) \* sum([1,3,1,2]) = 1 \* 7 = 7

所有力量值之和为 1 + 9 + 1 + 4 + 4 + 4 + 3 + 5 + 6 + 7 = 44 。

示例 2：

```text
输入：strength = [5,4,6]
输出：213
解释：以下是所有连续巫师组：
```

- [5,4,6] 中 [5] ，总力量值为 min([5]) \* sum([5]) = 5 \* 5 = 25
- [5,4,6] 中 [4] ，总力量值为 min([4]) \* sum([4]) = 4 \* 4 = 16
- [5,4,6] 中 [6] ，总力量值为 min([6]) \* sum([6]) = 6 \* 6 = 36
- [5,4,6] 中 [5,4] ，总力量值为 min([5,4]) \* sum([5,4]) = 4 \* 9 = 36
- [5,4,6] 中 [4,6] ，总力量值为 min([4,6]) \* sum([4,6]) = 4 \* 10 = 40
- [5,4,6] 中 [5,4,6] ，总力量值为 min([5,4,6]) \* sum([5,4,6]) = 4 \* 15 = 60

所有力量值之和为 25 + 16 + 36 + 36 + 40 + 60 = 213 。

__提示：__

- `1 <= strength.length <= 10 ^ 5`
- `1 <= strength[i] <= 10 ^ 9`

__思路:__

```text
单调栈 ➕ 前缀和
维护两个单调栈 left 和 right
left[i] 表示第 i 个元素左边第一个比它小的元素的下标, 初始值为 -1
right[i] 表示第 i 个元素右边第一个比它小或者相等的元素的下标, 初始值为 n
则 strength[i] 的左边界为 left[i] + 1, 右边界为 right[i] - 1
使用前缀和 s[i] 表示 strength[0] + strength[1] + ... + strength[i - 1], s[0] = 0
为了求出所有子数组的和, 还需要计算前缀和的前缀和 ss[i] 表示 s[0] + s[1] + ... + s[i - 1], ss[0] = ss[1] = 0
则 strength[i] 的贡献为 strength[i] * ((i - left[i]) * (ss[right[i] + 1] - ss[i]) - (right[i] - i) * (ss[i + 1] - ss[left[i] + 1]))
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int totalStrength(vector<int>& strength) 
    {
        int MOD = 1e9 + 7, n = strength.size();
        vector<int> left(n, -1), right(n, n), ss(n + 2);
        stack<int> st;
        for (int i = 0; i < n; i++) 
        {
            while (!st.empty() && strength[st.top()] >= strength[i]) 
            {
                right[st.top()] = i;
                st.pop();
            }
            if (!st.empty()) left[i] = st.top();
            st.push(i);
        }
        long long s = 0LL, result = 0LL;
        for (int i = 0; i < n; i++) ss[i + 2] = (ss[i + 1] + (s += strength[i]) % MOD) % MOD;
        for (int i = 0; i < n; i++) result = (result + strength[i] * (((long long)(i - left[i]) * (ss[right[i] + 1] - ss[i + 1]) - (long long)(right[i] - i) * (ss[i + 1] - ss[left[i] + 1])) % MOD)) % MOD;
        return (result + MOD) % MOD;
    }
};
```

__Java__:

```Java
class Solution {
    public int totalStrength(int[] strength) {
        int MOD = 1_000_000_007, n = strength.length, left[] = new int[n], right[] = new int[n], ss[] = new int[n + 2];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && strength[stack.peek()] >= strength[i]) right[stack.pop()] = i;
            if (!stack.isEmpty()) left[i] = stack.peek();
            stack.push(i);
        }
        long s = 0L, result = 0L;
        for (int i = 0; i < n; i++) ss[i + 2] = (int)((ss[i + 1] + (s += strength[i]) % MOD) % MOD);
        for (int i = 0; i < n; i++) result = (result + strength[i] * (((long)(i - left[i]) * (ss[right[i] + 1] - ss[i + 1]) - (long)(right[i] - i) * (ss[i + 1] - ss[left[i] + 1])) % MOD)) % MOD;
        return (int)(result + MOD) % MOD;
    }
}
```

__Python__:

```Python
class Solution:
    def totalStrength(self, strength: List[int]) -> int:
        MOD, left, right, st = 10 ** 9 + 7, [-1] * (n := len(strength)), [n] * n, []
        for i, v in enumerate(strength):
            while st and strength[st[-1]] >= v: 
                right[st.pop()] = i
            if st: 
                left[i] = st[-1]
            st.append(i)
        return 0 if not (ss := list(accumulate(accumulate(strength, initial=0), initial=0))) else sum(v * ((i - left[i]) * (ss[right[i] + 1] - ss[i + 1]) - (right[i] - i) * (ss[i + 1] - ss[left[i] + 1])) for i, v in enumerate(strength)) % MOD
```
