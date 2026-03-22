# 60 Permutation Sequence 第k个排列

__Description__:
The set [1,2,3,...,n] contains a total of n! unique permutations.

By listing and labeling all of the permutations in order, we get the following sequence for n = 3:

```text
"123"
"132"
"213"
"231"
"312"
"321"
```

Given n and k, return the kth permutation sequence.

__Note:__

Given n will be between 1 and 9 inclusive.
Given k will be between 1 and n! inclusive.

__Example:__

Example 1:

Input: n = 3, k = 3
Output: "213"

Example 2:

Input: n = 4, k = 9
Output: "2314"

__题目描述__:
给出集合 [1,2,3,…,n]，其所有元素共有 n! 种排列。

按大小顺序列出所有排列情况，并一一标记，当 n = 3 时, 所有排列如下：

```text
"123"
"132"
"213"
"231"
"312"
"321"
```

给定 n 和 k，返回第 k 个排列。

__说明：__

给定 n 的范围是 [1, 9]。
给定 k 的范围是[1,  n!]。

__示例 :__

示例 1:

输入: n = 3, k = 3
输出: "213"

示例 2:

输入: n = 4, k = 9
输出: "2314"

__思路__:

1. 回溯法找到所有排列中的第 k个
时间复杂度O(n * n!), 空间复杂度O(n)
2. 数学方法
对 1开头的排列应该有 (n - 1)!个, 其中 2开头的排列应该有 (n - 2)!个, 以此类推
比如 n = 3, k = 3时,
假定 nums = [1, 2, 3](1 ~ n)
因为数组是从 0开始, 所以 k先自减 1
第一个数字应该是 nums[k / 2!] = 2
然后 nums删除 2剩下 [1, 3]
k %= 2!, k = 1
重复上面的步骤即可
时间复杂度O(n ^ 2), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    string getPermutation(int n, int k) 
    {
        string result = string("123456789").substr(0, n);
        for (int i = 1; i < k; i++) next_permutation(result.begin(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String getPermutation(int n, int k) {
        char result[] = new char[n];
        int i = 0, j = 0, nums[] = new int[n], f[] = new int[n];
        f[0] = 1;
        for (; i < n; i++) {
            nums[i] = i + 1;
            if (i > 0) f[i] = i * f[i - 1];
        }
        for (i = 0, --k; i < n; i++) {
            int temp = k / f[n - i - 1];
            result[j++] = (char)(nums[temp] + '0');
            while (temp < n - i - 1) nums[temp] = nums[1 + temp++];
            k %= f[n - i - 1];
        }
        return new String(result);
    }
}
```

__Python__:

```Python
class Solution:
    def getPermutation(self, n: int, k: int) -> str:
        return ''.join(list(itertools.permutations('123456789'[:n]))[k - 1])
```
