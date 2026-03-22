# 66 Plus One 加一

__Description__:
Given a non-empty array of digits representing a non-negative integer, plus one to the integer.

The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.

You may assume the integer does not contain any leading zero, except the number 0 itself.

__Example__:

Example 1:
Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.

Example 2:
Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.

__题目描述__:
给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储一个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

__示例__:

示例 1:
输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。

示例 2:
输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。

__思路__:
特别注意[9] -> [1, 0]
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> plusOne(vector<int>& digits) 
    {
        int len = digits.size() - 1;
        while (len > -1 and ++digits[len] == 10) digits[len--] = 0;
        if (len == - 1) digits.insert(digits.begin(), 1);
        return digits;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] plusOne(int[] digits) {
        int len = digits.length - 1;
        while (len > -1 && ++digits[len] == 10) digits[len--] = 0;
        if (len != - 1) return digits;
        int[] result = new int[digits.length + 1];
        for (int i = 0; i < digits.length; i++) result[i + 1] = digits[i];
        result[0] = 1;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        return list(map(int, str(int("".join(map(str, digits))) + 1).zfill(len(digits))))
```
