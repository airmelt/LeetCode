# 17 Letter Combinations of a Phone Number 电话号码的字母组合

__Description__:
Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.

![Telephone-keypad](https://upload-images.jianshu.io/upload_images/16639143-2c54d76a1d15c4bc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__Example:__

Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:

Although the above answer is in lexicographical order, your answer could be in any order you want.

__题目描述__:
给定一个仅包含数字 2-9 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![电话按键](https://upload-images.jianshu.io/upload_images/16639143-c7b8de1df160b17b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

__示例 :__

输入："23"
输出：["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
说明:
尽管上面的答案是按字典序排列的，但是你可以任意选择答案输出的顺序。

__思路__:

回溯法, 相当于决策树中选择分支, 比如 “23”, 第一次选择 2, 对应 “abc”, 对这个字符串进行遍历, 再选择 3, 对应 ”def“, 遍历并链接两个字符即可得到结果
时间复杂度O(2 ^ n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> letterCombinations(string digits) 
    {
        vector<string> result;
        if (digits.size() == 0) return result;
        track_back(result, "", digits);
        return result;
    }
private:
    const string table[10]{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    void track_back(vector<string> &result, string combination, string next) 
    {
        if (next.size() == 0) result.push_back(combination);
        else for (auto letter : table[next[0] - '0']) track_back(result, combination + letter, next.substr(1));
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        if (digits.length() == 0) return result;
        trackBack(result, "", digits);
        return result;
    }
    
    private static String table[] = new String[]{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
    
    private void trackBack(List<String> result, String combination, String next) {
        if (next.length() == 0) result.add(combination);
        else for (char letter : table[next.charAt(0) - '0'].toCharArray()) trackBack(result, combination + letter, next.substring(1));
    }
}
```

__Python__:

```Python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        phone = {'2': ['a', 'b', 'c'],
                 '3': ['d', 'e', 'f'],
                 '4': ['g', 'h', 'I'],
                 '5': ['j', 'k', 'l'],
                 '6': ['m', 'n', 'o'],
                 '7': ['p', 'q', 'r', 's'],
                 '8': ['t', 'u', 'v'],
                 '9': ['w', 'x', 'y', 'z']}     
        def track_back(combination, next_digits):
            if len(next_digits) == 0:
                result.append(combination)
            else:
                for letter in phone[next_digits[0]]:
                    track_back(combination + letter, next_digits[1:])          
        result = []
        if digits:
            track_back("", digits)
        return result
```
