# 1985 Find the Kth Largest Integer in the Array 找出数组中的第 K 大整数

__Description:__

You are given an array of strings `nums` and an integer `k`. Each string in `nums` represents an integer without leading zeros.

Return _the string that represents the_ `k ^ th` ___largest integer__ in_ `nums`.

__Note__: Duplicate numbers should be counted distinctly. For example, if `nums` is `["1","2","2"]`, `"2"` is the first largest integer, `"2"` is the second-largest integer, and `"1"` is the third-largest integer.

__Example:__

Example 1:

```text
Input: nums = ["3","6","7","10"], k = 4
Output: "3"
Explanation:
The numbers in nums sorted in non-decreasing order are ["3","6","7","10"].
The 4th largest integer in nums is "3".
```

Example 2:

```text
Input: nums = ["2","21","12","1"], k = 3
Output: "2"
Explanation:
The numbers in nums sorted in non-decreasing order are ["1","2","12","21"].
The 3rd largest integer in nums is "2".
```

Example 3:

```text
Input: nums = ["0","0"], k = 2
Output: "0"
Explanation:
The numbers in nums sorted in non-decreasing order are ["0","0"].
The 2nd largest integer in nums is "0".
```

__Constraints:__

- `1 <= k <= nums.length <= 10 ^ 4`
- `1 <= nums[i].length <= 100`
- `nums[i]` consists of only digits.
- `nums[i]` will not have any leading zeros.

__题目描述:__

给你一个字符串数组 `nums` 和一个整数 `k` 。 `nums` 中的每个字符串都表示一个不含前导零的整数。

返回 `nums` 中表示第 `k` 大整数的字符串。

__注意:__重复的数字在统计时会视为不同元素考虑。例如，如果 `nums` 是 `["1","2","2"]`，那么 `"2"` 是最大的整数， `"2"` 是第二大的整数， `"1"` 是第三大的整数。

__示例:__

示例 1：

```text
输入：nums = ["3","6","7","10"], k = 4
输出："3"
解释：
nums 中的数字按非递减顺序排列为 ["3","6","7","10"]
其中第 4 大整数是 "3"
```

示例 2：

```text
输入：nums = ["2","21","12","1"], k = 3
输出："2"
解释：
nums 中的数字按非递减顺序排列为 ["1","2","12","21"]
其中第 3 大整数是 "2"
```

示例 3：

```text
输入：nums = ["0","0"], k = 2
输出："0"
解释：
nums 中的数字按非递减顺序排列为 ["0","0"]
其中第 2 大整数是 "0"
```

__提示：__

- `1 <= k <= nums.length <= 10 ^ 4`
- `1 <= nums[i].length <= 100`
- `nums[i]` 仅由数字组成
- `nums[i]` 不含任何前导零

__思路:__

```text
排序
自定义排序
如果两个字符串长度相等，那么比较字符串大小
如果两个字符串长度不相等，那么比较字符串长度
时间复杂度为 O(NlogN), 空间复杂度为 O(logN)
如果使用快速排序选择第 k 个元素，时间复杂度为 O(N), 空间复杂度为 O(logN)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string kthLargestNumber(vector<string>& nums, int k) 
    {
        nth_element(nums.begin(), nums.begin() + k - 1, nums.end(), [](const auto& a, const auto& b) { return a.size() == b.size() ? a > b : a.size() > b.size(); });
        return nums[k - 1];
    }
};
```

__Java__:

```Java
class Solution {
  public String kthLargestNumber(String[] nums, int k) {
        return Arrays.stream(nums).sorted(Comparator.comparingInt(String::length).thenComparing(String::compareTo)).skip(nums.length - k).findFirst().orElse("");
    }
}
```

__Python__:

```Python
class Solution:
    def kthLargestNumber(self, nums: List[str], k: int) -> str:
        return sorted(nums, key=int)[-k]
```
