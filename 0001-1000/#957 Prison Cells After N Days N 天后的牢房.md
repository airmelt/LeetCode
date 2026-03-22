# 957 Prison Cells After N Days N 天后的牢房

__Description__:
There are 8 prison cells in a row and each cell is either occupied or vacant.

Each day, whether the cell is occupied or vacant changes according to the following rules:

If a cell has two adjacent neighbors that are both occupied or both vacant, then the cell becomes occupied.
Otherwise, it becomes vacant.
Note that because the prison is a row, the first and the last cells in the row can't have two adjacent neighbors.

You are given an integer array cells where cells[i] == 1 if the ith cell is occupied and cells[i] == 0 if the ith cell is vacant, and you are given an integer n.

Return the state of the prison after n days (i.e., n such changes described above).

__Example:__

Example 1:

Input: cells = [0,1,0,1,1,0,0,1], n = 7
Output: [0,0,1,1,0,0,0,0]
Explanation: The following table summarizes the state of the prison on each day:
Day 0: [0, 1, 0, 1, 1, 0, 0, 1]
Day 1: [0, 1, 1, 0, 0, 0, 0, 0]
Day 2: [0, 0, 0, 0, 1, 1, 1, 0]
Day 3: [0, 1, 1, 0, 0, 1, 0, 0]
Day 4: [0, 0, 0, 0, 0, 1, 0, 0]
Day 5: [0, 1, 1, 1, 0, 1, 0, 0]
Day 6: [0, 0, 1, 0, 1, 1, 0, 0]
Day 7: [0, 0, 1, 1, 0, 0, 0, 0]

Example 2:

Input: cells = [1,0,0,1,0,0,1,0], n = 1000000000
Output: [0,0,1,1,1,1,1,0]

__Constraints:__

cells.length == 8
cells[i] is either 0 or 1.
1 <= n <= 10^9

__题目描述__:
8 间牢房排成一排，每间牢房不是有人住就是空着。

每天，无论牢房是被占用或空置，都会根据以下规则进行更改：

如果一间牢房的两个相邻的房间都被占用或都是空的，那么该牢房就会被占用。
否则，它就会被空置。
（请注意，由于监狱中的牢房排成一行，所以行中的第一个和最后一个房间无法有两个相邻的房间。）

我们用以下方式描述监狱的当前状态：如果第 i 间牢房被占用，则 cell[i]==1，否则 cell[i]==0。

根据监狱的初始状态，在 N 天后返回监狱的状况（和上述 N 种变化）。

__示例 :__

示例 1：

输入：cells = [0,1,0,1,1,0,0,1], N = 7
输出：[0,0,1,1,0,0,0,0]
解释：
下表概述了监狱每天的状况：
Day 0: [0, 1, 0, 1, 1, 0, 0, 1]
Day 1: [0, 1, 1, 0, 0, 0, 0, 0]
Day 2: [0, 0, 0, 0, 1, 1, 1, 0]
Day 3: [0, 1, 1, 0, 0, 1, 0, 0]
Day 4: [0, 0, 0, 0, 0, 1, 0, 0]
Day 5: [0, 1, 1, 1, 0, 1, 0, 0]
Day 6: [0, 0, 1, 0, 1, 1, 0, 0]
Day 7: [0, 0, 1, 1, 0, 0, 0, 0]

示例 2：

输入：cells = [1,0,0,1,0,0,1,0], N = 1000000000
输出：[0,0,1,1,1,1,1,0]

__提示:__

cells.length == 8
cells[i] 的值为 0 或 1
1 <= N <= 10^9

__思路__:

模拟
猜测有周期, 尝试打印出一个周期, 可以找到周期为 14
将 N 压缩到 14 以内
循环执行 cell = ~(cell << 1 ^ cell >> 1) & 0x7E, 最后转化为列表即可
或者直接按题意模拟
时间复杂度为 O(1), 空间复杂度为 O(1), 最多只需要执行 14 次变换, 数组长度为 8

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> prisonAfterNDays(vector<int>& cells, int n) 
    {
        n = (n - 1) % 14 + 1;
        while (n--) helper(cells);
        return cells;
    }
private:
    void helper(vector<int>& cells) 
    {
        vector<int> result(8, 0);
        for (int i = 1; i < 7; i++) if (cells[i - 1] == cells[i + 1]) result[i] = 1;
        cells = result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] prisonAfterNDays(int[] cells, int n) {
        n = (n - 1) % 14 + 1;
        while (n-- != 0) cells = helper(cells);
        return cells;
    }

    private int[] helper(int[] cells) {
        int[] result = new int[8];
        for (int i = 1; i < 7; i++) if (cells[i - 1] == cells[i + 1]) result[i] = 1;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def prisonAfterNDays(self, cells: List[int], n: int) -> List[int]:
        n, cell = (n - 1) % 14 + 1, sum([cells[i] * (2 ** (7 - i)) for i in range(len(cells))])
        for _ in range(n):
            cell = ~(cell << 1 ^ cell >> 1) & 0x7E
        return list(map(int, list('{:08b}'.format(cell))))
```
