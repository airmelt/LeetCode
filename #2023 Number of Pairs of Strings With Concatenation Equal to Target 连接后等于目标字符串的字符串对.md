# 2023 Number of Pairs of Strings With Concatenation Equal to Target 连接后等于目标字符串的字符串对

__Description:__

Given an array of __digit__ strings `nums` and a __digit__ string `target`, return _the number of pairs of indices_ `(i, j)` _(where_ `i != j`_) such that the __concatenation__ of_ `nums[i] + nums[j]` _equals_ `target`.

__Example:__

Example 1:

```text
Input: nums = ["777","7","77","77"], target = "7777"
Output: 4
Explanation: Valid pairs are:
```

- (0, 1): "777" + "7"
- (1, 0): "7" + "777"
- (2, 3): "77" + "77"
- (3, 2): "77" + "77"

Example 2:

```text
Input: nums = ["123","4","12","34"], target = "1234"
Output: 2
Explanation: Valid pairs are:
```

- (0, 1): "123" + "4"
- (2, 3): "12" + "34"

Example 3:

```text
Input: nums = ["1","1","1"], target = "11"
Output: 6
Explanation: Valid pairs are:
```

- (0, 1): "1" + "1"
- (1, 0): "1" + "1"
- (0, 2): "1" + "1"
- (2, 0): "1" + "1"
- (1, 2): "1" + "1"
- (2, 1): "1" + "1"

__Constraints:__

- `2 <= nums.length <= 100`
- `1 <= nums[i].length <= 100`
- `2 <= target.length <= 100`
- `nums[i]` and `target` consist of digits.
- `nums[i]` and `target` do not have leading zeros.

__题目描述:__

给你一个 __数字__ 字符串数组 `nums` 和一个 __数字__ 字符串 `target` ，请你返回 `nums[i] + nums[j]` （两个字符串连接）结果等于 `target` 的下标 `(i, j)` （需满足 `i != j`）的数目。

__示例:__

示例 1：

```text
输入：nums = ["777","7","77","77"], target = "7777"
输出：4
解释：符合要求的下标对包括：
```

- (0, 1)："777" + "7"
- (1, 0)："7" + "777"
- (2, 3)："77" + "77"
- (3, 2)："77" + "77"

示例 2：

```text
输入：nums = ["123","4","12","34"], target = "1234"
输出：2
解释：符合要求的下标对包括
```

- (0, 1)："123" + "4"
- (2, 3)："12" + "34"

示例 3：

```text
输入：nums = ["1","1","1"], target = "11"
输出：6
解释：符合要求的下标对包括
```

- (0, 1)："1" + "1"
- (1, 0)："1" + "1"
- (0, 2)："1" + "1"
- (2, 0)："1" + "1"
- (1, 2)："1" + "1"
- (2, 1)："1" + "1"

__提示：__

- `2 <= nums.length <= 100`
- `1 <= nums[i].length <= 100`
- `2 <= target.length <= 100`
- `nums[i]` 和 `target` 只包含数字。
- `nums[i]` 和 `target` 不含有任何前导 0 。

__思路:__

```text
1. 暴力法
遍历所有的字符串对，判断是否满足条件
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1), 其中 N 为 nums 的长度
2. 哈希表
使用哈希表存储 nums 中的字符串出现的次数
遍历目标的前缀和后缀
如果前缀和后缀相等，则结果加上前缀出现的次数乘以前缀出现的次数减一
否则结果加上前缀出现的次数乘以后缀出现的次数
时间复杂度为 O(M + N), 空间复杂度为 O(N), 其中 M 为 target 的长度，N 为 nums 的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numOfPairs(vector<string>& nums, string target) 
    {
        int result = 0;
        unordered_map<string, int> m;
        for (const auto& num : nums) ++m[num];
        for (int i = 1, n = target.size(); i < n; i++) 
        {
            if (target.substr(0, i) == target.substr(i)) result += m[target.substr(i)] * (m[target.substr(i)] - 1);
            else result += m[target.substr(0, i)] * m[target.substr(i)];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numOfPairs(String[] nums, String target) {
        int result = 0;
        Map<String, Integer> map = new HashMap<>();
        for (String num : nums) map.merge(num, 1, Integer::sum);
        for (int i = 1, n = target.length(); i < n; i++) {
            if (target.substring(0, i).equals(target.substring(i))) result += map.getOrDefault(target.substring(i), 0) * (map.getOrDefault(target.substring(i), 0) - 1);
            else result += map.getOrDefault(target.substring(0, i), 0) * map.getOrDefault(target.substring(i), 0);
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numOfPairs(self, nums: List[str], target: str) -> int:
        return sum(nums[i] + nums[j] == target for i in range(len(nums)) for j in range(len(nums)) if i != j)
```
