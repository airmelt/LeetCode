# 2032 Two Out of Three 至少在两个数组中出现的值

__Description:__

__Example:__

Example 1:

```text
Input: nums1 = [1,1,3,2], nums2 = [2,3], nums3 = [3]
Output: [3,2]
Explanation: The values that are present in at least two arrays are:
```

- 3, in all three arrays.
- 2, in nums1 and nums2.

Example 2:

```text
Input: nums1 = [3,1], nums2 = [2,3], nums3 = [1,2]
Output: [2,3,1]
Explanation: The values that are present in at least two arrays are:
```

- 2, in nums2 and nums3.
- 3, in nums1 and nums2.
- 1, in nums1 and nums3.

Example 3:

```text
Input: nums1 = [1,2,2], nums2 = [4,3,3], nums3 = [5]
Output: []
Explanation: No value is present in at least two arrays.
```

__Constraints:__

- `1 <= nums1.length, nums2.length, nums3.length <= 100`
- `1 <= nums1[i], nums2[j], nums3[k] <= 100`

__题目描述:__

__示例:__

示例 1：

```text
输入：nums1 = [1,1,3,2], nums2 = [2,3], nums3 = [3]
输出：[3,2]
解释：至少在两个数组中出现的所有值为：
```

- 3 ，在全部三个数组中都出现过。
- 2 ，在数组 nums1 和 nums2 中出现过。

示例 2：

```text
输入：nums1 = [3,1], nums2 = [2,3], nums3 = [1,2]
输出：[2,3,1]
解释：至少在两个数组中出现的所有值为：
```

- 2 ，在数组 nums2 和 nums3 中出现过。
- 3 ，在数组 nums1 和 nums2 中出现过。
- 1 ，在数组 nums1 和 nums3 中出现过。

示例 3：

```text
输入：nums1 = [1,2,2], nums2 = [4,3,3], nums3 = [5]
输出：[]
解释：不存在至少在两个数组中出现的值。
```

__提示：__

- `1 <= nums1.length, nums2.length, nums3.length <= 100`
- `1 <= nums1[i], nums2[j], nums3[k] <= 100`

__思路:__

```text
哈希表
用哈希表或者数组记录已经出现的值
遍历三个数组，将每个数组中的值对应的位设置为 1
最后遍历哈希表，将出现至少两次的值加入结果数组
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> twoOutOfThree(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3) 
    {
        vector<int> m(101), result;
        for (const auto& i : nums1) m[i] |= 0x1;
        for (const auto& i : nums2) m[i] |= 0x10;
        for (const auto& i : nums3) m[i] |= 0x100;
        for (int i = 0; i < 101; i++) if (m[i] & (m[i] - 1)) result.emplace_back(i);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<Integer> twoOutOfThree(int[] nums1, int[] nums2, int[] nums3) {
        int[] map = new int[101];
        for (int i : nums1) map[i] |= 0x1;
        for (int i : nums2) map[i] |= 0x10;
        for (int i : nums3) map[i] |= 0x100;
        List<Integer> result = new ArrayList<>();
        for (int i = 0; i < 101; i++) if ((map[i] & (map[i] - 1)) != 0) result.add(i);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def twoOutOfThree(self, nums1: List[int], nums2: List[int], nums3: List[int]) -> List[int]:
        return list((set(nums1) | set(nums2)) & (set(nums3) | set(nums2)) & (set(nums1) | set(nums3)))
```
