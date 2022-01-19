# 823 Binary Trees With Factors 带因子的二叉树

__Description__:
Given an array of unique integers, arr, where each integer arr[i] is strictly greater than 1.

We make a binary tree using these integers, and each number may be used for any number of times. Each non-leaf node's value should be equal to the product of the values of its children.

Return the number of binary trees we can make. The answer may be too large so return the answer modulo 10^9 + 7.

__Example:__

Example 1:

Input: arr = [2,4]
Output: 3
Explanation: We can make these trees: [2], [4], [4, 2, 2]

Example 2:

Input: arr = [2,4,5,10]
Output: 7
Explanation: We can make these trees: [2], [4], [5], [10], [4, 2, 2], [10, 2, 5], [10, 5, 2].

__Constraints:__

1 <= arr.length <= 1000
2 <= arr[i] <= 10^9
All the values of arr are unique.

__题目描述__:
给出一个含有不重复整数元素的数组 arr ，每个整数 arr[i] 均大于 1。

用这些整数来构建二叉树，每个整数可以使用任意次数。其中：每个非叶结点的值应等于它的两个子结点的值的乘积。

满足条件的二叉树一共有多少个？答案可能很大，返回 对 10^9 + 7 取余 的结果。

__示例 :__

示例 1:

输入: arr = [2, 4]
输出: 3
解释: 可以得到这些二叉树: [2], [4], [4, 2, 2]

示例 2:

输入: arr = [2, 4, 5, 10]
输出: 7
解释: 可以得到这些二叉树: [2], [4], [5], [10], [4, 2, 2], [10, 2, 5], [10, 5, 2].

__提示:__

1 <= arr.length <= 1000
2 <= arr[i] <= 10^9
arr 中的所有值 互不相同

__思路__:

动态规划
每个结点自身一定可以构造一棵二叉树
将链表排序
dp[w] += dp[u] \* dp[v], if w == u \* v
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int numFactoredBinaryTrees(vector<int>& arr) 
    {
        sort(arr.begin(), arr.end());
        map<int, long> m;
        for (const auto& num : arr) 
        {
            m[num] = 1L;
            for (auto& item : m) if (!(num % item.first) and m.count(num / item.first)) m[num] += m[num / item.first] * item.second;
        }
        long result = 0L, mod = 1000000007L;
        for (const auto& item : m) result += item.second;
        return (int)(result % mod);
    }
};
```

__Java__:

```Java
class Solution {
    public int numFactoredBinaryTrees(int[] arr) {
        Arrays.sort(arr);
        Map<Integer, Long> map = new HashMap<>();
        for (int num : arr) {
            map.put(num, 1L);
            for (int key : map.keySet()) if (num % key == 0 && map.containsKey(num / key)) map.put(num, map.get(num) + map.get(key) * map.get(num / key));
        }
        long result = 0L, mod = 1000000007L;
        for (long value : map.values()) result += value;
        return (int)(result % mod);
    }
}
```

__Python__:

```Python
class Solution:
    def numFactoredBinaryTrees(self, arr: List[int]) -> int:
        arr.sort()
        d = {}
        for num in arr:
            d[num] = 1
            for factor in d:
                if not num % factor and (result := num // factor) in d:
                    d[num] += d[factor] * d[result]
        return sum(d.values()) % (10 ** 9 + 7)
```
