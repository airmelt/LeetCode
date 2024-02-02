# 1980 Find Unique Binary String 找出不同的二进制字符串

__Description:__

Given an array of strings `nums` containing `n` __unique__ binary strings each of length `n`, return _a binary string of length_ `n` _that __does not appear__ in_ `nums`_. If there are multiple answers, you may return __any__ of them_.

__Example:__

Example 1:

```text
Input: nums = ["01","10"]
Output: "11"
Explanation: "11" does not appear in nums. "00" would also be correct.
```

Example 2:

```text
Input: nums = ["00","01"]
Output: "11"
Explanation: "11" does not appear in nums. "10" would also be correct.
```

Example 3:

```text
Input: nums = ["111","011","001"]
Output: "101"
Explanation: "101" does not appear in nums. "000", "010", "100", and "110" would also be correct.
```

__Constraints:__

- `n == nums.length`
- `1 <= n <= 16`
- `nums[i].length == n`
- `nums[i]` is either `'0'` or `'1'`.
- All the strings of `nums` are __unique__.

__题目描述:__

给你一个字符串数组 `nums` ，该数组由 `n` 个 __互不相同__ 的二进制字符串组成，且每个字符串长度都是 `n` 。请你找出并返回一个长度为 `n` 且 __没有出现__ 在 `nums` 中的二进制字符串_。_如果存在多种答案，只需返回 __任意一个__ 即可。

__示例:__

示例 1：

```text
输入：nums = ["01","10"]
输出："11"
解释："11" 没有出现在 nums 中。"00" 也是正确答案。
```

示例 2：

```text
输入：nums = ["00","01"]
输出："11"
解释："11" 没有出现在 nums 中。"10" 也是正确答案。
```

示例 3：

```text
输入：nums = ["111","011","001"]
输出："101"
解释："101" 没有出现在 nums 中。"000"、"010"、"100"、"110" 也是正确答案。
```

__提示：__

- `n == nums.length`
- `1 <= n <= 16`
- `nums[i].length == n`
- `nums[i]` 为 `'0'` 或 `'1'`
- `nums` 中的所有字符串 __互不相同__

__思路:__

```text
数学
构造一个字符串
使得第 i 位与 nums[i] 的第 i 位不同
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string findDifferentBinaryString(vector<string>& nums) 
    {
        string result;
        for (int i = 0, n = nums.size(); i < n; i++) result.push_back(nums[i][i] ^ 1);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String findDifferentBinaryString(String[] nums) {
        StringBuilder result = new StringBuilder();
        for (int i = 0, n = nums.length; i < n; i++) result.append(nums[i].charAt(i) == '0' ? '1' : '0');
        return result.toString();
    }
}
```

__Python__:

```Python
class Solution:
    def findDifferentBinaryString(self, nums: List[str]) -> str:
        return ''.join(chr(ord(nums[i][i]) ^ 1) for i in range(len(nums)))
```
