__Description__:
Given a list of non negative integers, arrange them such that they form the largest number.

__Example:__
Example 1:

Input: [10,2]
Output: "210"

Example 2:

Input: [3,30,34,5,9]
Output: "9534330"

__Note:__
The result may be very large, so you need to return a string instead of an integer.

__题目描述__:
给定一组非负整数，重新排列它们的顺序使之组成一个最大的整数。

__示例 :__
示例 1:

输入: [10,2]
输出: 210

示例 2:

输入: [3,30,34,5,9]
输出: 9534330

__说明：__
输出结果可能非常大，所以你需要返回一个字符串而不是整数。

__思路__:
重写排序的比较函数
按照字符串排序
时间复杂度O(nlgn), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    string largestNumber(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end(), [](int a, int b) {return to_string(a) + to_string(b) > to_string(b) + to_string(a);});
        if (nums.front() == 0) return "0";
        string result;
        for (auto num : nums) result += to_string(num);
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public String largestNumber(int[] nums) {
        String[] numsList = new String[nums.length];
        for (int i = 0; i < nums.length; i++) numsList[i] = String.valueOf(nums[i]);
        Arrays.sort(numsList, (s1, s2) -> (s2 + s1).compareTo(s1 + s2));
        if ("0".equals(numsList[0])) return "0";
        StringBuilder result = new StringBuilder();
        for (String s : numsList) result.append(s);
        return result.toString();
    }
}
```

__Python__:
```Python
class Solution:
    def largestNumber(self, nums: List[int]) -> str:
        return '0' if ''.join(sorted(map(str, nums), key=functools.cmp_to_key(lambda x, y: 0 if x + y == y + x else 1 if x + y > y + x else -1), reverse=True))[0] == '0' else ''.join(sorted(map(str, nums), key=functools.cmp_to_key(lambda x, y: 0 if x + y == y + x else 1 if x + y > y + x else -1), reverse=True))
```