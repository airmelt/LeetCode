# 2125 Number of Laser Beams in a Bank 银行中的激光束数量

__Description:__

Anti-theft security devices are activated inside a bank. You are given a __0-indexed__ binary string array `bank` representing the floor plan of the bank, which is an `m x n` 2D matrix. `bank[i]` represents the `i ^ th` row, consisting of `'0'`s and `'1'`s. `'0'` means the cell is empty, while `'1'` means the cell has a security device.

There is one laser beam between any two security devices if both conditions are met:

- The two devices are located on two __different rows__: `r1` and `r2`, where `r1 < r2`.
- For __each__ row `i` where `r1 < i < r2`, there are __no security devices__ in the `i ^ th` row.

Laser beams are independent, i.e., one beam does not interfere nor join with another.

Return the total number of laser beams in the bank.

__Example:__

Example 1:

![2125-1](https://assets.leetcode.com/uploads/2021/12/24/laser1.jpg)

```text
Input: bank = ["011001","000000","010100","001000"]
Output: 8
Explanation: Between each of the following device pairs, there is one beam. In total, there are 8 beams:
 * bank[0][1] -- bank[2][1]
 * bank[0][1] -- bank[2][3]
 * bank[0][2] -- bank[2][1]
 * bank[0][2] -- bank[2][3]
 * bank[0][5] -- bank[2][1]
 * bank[0][5] -- bank[2][3]
 * bank[2][1] -- bank[3][2]
 * bank[2][3] -- bank[3][2]
Note that there is no beam between any device on the 0th row with any on the 3rd row.
This is because the 2nd row contains security devices, which breaks the second condition.
```

Example 2:

![2125-2](https://assets.leetcode.com/uploads/2021/12/24/laser2.jpg)

```text
Input: bank = ["000","111","000"]
Output: 0
Explanation: There does not exist two devices located on two different rows.
```

__Constraints:__

- `m == bank.length`
- `n == bank[i].length`
- `1 <= m, n <= 500`
- `bank[i][j]` is either `'0'` or `'1'`.

__题目描述:__

银行内部的防盗安全装置已经激活。给你一个下标从 __0__ 开始的二进制字符串数组 `bank` ，表示银行的平面图，这是一个大小为 `m x n` 的二维矩阵。 `bank[i]` 表示第 `i` 行的设备分布，由若干 `'0'` 和若干 `'1'` 组成。 `'0'` 表示单元格是空的，而 `'1'` 表示单元格有一个安全设备。

对任意两个安全设备而言，如果同时 满足下面两个条件，则二者之间存在 一个 激光束：

- 两个设备位于两个 __不同行__ : `r1` 和 `r2` ，其中 `r1 < r2` 。
- 满足 `r1 < i < r2` 的 __所有__ 行 `i` ，都 __没有安全设备__ 。

激光束是独立的，也就是说，一个激光束既不会干扰另一个激光束，也不会与另一个激光束合并成一束。

返回银行中激光束的总数量。

__示例:__

示例 1：

![2125-3](https://assets.leetcode.com/uploads/2021/12/24/laser1.jpg)

```text
输入：bank = ["011001","000000","010100","001000"]
输出：8
解释：在下面每组设备对之间，存在一条激光束。总共是 8 条激光束：
 * bank[0][1] -- bank[2][1]
 * bank[0][1] -- bank[2][3]
 * bank[0][2] -- bank[2][1]
 * bank[0][2] -- bank[2][3]
 * bank[0][5] -- bank[2][1]
 * bank[0][5] -- bank[2][3]
 * bank[2][1] -- bank[3][2]
 * bank[2][3] -- bank[3][2]
注意，第 0 行和第 3 行上的设备之间不存在激光束。
这是因为第 2 行存在安全设备，这不满足第 2 个条件。
```

示例 2：

![2125-4](https://assets.leetcode.com/uploads/2021/12/24/laser2.jpg)

```text
输入：bank = ["000","111","000"]
输出：0
解释：不存在两个位于不同行的设备
```

__提示：__

- `m == bank.length`
- `n == bank[i].length`
- `1 <= m, n <= 500`
- `bank[i][j]` 为 `'0'` 或 `'1'`

__思路:__

```text
模拟
统计每一行的激光数量
如果为 0 跳过该行
否则加上该行与前一行的激光数量的乘积
时间复杂度为 O(MN), 空间复杂度为 O(1), 其中 M 为字符串的长度
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int numberOfBeams(vector<string>& bank) 
    {
        int last = 0, result = 0, c = 0;
        for (const auto& line: bank) 
        {
            if (c = count_if(line.begin(), line.end(), [](char ch) { return ch == '1'; })) 
            {
                result += last * c;
                last = c;
            }
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfBeams(String[] bank) {
        int result = 0, last = 0, c = 0;
        for (String line : bank) {
            if ((c = helper(line)) != 0) {
                result += last * c;
                last = c;
            }
        }
        return result;
    }

    private int helper(String line) {
        int result = 0;
        for (char c : line.toCharArray()) if (c == '1') ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfBeams(self, bank: List[str]) -> int:
        return sum(x.count('1') * y.count('1') for x, y in pairwise([line for line in bank if '1' in line]))
```
