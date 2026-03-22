# 1577 Number of Ways Where Square of Number Is Equal to Product of Two Numbers 数的平方等于两数乘积的方法数

__Description:__

Given two arrays of integers `nums1` and `nums2`, return the number of triplets formed (type 1 and type 2) under the following rules:

- Type 1: Triplet (i, j, k) if `nums1[i] ^ 2 == nums2[j] * nums2[k]` where `0 <= i < nums1.length` and `0 <= j < k < nums2.length`.
- Type 2: Triplet (i, j, k) if `nums2[i] ^ 2 == nums1[j] * nums1[k]` where `0 <= i < nums2.length` and `0 <= j < k < nums1.length`.

__Example:__

Example 1:

```text
Input: nums1 = [7,4], nums2 = [5,2,8,9]
Output: 1
Explanation: Type 1: (1, 1, 2), nums1[1]2 = nums2[1] * nums2[2]. (42 = 2 * 8).
```

Example 2:

```text
Input: nums1 = [1,1], nums2 = [1,1,1]
Output: 9
Explanation: All Triplets are valid, because 12 = 1 * 1.
Type 1: (0,0,1), (0,0,2), (0,1,2), (1,0,1), (1,0,2), (1,1,2).  nums1[i]2 = nums2[j] * nums2[k].
Type 2: (0,0,1), (1,0,1), (2,0,1). nums2[i]2 = nums1[j] * nums1[k].
```

Example 3:

```text
Input: nums1 = [7,7,8,3], nums2 = [1,2,9,7]
Output: 2
Explanation: There are 2 valid triplets.
Type 1: (3,0,2).  nums1[3]2 = nums2[0] * nums2[2].
Type 2: (3,0,1).  nums2[3]2 = nums1[0] * nums1[1].
```

__Constraints:__

- `1 <= nums1.length, nums2.length <= 1000`
- `1 <= nums1[i], nums2[i] <= 10 ^ 5`

__题目描述:__

给你两个整数数组 `nums1` 和 `nums2` ，请你返回根据以下规则形成的三元组的数目（类型 1 和类型 2 ）:

- 类型 1:三元组 `(i, j, k)` ，如果 `nums1[i] ^ 2 == nums2[j] * nums2[k]` 其中 `0 <= i < nums1.length` 且 `0 <= j < k < nums2.length`
- 类型 2:三元组 `(i, j, k)` ，如果 `nums2[i] ^ 2 == nums1[j] * nums1[k]` 其中 `0 <= i < nums2.length` 且 `0 <= j < k < nums1.length`

__示例:__

示例 1：

```text
输入：nums1 = [7,4], nums2 = [5,2,8,9]
输出：1
解释：类型 1：(1,1,2), nums1[1] ^ 2 = nums2[1] * nums2[2] (4 ^ 2 = 2 * 8)
```

示例 2：

```text
输入：nums1 = [1,1], nums2 = [1,1,1]
输出：9
解释：所有三元组都符合题目要求，因为 1 ^ 2 = 1 * 1
类型 1：(0,0,1), (0,0,2), (0,1,2), (1,0,1), (1,0,2), (1,1,2), nums1[i] ^ 2 = nums2[j] * nums2[k]
类型 2：(0,0,1), (1,0,1), (2,0,1), nums2[i] ^ 2 = nums1[j] * nums1[k]
```

示例 3：

```text
输入：nums1 = [7,7,8,3], nums2 = [1,2,9,7]
输出：2
解释：有两个符合题目要求的三元组
类型 1：(3,0,2), nums1[3] ^ 2 = nums2[0] * nums2[2]
类型 2：(3,0,1), nums2[3] ^ 2 = nums1[0] * nums1[1]
```

示例 4：

```text
输入：nums1 = [4,7,9,11,23], nums2 = [3,5,1024,12,18]
输出：0
解释：不存在符合题目要求的三元组
```

__提示：__

- `1 <= nums1.length, nums2.length <= 1000`
- `1 <= nums1[i], nums2[i] <= 10 ^ 5`

__思路:__

```text
哈希表
将两个数组的每个元素的平方和存入哈希表
查询的时候只需要查询两个元素的积
注意乘法可能溢出, 需要用 long 型
时间复杂度为 O(MN), 空间复杂度为 O(M + N)
```

__代码:__

__C++__:

```C++
class Solution 
{
private:
    int helper(vector<int>& nums, unordered_map<long long, int> mp)
    {
        int result = 0, n = nums.size();
        for (int i = 0; i < n; i++) for (int j = i + 1; j < n; j++) if (mp.find(1LL * nums[i] * nums[j]) != mp.end()) result += mp[1LL * nums[i] * nums[j]];
        return result;
    }
public:
    int numTriplets(vector<int>& nums1, vector<int>& nums2) 
    {
        unordered_map<long long, int> mp1, mp2;
        for (const auto& num: nums1) ++mp1[1LL * num * num];
        for (const auto& num: nums2) ++mp2[1LL * num * num];
        return helper(nums1, mp2) + helper(nums2, mp1);
    }
};
```

__Java__:

```Java
class Solution {
    public int numTriplets(int[] nums1, int[] nums2) {
        Map<Integer, Integer> map1 = new HashMap<Integer, Integer>(), map2 = new HashMap<Integer, Integer>();
        for (int num : nums1) map1.put(num, map1.getOrDefault(num, 0) + 1);
        for (int num : nums2) map2.put(num, map2.getOrDefault(num, 0) + 1);
        return helper(map1, map2) + helper(map2, map1);
    }

    public int helper(Map<Integer, Integer> map1, Map<Integer, Integer> map2) {
        int result = 0;
        Set<Integer> set1 = map1.keySet(), set2 = map2.keySet();
        for (int num1 : set1) {
            int count1 = map1.get(num1);
            long square = (long)num1 * num1;
            for (int num2 : set2) {
                if (square % num2 == 0 && square / num2 <= Integer.MAX_VALUE) {
                    int num3 = (int)(square / num2);
                    if (num2 == num3) {
                        int count2 = map2.get(num2), cur = count1 * count2 * (count2 - 1) / 2;
                        result += cur;
                    } else if (num2 < num3 && set2.contains(num3)) {
                        int count2 = map2.get(num2), count3 = map2.get(num3), cur = count1 * count2 * count3;
                        result += cur;
                    }
                }
            }
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numTriplets(self, nums1: List[int], nums2: List[int]) -> int:
        return 0 if not (c1 := Counter((nums1[i] * nums1[j] for i in range(len(nums1)) for j in range(i + 1, len(nums1))))) or not (c2 := Counter((nums2[i] * nums2[j] for i in range(len(nums2)) for j in range(i + 1, len(nums2))))) else sum(c1[nums2[i] ** 2] for i in range(len(nums2))) + sum(c2[nums1[i] ** 2] for i in range(len(nums1)))
```
