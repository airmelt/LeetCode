# 165 Compare Version Numbers 比较版本号

__Description__:
Compare two version numbers version1 and version2.
If version1 > version2 return 1; if version1 < version2 return -1;otherwise return 0.

You may assume that the version strings are non-empty and contain only digits and the . character.

The . character does not represent a decimal point and is used to separate number sequences.

For instance, 2.5 is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.

You may assume the default revision number for each level of a version number to be 0. For example, version number 3.4 has a revision number of 3 and 4 for its first and second level revision number. Its third and fourth level revision number are both 0.

__Example:__

Example 1:

Input: version1 = "0.1", version2 = "1.1"
Output: -1

Example 2:

Input: version1 = "1.0.1", version2 = "1"
Output: 1

Example 3:

Input: version1 = "7.5.2.4", version2 = "7.5.3"
Output: -1

Example 4:

Input: version1 = "1.01", version2 = "1.001"
Output: 0
Explanation: Ignoring leading zeroes, both “01” and “001" represent the same number “1”

Example 5:

Input: version1 = "1.0", version2 = "1.0.0"
Output: 0
Explanation: The first version number does not have a third level revision number, which means its third level revision number is default to "0"

__Note:__

Version strings are composed of numeric strings separated by dots . and this numeric strings may have leading zeroes.
Version strings do not start or end with dots, and they will not be two consecutive dots.

__题目描述__:
比较两个版本号 version1 和 version2。
如果 version1 > version2 返回 1，如果 version1 < version2 返回 -1， 除此之外返回 0。

你可以假设版本字符串非空，并且只包含数字和 . 字符。

 . 字符不代表小数点，而是用于分隔数字序列。

例如，2.5 不是“两个半”，也不是“差一半到三”，而是第二版中的第五个小版本。

你可以假设版本号的每一级的默认修订版号为 0。例如，版本号 3.4 的第一级（大版本）和第二级（小版本）修订号分别为 3 和 4。其第三级和第四级修订号均为 0。

__示例 :__

示例 1:

输入: version1 = "0.1", version2 = "1.1"
输出: -1

示例 2:

输入: version1 = "1.0.1", version2 = "1"
输出: 1

示例 3:

输入: version1 = "7.5.2.4", version2 = "7.5.3"
输出: -1

示例 4：

输入：version1 = "1.01", version2 = "1.001"
输出：0
解释：忽略前导零，“01” 和 “001” 表示相同的数字 “1”。

示例 5：

输入：version1 = "1.0", version2 = "1.0.0"
输出：0
解释：version1 没有第三级修订号，这意味着它的第三级修订号默认为 “0”。

__提示：__

版本字符串由以点 （.） 分隔的数字字符串组成。这个数字字符串可能有前导零。
版本字符串不以点开始或结束，并且其中不会有两个连续的点。

__思路__:

类似字符串转数字
以 '.'分割开字符串, 每个中间转化成 int即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int compareVersion(string version1, string version2) 
    {
        int i = 0, j = 0;
        while(i < version1.size() or j < version2.size())
        {
            int a = 0, b = 0;
            while (i < version1.size() and version1[i] != '.') a = a * 10 + version1[i++] - '0';
            while (j < version2.size() and version2[j] != '.') b = b * 10 + version2[j++] - '0';
            if (a > b) return 1;
            else if (a < b) return -1;
            i++;
            j++;
        }
        return 0;
    }
};
```

__Java__:

```Java
class Solution {
    public int compareVersion(String version1, String version2) {
        int i = 0, j = 0;
        while(i < version1.length() || j < version2.length()) {
            int a = 0, b = 0;
            while (i < version1.length() && version1.charAt(i) != '.') a = a * 10 + version1.charAt(i++) - '0';
            while (j < version2.length() && version2.charAt(j) != '.') b = b * 10 + version2.charAt(j++) - '0';
            if (a > b) return 1;
            else if (a < b) return -1;
            i++;
            j++;
        }
        return 0;
    }
}
```

__Python__:

```Python
class Solution:
    def compareVersion(self, version1: str, version2: str) -> int:
        v1, v2 = [1, *map(int, version1.split('.'))], [1, *map(int, version2.split('.'))]
        while not v1[-1]:
            v1.pop()
        while not v2[-1]:
            v2.pop()
        return v1 > v2 and 1 or v1 < v2 and -1 or 0
```
