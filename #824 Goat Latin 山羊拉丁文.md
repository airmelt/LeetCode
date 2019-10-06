__Description__:
A sentence S is given, composed of words separated by spaces. Each word consists of lowercase and uppercase letters only.

We would like to convert the sentence to "Goat Latin" (a made-up language similar to Pig Latin.)

The rules of Goat Latin are as follows:

If a word begins with a vowel (a, e, i, o, or u), append "ma" to the end of the word.
For example, the word 'apple' becomes 'applema'.
 
If a word begins with a consonant (i.e. not a vowel), remove the first letter and append it to the end, then add "ma".
For example, the word "goat" becomes "oatgma".
 
Add one letter 'a' to the end of each word per its word index in the sentence, starting with 1.
For example, the first word gets "a" added to the end, the second word gets "aa" added to the end and so on.
Return the final sentence representing the conversion from S to Goat Latin. 

__Example:__
Example 1:

Input: "I speak Goat Latin"
Output: "Imaa peaksmaaa oatGmaaaa atinLmaaaaa"

Example 2:

Input: "The quick brown fox jumped over the lazy dog"
Output: "heTmaa uickqmaaa rownbmaaaa oxfmaaaaa umpedjmaaaaaa overmaaaaaaa hetmaaaaaaaa azylmaaaaaaaaa ogdmaaaaaaaaaa"
 
__Notes:__

S contains only uppercase, lowercase and spaces. Exactly one space between each word.
1 <= S.length <= 150.

__题目描述__:
给定一个由空格分割单词的句子 S。每个单词只包含大写或小写字母。

我们要将句子转换为 “Goat Latin”（一种类似于 猪拉丁文 - Pig Latin 的虚构语言）。

山羊拉丁文的规则如下：

如果单词以元音开头（a, e, i, o, u），在单词后添加"ma"。
例如，单词"apple"变为"applema"。

如果单词以辅音字母开头（即非元音字母），移除第一个字符并将它放到末尾，之后再添加"ma"。
例如，单词"goat"变为"oatgma"。

根据单词在句子中的索引，在单词最后添加与索引相同数量的字母'a'，索引从1开始。
例如，在第一个单词后添加"a"，在第二个单词后添加"aa"，以此类推。
返回将 S 转换为山羊拉丁文后的句子。

__示例 :__
示例 1:

输入: "I speak Goat Latin"
输出: "Imaa peaksmaaa oatGmaaaa atinLmaaaaa"

示例 2:

输入: "The quick brown fox jumped over the lazy dog"
输出: "heTmaa uickqmaaa rownbmaaaa oxfmaaaaa umpedjmaaaaaa overmaaaaaaa hetmaaaaaaaa azylmaaaaaaaaa ogdmaaaaaaaaaa"
__说明：__

S 中仅包含大小写字母和空格。单词间有且仅有一个空格。
1 <= S.length <= 150。

__思路__:
用 split拆分之后按照规则添加然后合并即可
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution {
public:
    string toGoatLatin(string S) 
    {
        vector<string> nums;
        S += " ";
        string temp = "", test = "AEIOUaeiou";
        for (int i = 0; i < S.size(); i++)
        {
            if (S[i] != ' ') temp += S[i];
            else
            {
                nums.push_back(temp);
                temp.clear();
            }
        }
        for (int i = 0; i < nums.size(); i++)
        {
            if (test.find(nums[i][0]) == -1)
            {
                nums[i] += nums[i][0];
                nums[i] = nums[i].substr(1);
            }
            nums[i] = nums[i] + "ma";
            for (int j = 0;j < i + 1;j++) nums[i] += "a";
        } 
        string result = "";
        for (int i = 0; i < nums.size(); i++) result += nums[i] +" ";
        result.resize(result.size() - 1);
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public String toGoatLatin(String S) {
        String dic = "aeiouAEIOU", s[] = S.split(" ");
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < s.length; i++) {
            StringBuilder word = new StringBuilder(s[i]);
            if (dic.indexOf(word.charAt(0)) == -1) {
                word.append(word.charAt(0));
                word.delete(0, 1); 
            }
            word.append("ma");
            for (int j = 0; j <= i; j++) word.append('a');
            sb.append(word.toString());
            sb.append(" ");
        }
        sb.setLength(sb.length() - 1);
        return sb.toString();
    }
}
```

__Python__:
```Python
class Solution:
    def toGoatLatin(self, S: str) -> str:
        return ' '.join((lambda s: s + 'ma' if s[0] in 'aeiouAEIOU' else s[1:] + s[0] + 'ma')(s) + (i + 1) * 'a' for i, s in enumerate(S.split(' ')))
```