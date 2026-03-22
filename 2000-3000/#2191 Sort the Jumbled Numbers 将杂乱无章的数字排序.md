# 2191 Sort the Jumbled Numbers 将杂乱无章的数字排序

__Description:__

You are given a __0-indexed__ integer array `mapping` which represents the mapping rule of a shuffled decimal system. `mapping[i] = j` means digit `i` should be mapped to digit `j` in this system.

The __mapped value__ of an integer is the new integer obtained by replacing each occurrence of digit `i` in the integer with `mapping[i]` for all `0 <= i <= 9`.

You are also given another integer array `nums`. Return _the array_ `nums` _sorted in __non-decreasing__ order based on the __mapped values__ of its elements._

Notes:

- Elements with the same mapped values should appear in the __same relative order__ as in the input.
- The elements of `nums` should only be sorted based on their mapped values and __not be replaced__ by them.

__Example:__

Example 1:

```text
Input: mapping = [8,9,4,0,2,1,3,5,7,6], nums = [991,338,38]
Output: [338,38,991]
Explanation: 
Map the number 991 as follows:
1. mapping[9] = 6, so all occurrences of the digit 9 will become 6.
2. mapping[1] = 9, so all occurrences of the digit 1 will become 9.
Therefore, the mapped value of 991 is 669.
338 maps to 007, or 7 after removing the leading zeros.
38 maps to 07, which is also 7 after removing leading zeros.
Since 338 and 38 share the same mapped value, they should remain in the same relative order, so 338 comes before 38.
Thus, the sorted array is [338,38,991].
```

Example 2:

```text
Input: mapping = [0,1,2,3,4,5,6,7,8,9], nums = [789,456,123]
Output: [123,456,789]
Explanation: 789 maps to 789, 456 maps to 456, and 123 maps to 123. Thus, the sorted array is [123,456,789].
```

__Constraints:__

- `mapping.length == 10`
- `0 <= mapping[i] <= 9`
- All the values of `mapping[i]` are __unique__.
- `1 <= nums.length <= 3 * 10 ^ 4`
- `0 <= nums[i] < 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的整数数组 `mapping` ，它表示一个十进制数的映射规则， `mapping[i] = j` 表示这个规则下将数位 `i` 映射为数位 `j` 。

一个整数 __映射后的值__ 为将原数字每一个数位 `i` （ `0 <= i <= 9`）映射为 `mapping[i]` 。

另外给你一个整数数组 `nums` ，请你将数组 `nums` 中每个数按照它们映射后对应数字非递减顺序排序后返回。

注意：

- 如果两个数字映射后对应的数字大小相同，则将它们按照输入中的 __相对顺序__ 排序。
- `nums` 中的元素只有在排序的时候需要按照映射后的值进行比较，返回的值应该是输入的元素本身。

__示例:__

示例 1：

```text
输入：mapping = [8,9,4,0,2,1,3,5,7,6], nums = [991,338,38]
输出：[338,38,991]
解释：
将数字 991 按照如下规则映射：
1. mapping[9] = 6 ，所有数位 9 都会变成 6 。
2. mapping[1] = 9 ，所有数位 1 都会变成 9 。
所以，991 映射的值为 669 。
338 映射为 007 ，去掉前导 0 后得到 7 。
38 映射为 07 ，去掉前导 0 后得到 7 。
由于 338 和 38 映射后的值相同，所以它们的前后顺序保留原数组中的相对位置关系，338 在 38 的前面。
所以，排序后的数组为 [338,38,991] 。
```

示例 2：

```text
输入：mapping = [0,1,2,3,4,5,6,7,8,9], nums = [789,456,123]
输出：[123,456,789]
解释：789 映射为 789 ，456 映射为 456 ，123 映射为 123 。所以排序后数组为 [123,456,789] 。
```

__提示：__

- `mapping.length == 10`
- `0 <= mapping[i] <= 9`
- `mapping[i]` 的值 __互不相同__ 。
- `1 <= nums.length <= 3 * 10 ^ 4`
- `0 <= nums[i] < 10 ^ 9`

__思路:__

```text
模拟
按照映射找到每个数字的映射值, 然后按照映射值排序
可以直接使用 map/TreeSet 等数据结构进行排序
也可以排序之后再映射回原来的值
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> sortJumbled(vector<int>& mapping, vector<int>& nums) 
    {
        map<int, vector<int>> m;
        for (const auto& num : nums) 
        {
            int cur = 0;
            if (!num) cur = mapping[num];
            for (int i = num, k = 1; i; i /= 10, k *= 10) cur = cur + mapping[i % 10] * k;
            m[cur].emplace_back(num);
        }
        int n = nums.size(), j = 0;
        for (const auto& [key, val] : m) for (const auto& num : val) nums[j++] = num;
        return nums;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] sortJumbled(int[] mapping, int[] nums) {
        Map<Integer, List<Integer>> map = new TreeMap<>();
        for (int num : nums) {
            int cur = 0;
            if (num == 0) cur = mapping[num];
            for (int i = num, k = 1; i != 0; i /= 10, k *= 10) cur = cur + mapping[i % 10] * k;
            map.computeIfAbsent(cur, t -> new ArrayList<>()).add(num);
        }
        int n = nums.length, result[] = new int[n], j = 0;
        for (List<Integer> val : map.values()) for (Integer num : val) result[j++] = num;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def sortJumbled(self, mapping: List[int], nums: List[int]) -> List[int]:
```
