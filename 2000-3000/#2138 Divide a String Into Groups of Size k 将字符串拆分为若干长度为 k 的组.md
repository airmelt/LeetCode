# 2138 Divide a String Into Groups of Size k 将字符串拆分为若干长度为 k 的组

__Description:__

A string `s` can be partitioned into groups of size `k` using the following procedure:

- The first group consists of the first `k` characters of the string, the second group consists of the next `k` characters of the string, and so on. Each character can be a part of __exactly one__ group.
- For the last group, if the string __does not__ have `k` characters remaining, a character `fill` is used to complete the group.

Note that the partition is done so that after removing the `fill` character from the last group (if it exists) and concatenating all the groups in order, the resultant string should be `s`.

Given the string `s`, the size of each group `k` and the character `fill`, return _a string array denoting the __composition of every group___ `s` _has been divided into, using the above procedure_.

__Example:__

Example 1:

```text
Input: s = "abcdefghi", k = 3, fill = "x"
Output: ["abc","def","ghi"]
Explanation:
The first 3 characters "abc" form the first group.
The next 3 characters "def" form the second group.
The last 3 characters "ghi" form the third group.
Since all groups can be completely filled by characters from the string, we do not need to use fill.
Thus, the groups formed are "abc", "def", and "ghi".
```

Example 2:

```text
Input: s = "abcdefghij", k = 3, fill = "x"
Output: ["abc","def","ghi","jxx"]
Explanation:
Similar to the previous example, we are forming the first three groups "abc", "def", and "ghi".
For the last group, we can only use the character 'j' from the string. To complete this group, we add 'x' twice.
Thus, the 4 groups formed are "abc", "def", "ghi", and "jxx".
```

__Constraints:__

- `1 <= s.length <= 100`
- `s` consists of lowercase English letters only.
- `1 <= k <= 100`
- `fill` is a lowercase English letter.

__题目描述:__

字符串 `s` 可以按下述步骤划分为若干长度为 `k` 的组:

- 第一组由字符串中的前 `k` 个字符组成, 第二组由接下来的 `k` 个字符串组成, 依此类推。每个字符都能够成为 __某一个__ 组的一部分。
- 对于最后一组, 如果字符串剩下的字符 __不足__ `k` 个, 需使用字符 `fill` 来补全这一组字符。

注意, 在去除最后一个组的填充字符 `fill`（如果存在的话）并按顺序连接所有的组后, 所得到的字符串应该是 `s` 。

给你一个字符串 `s` , 以及每组的长度 `k` 和一个用于填充的字符 `fill` , 按上述步骤处理之后, 返回一个字符串数组, 该数组表示 `s` 分组后 __每个组的组成情况__ 。

__示例:__

示例 1：

```text
输入：s = "abcdefghi", k = 3, fill = "x"
输出：["abc","def","ghi"]
解释：
前 3 个字符是 "abc" , 形成第一组。
接下来 3 个字符是 "def" , 形成第二组。
最后 3 个字符是 "ghi" , 形成第三组。
由于所有组都可以由字符串中的字符完全填充, 所以不需要使用填充字符。
因此, 形成 3 组, 分别是 "abc"、"def" 和 "ghi" 。
```

示例 2：

```text
输入：s = "abcdefghij", k = 3, fill = "x"
输出：["abc","def","ghi","jxx"]
解释：
与前一个例子类似, 形成前三组 "abc"、"def" 和 "ghi" 。
对于最后一组, 字符串中仅剩下字符 'j' 可以用。为了补全这一组, 使用填充字符 'x' 两次。
因此, 形成 4 组, 分别是 "abc"、"def"、"ghi" 和 "jxx" 。
```

__提示：__

- `1 <= s.length <= 100`
- `s` 仅由小写英文字母组成
- `1 <= k <= 100`
- `fill` 是一个小写英文字母

__思路:__

```text
模拟
可以先将字符串 s 按照长度为 k 的组进行划分, 然后对于最后一组, 如果长度不足 k, 可以使用 fill 进行填充
或者在分割之前, 先将 s 的长度补全为 k 的整数倍, 然后再进行分割
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> divideString(string s, int k, char fill) 
    {
        vector<string> result;
        int n = s.size(), cur = 0;
        while (cur < n)
        {
            result.emplace_back(s.substr(cur, k));
            cur += k;
        }    
        result.back() += string(k - result.back().size(), fill);
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String[] divideString(String s, int k, char fill) {
        if (s.length() % k > 0) {
            StringBuilder sb = new StringBuilder(s);
            for (int i = 0, r = k - s.length() % k; i < r; i++) sb.append(fill);
            s = sb.toString();
        }
        String[] result = new String[s.length() / k];
        for (int i = 0, n = result.length; i < n; i++) result[i] = s.substring(i * k, (i + 1) * k);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def divideString(self, s: str, k: int, fill: str) -> List[str]:
        return [s[i:i + k] if len(s[i:i + k]) == k else s[i:i + k] + fill * (k - len(s[i:i + k])) for i in range(0, len(s), k)]
```
