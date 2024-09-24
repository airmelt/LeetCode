# 2261 K Divisible Elements Sub arrays 含最多 K 个可整除元素的子数组

__Description:__

Given an integer array `nums` and two integers `k` and `p`, return _the number of __distinct sub arrays,__ which have __at most___ `k` _elements_ that are _divisible by_ `p`.

Two arrays `nums1` and `nums2` are said to be __distinct__ if:

- They are of __different__ lengths, or
- There exists __at least__ one index `i` where `nums1[i] != nums2[i]`.

A sub array is defined as a non-empty contiguous sequence of elements in an array.

__Example:__

Example 1:

```text
Input: nums = [2,3,3,2,2], k = 2, p = 2
Output: 11
Explanation:
The elements at indices 0, 3, and 4 are divisible by p = 2.
The 11 distinct sub arrays which have at most k = 2 elements divisible by 2 are:
[2], [2,3], [2,3,3], [2,3,3,2], [3], [3,3], [3,3,2], [3,3,2,2], [3,2], [3,2,2], and [2,2].
Note that the sub arrays [2] and [3] occur more than once in nums, but they should each be counted only once.
The sub array [2,3,3,2,2] should not be counted because it has 3 elements that are divisible by 2.
```

Example 2:

```text
Input: nums = [1,2,3,4], k = 4, p = 1
Output: 10
Explanation:
All element of nums are divisible by p = 1.
Also, every sub array of nums will have at most 4 elements that are divisible by 1.
Since all sub arrays are distinct, the total number of sub arrays satisfying all the constraints is 10.
```

__Constraints:__

- `1 <= nums.length <= 200`
- `1 <= nums[i], p <= 200`
- `1 <= k <= nums.length`

Follow up:

Can you solve this problem in O(n2) time complexity?

__题目描述:__

给你一个整数数组 `nums` 和两个整数 `k` 和 `p` ，找出并返回满足要求的不同的子数组数，要求子数组中最多 `k` 个可被 `p` 整除的元素。

如果满足下述条件之一，则认为数组 `nums1` 和 `nums2` 是 __不同__ 数组:

- 两数组长度 __不同__ ，或者
- 存在 __至少__ 一个下标 `i` 满足 `nums1[i] != nums2[i]` 。

子数组 定义为：数组中的连续元素组成的一个 非空 序列。

__示例:__

示例 1：

```text
输入：nums = [2,3,3,2,2], k = 2, p = 2
输出：11
解释：
位于下标 0、3 和 4 的元素都可以被 p = 2 整除。
共计 11 个不同子数组都满足最多含 k = 2 个可以被 2 整除的元素：
[2]、[2,3]、[2,3,3]、[2,3,3,2]、[3]、[3,3]、[3,3,2]、[3,3,2,2]、[3,2]、[3,2,2] 和 [2,2] 。
注意，尽管子数组 [2] 和 [3] 在 nums 中出现不止一次，但统计时只计数一次。
子数组 [2,3,3,2,2] 不满足条件，因为其中有 3 个元素可以被 2 整除。
```

示例 2：

```text
输入：nums = [1,2,3,4], k = 4, p = 1
输出：10
解释：
nums 中的所有元素都可以被 p = 1 整除。
此外，nums 中的每个子数组都满足最多 4 个元素可以被 1 整除。
因为所有子数组互不相同，因此满足所有限制条件的子数组总数为 10 。
```

__提示：__

- `1 <= nums.length <= 200`
- `1 <= nums[i], p <= 200`
- `1 <= k <= nums.length`

进阶：

你可以设计并实现时间复杂度为 `O(n ^ 2)` 的算法解决此问题吗？

__思路:__

```text
1. 哈希表
遍历数组
遍历子数组, 如果子数组中的元素满足整除 p 的个数超过 k, 则跳出
否则将子数组加入哈希表, Python 可以转化为元组加入哈希表, 也可以转化为字符串加入哈希表
返回哈希表的长度
时间复杂度为 O(N ^ 3), 空间复杂度为 O(N ^ 3)
2. 字典树(Trie)
用字典树记录每个子数组的情况
遍历数组, 遍历子数组, 如果子数组中的元素满足整除 p 的个数超过 k, 则跳出
否则将子数组加入字典树
返回字典树的个数
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countDistinct(vector<int>& nums, int k, int p) 
    {
        vector<unordered_map<int, int>> trie;
        auto insert = [&]() -> int 
        {
            trie.push_back(unordered_map<int, int>()); 
            return trie.size() - 1; 
        };
        insert();
        for(int i = 0, n = nums.size(); i < n; i++) 
        {
            for (int j = i, node = 0, c = 0; j < n; j++) 
            {
                if (!(nums[j] % p)) ++c;
                if (c > k) break;
                if (!trie[node].count(nums[j])) trie[node][nums[j]] = insert();
                node = trie[node][nums[j]];
            }
        }
        return trie.size() - 1;
    }
};
```

__Java__:

```Java
class Solution {
    public int countDistinct(int[] nums, int k, int p) {
        int result = 0;
        Set<String> set = new HashSet<>();
        for (int i = 0, n = nums.length; i < n; i++) {
            StringBuilder sb = new StringBuilder();
            for (int j = i, c = 0; j < n; j++) {
                if (nums[j] % p == 0) ++c;
                if (c > k) break;
                sb.append(nums[j] + "*");
                set.add(sb.toString());
            }
        }
        return set.size();
    }
}
```

__Python__:

```Python
class Solution:
    def countDistinct(self, nums: List[int], k: int, p: int) -> int:
        s, n = set(), len(nums)
        for i in range(n):
            cur = 0
            for j in range(i, n):
                if not nums[j] % p:
                    cur += 1
                    if cur > k:
                        break
                s.add(tuple(nums[i:j + 1]))
        return len(s)
```
