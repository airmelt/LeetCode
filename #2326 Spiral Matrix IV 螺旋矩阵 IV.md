# 2326 Spiral Matrix IV 螺旋矩阵 IV

__Description:__

You are given two integers `m` and `n`, which represent the dimensions of a matrix.

You are also given the `head` of a linked list of integers.

Generate an `m x n` matrix that contains the integers in the linked list presented in __spiral__ order __(clockwise)__, starting from the __top-left__ of the matrix. If there are remaining empty spaces, fill them with `-1`.

Return the generated matrix.

__Example:__

Example 1:

![2326-1](https://assets.leetcode.com/uploads/2022/05/09/ex1new.jpg)

```text
Input: m = 3, n = 5, head = [3,0,2,6,8,1,7,9,4,2,5,5,0]
Output: [[3,0,2,6,8],[5,0,-1,-1,1],[5,2,4,9,7]]
Explanation: The diagram above shows how the values are printed in the matrix.
Note that the remaining spaces in the matrix are filled with -1.
```

Example 2:

![2326-2](https://assets.leetcode.com/uploads/2022/05/11/ex2.jpg)

```text
Input: m = 1, n = 4, head = [0,1,2]
Output: [[0,1,2,-1]]
Explanation: The diagram above shows how the values are printed from left to right in the matrix.
The last space in the matrix is set to -1.
```

__Constraints:__

- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- The number of nodes in the list is in the range `[1, m * n]`.
- `0 <= Node.val <= 1000`

__题目描述:__

给你两个整数: `m` 和 `n` ，表示矩阵的维数。

另给你一个整数链表的头节点 `head` 。

请你生成一个大小为 `m x n` 的螺旋矩阵，矩阵包含链表中的所有整数。链表中的整数从矩阵 __左上角__ 开始、__顺时针__ 按 __螺旋__ 顺序填充。如果还存在剩余的空格，则用 `-1` 填充。

返回生成的矩阵。

__示例:__

示例 1：

![2326-3](https://assets.leetcode.com/uploads/2022/05/09/ex1new.jpg)

```text
输入：m = 3, n = 5, head = [3,0,2,6,8,1,7,9,4,2,5,5,0]
输出：[[3,0,2,6,8],[5,0,-1,-1,1],[5,2,4,9,7]]
解释：上图展示了链表中的整数在矩阵中是如何排布的。
注意，矩阵中剩下的空格用 -1 填充。
```

示例 2：

![2326-4](https://assets.leetcode.com/uploads/2022/05/11/ex2.jpg)

```text
输入：m = 1, n = 4, head = [0,1,2]
输出：[[0,1,2,-1]]
解释：上图展示了链表中的整数在矩阵中是如何从左到右排布的。 
注意，矩阵中剩下的空格用 -1 填充。
```

__提示：__

- `1 <= m, n <= 10 ^ 5`
- `1 <= m * n <= 10 ^ 5`
- 链表中节点数目在范围 `[1, m * n]` 内
- `0 <= Node.val <= 1000`

__思路:__

```text
模拟
给定 4 个方向
每次遍历链表的时候, 判断下一个位置是否越界或者已经被填充
如果是, 则更换方向
更换方向的时候, 可以使用取模的方式
否则, 继续填充
时间复杂度为 O(MN), 空间复杂度为 O(1), 不计返回的结果矩阵
```

__代码:__

__C++__:

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution 
{
public:
    vector<vector<int>> spiralMatrix(int m, int n, ListNode* head) 
    {
        vector<vector<int>> result(m, vector<int>(n, -1));
        int d[4][2]{{0, 1}, {1, 0}, {0, -1}, {-1, 0}}, x = 0, y = 0, i = 0, dx = 0, dy = 1;
        while (head) 
        {
            result[x][y] = head -> val;
            head = head -> next;
            if (!(x + dx > -1 && x + dx < m && y + dy > -1 && y + dy < n and result[x + dx][y + dy] == -1)) 
            {
                i = (i + 1) % 4;
                dx = d[i][0];
                dy = d[i][1];
            }
            x += dx;
            y += dy;
        }
        return result;
    }
};
```

__Java__:

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public int[][] spiralMatrix(int m, int n, ListNode head) {
        int d[][] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}}, result[][] = new int[m][n], x = 0, y = 0, i = 0, dx = 0, dy = 1;
        for (int t = 0; t < m; t++) Arrays.fill(result[t], -1);
        while (head != null) {
            result[x][y] = head.val;
            head = head.next;
            if (!(x + dx > -1 && x + dx < m && y + dy > -1 && y + dy < n && result[x + dx][y + dy] == -1)) {
                i = (i + 1) % 4;
                dx = d[i][0];
                dy = d[i][1];
            }
            x += dx;
            y += dy;
        }
        return result;
    }
}
```

__Python__:

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def spiralMatrix(self, m: int, n: int, head: Optional[ListNode]) -> List[List[int]]:
        d, result, x, y, i = [(0, 1), (1, 0), (0, -1), (-1, 0)], [[-1] * n for _ in range(m)], 0, 0, 0
        while head:
            result[x][y], head = head.val, head.next
            dx, dy = d[i]
            if not (-1 < x + dx < m and -1 < y + dy < n and result[x + dx][y + dy] == -1):
                dx, dy = d[(i := (i + 1) % 4)]
            x += dx
            y += dy
        return result
```
