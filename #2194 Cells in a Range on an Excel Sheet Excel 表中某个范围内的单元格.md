# 2194 Cells in a Range on an Excel Sheet Excel 表中某个范围内的单元格

__Description:__

A cell `(r, c)` of an excel sheet is represented as a string `"<col><row>"` where:

- `<col>` denotes the column number `c` of the cell. It is represented by __alphabetical letters__.

- For example, the `1 ^ st` column is denoted by `'A'`, the `2 ^ nd` by `'B'`, the `3 ^ rd` by `'C'`, and so on.
- `<row>` is the row number `r` of the cell. The `r ^ th` row is represented by the __integer__ `r`.

- For example, the `1 ^ st` column is denoted by `'A'`, the `2 ^ nd` by `'B'`, the `3 ^ rd` by `'C'`, and so on.

You are given a string `s` in the format `"<col1><row1>:<col2><row2>"`, where `<col1>` represents the column `c1`, `<row1>` represents the row `r1`, `<col2>` represents the column `c2`, and `<row2>` represents the row `r2`, such that `r1 <= r2` and `c1 <= c2`.

Return _the __list of cells___ `(x, y)` _such that_ `r1 <= x <= r2` _and_ `c1 <= y <= c2`. The cells should be represented as __strings__ in the format mentioned above and be sorted in __non-decreasing__ order first by columns and then by rows.

__Example:__

Example 1:

![2194-1](https://assets.leetcode.com/uploads/2022/02/08/ex1drawio.png)

```text
Input: s = "K1:L2"
Output: ["K1","K2","L1","L2"]
Explanation:
The above diagram shows the cells which should be present in the list.
The red arrows denote the order in which the cells should be presented.
```

Example 2:

![2194-2](https://assets.leetcode.com/uploads/2022/02/09/exam2drawio.png)

```text
Input: s = "A1:F1"
Output: ["A1","B1","C1","D1","E1","F1"]
Explanation:
The above diagram shows the cells which should be present in the list.
The red arrow denotes the order in which the cells should be presented.
```

__Constraints:__

- `s.length == 5`
- `'A' <= s[0] <= s[3] <= 'Z'`
- `'1' <= s[1] <= s[4] <= '9'`
- `s` consists of uppercase English letters, digits and `':'`.

__题目描述:__

Excel 表中的一个单元格 `(r, c)` 会以字符串 `"<col><row>"` 的形式进行表示，其中:

- `<col>` 即单元格的列号 `c` 。用英文字母表中的 __字母__ 标识。

- 例如，第 `1` 列用 `'A'` 表示，第 `2` 列用 `'B'` 表示，第 `3` 列用 `'C'` 表示，以此类推。
- `<row>` 即单元格的行号 `r` 。第 `r` 行就用 __整数__ `r` 标识。

- 例如，第 `1` 列用 `'A'` 表示，第 `2` 列用 `'B'` 表示，第 `3` 列用 `'C'` 表示，以此类推。

给你一个格式为 `"<col1><row1>:<col2><row2>"` 的字符串 `s` ，其中 `<col1>` 表示 `c1` 列， `<row1>` 表示 `r1` 行， `<col2>` 表示 `c2` 列， `<row2>` 表示 `r2` 行，并满足 `r1 <= r2` 且 `c1 <= c2` 。

找出所有满足 `r1 <= x <= r2` 且 `c1 <= y <= c2` 的单元格，并以列表形式返回。单元格应该按前面描述的格式用 __字符串__ 表示，并以 __非递减__ 顺序排列（先按列排，再按行排）。

__示例:__

示例 1：

![2194-3](https://assets.leetcode.com/uploads/2022/02/08/ex1drawio.png)

```text
输入：s = "K1:L2"
输出：["K1","K2","L1","L2"]
解释：
上图显示了列表中应该出现的单元格。
红色箭头指示单元格的出现顺序。
```

示例 2：

![2194-4](https://assets.leetcode.com/uploads/2022/02/09/exam2drawio.png)

```text
输入：s = "A1:F1"
输出：["A1","B1","C1","D1","E1","F1"]
解释：
上图显示了列表中应该出现的单元格。 
红色箭头指示单元格的出现顺序。
```

__提示：__

- `s.length == 5`
- `'A' <= s[0] <= s[3] <= 'Z'`
- `'1' <= s[1] <= s[4] <= '9'`
- `s` 由大写英文字母、数字、和 `':'` 组成

__思路:__

```text
模拟
两层模拟均使用 char 类型进行遍历
遍历列号和行号, 将其转换为 string 类型
添加到结果中
时间复杂度为 O(N ^ 2), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<string> cellsInRange(string s) 
    {
        vector<string> result;
        for (char a = s[0]; a <= s[3]; a++) for (char b = s[1]; b <= s[4]; b++) result.emplace_back(string(1, a) + string(1, b));
        return result;    
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> cellsInRange(String s) {
        List<String> result = new ArrayList<>();
        for (char a = s.charAt(0); a <= s.charAt(3); a++) for (char b = s.charAt(1); b <= s.charAt(4); b++) result.add(String.valueOf(a) + String.valueOf(b));
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def cellsInRange(self, s: str) -> List[str]:
        return [chr(c) + chr(r) for c in range(ord(s[0]), ord(s[3]) + 1) for r in range(ord(s[1]), ord(s[4]) + 1)]
```
