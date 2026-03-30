# 2956 Find Common Elements Between Two Arrays 找到两个数组中的公共元素

__Description:__

You are given two integer arrays `nums1` and `nums2` of sizes `n` and `m`, respectively. Calculate the following values:

- `answer1` : the number of indices `i` such that `nums1[i]` exists in `nums2`.
- `answer2` : the number of indices `i` such that `nums2[i]` exists in `nums1`.

Return `[answer1,answer2]`.

__Example:__

Example 1:

```text
Input: nums1 = [2,3,2], nums2 = [1,2]

Output: [2,1]

Explanation:
```

![2956-1](https://assets.leetcode.com/uploads/2024/05/26/3488_find_common_elements_between_two_arrays-t1.gif)

Example 2:

```text
Input: nums1 = [4,3,2,3,1], nums2 = [2,2,5,2,3,6]

Output: [3,4]

Explanation:
```

The elements at indices 1, 2, and 3 in `nums1` exist in `nums2` as well. So `answer1` is 3.

The elements at indices 0, 1, 3, and 4 in `nums2` exist in `nums1`. So `answer2` is 4.

Example 3:

```text
Input: nums1 = [3,4,2,3], nums2 = [1,5]

Output: [0,0]

Explanation:
```

No numbers are common between `nums1` and `nums2`, so answer is [0,0].

__Constraints:__

- `n == nums1.length`
- `m == nums2.length`
- `1 <= n, m <= 100`
- `1 <= nums1[i], nums2[i] <= 100`

__题目描述:__

给你两个下标从 __0__ 开始的整数数组 `nums1` 和 `nums2` ，它们分别含有 `n` 和 `m` 个元素。请你计算以下两个数值:

- `answer1`:使得 `nums1[i]` 在 `nums2` 中出现的下标 `i` 的数量。
- `answer2`:使得 `nums2[i]` 在 `nums1` 中出现的下标 `i` 的数量。

返回 `[answer1, answer2]`。

__示例:__

示例 1：

```text
输入：nums1 = [2,3,2], nums2 = [1,2]

输出：[2,1]

解释：
```

![2956-2](https://assets.leetcode.com/uploads/2024/05/26/3488_find_common_elements_between_two_arrays-t1.gif)

示例 2：

```text
输入：nums1 = [4,3,2,3,1], nums2 = [2,2,5,2,3,6]

输出：[3,4]

解释：
```

`nums1` 中下标在 1，2，3 的元素在 `nums2` 中也存在。所以 `answer1` 为 3。

`nums2` 中下标在 0，1，3，4 的元素在 `nums1` 中也存在。所以 `answer2` 为 4。

示例 3：

```text
输入：nums1 = [3,4,2,3], nums2 = [1,5]

输出：[0,0]

解释：
```

`nums1` 和 `nums2` 中没有相同的数字，所以答案是 [0,0]。

__提示：__

- `n == nums1.length`
- `m == nums2.length`
- `1 <= n, m <= 100`
- `1 <= nums1[i], nums2[i] <= 100`

__思路:__

```text
模拟
将 nums1 和 nums2 转化成哈希集合 set1 和 set2
如果 nums1 中元素在 set2 中 answer1 累加
如果 nums2 中元素在 set1 中 answer2 累加
最后返回一个长度为 2 的数组 [answer1, answer2]
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> findIntersectionValues(vector<int>& nums1, vector<int>& nums2) 
    {
        unordered_set<int> set1(nums1.begin(), nums1.end()), set2(nums2.begin(), nums2.end());
        vector<int> result(2);
        for (const auto& num : nums1) result.front() += set2.count(num);
        for (const auto& num : nums2) result.back() += set1.count(num);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] findIntersectionValues(int[] nums1, int[] nums2) {
        return new int[]{ (int)Arrays.stream(nums1).filter(Arrays.stream(nums2).boxed().collect(Collectors.toSet())::contains).count(), (int)Arrays.stream(nums2).filter(Arrays.stream(nums1).boxed().collect(Collectors.toSet())::contains).count() };
    }
}
```

__Python__:

```Python
class Solution:
    def findIntersectionValues(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return [sum(num in set(nums2) for num in nums1), sum(num in set(nums1) for num in nums2)]
```
