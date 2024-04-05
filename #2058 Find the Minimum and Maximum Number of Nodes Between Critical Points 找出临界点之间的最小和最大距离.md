# 2058 Find the Minimum and Maximum Number of Nodes Between Critical Points 找出临界点之间的最小和最大距离

__Description:__

A critical point in a linked list is defined as either a local maxima or a local minima.

A node is a local maxima if the current node has a value strictly greater than the previous node and the next node.

A node is a local minima if the current node has a value strictly smaller than the previous node and the next node.

Note that a node can only be a local maxima/minima if there exists both a previous node and a next node.

Given a linked list `head`, return _an array of length 2 containing_ `[minDistance, maxDistance]` _where_ `minDistance` _is the __minimum distance__ between __any two distinct__ critical points and_ `maxDistance` _is the __maximum distance__ between __any two distinct__ critical points. If there are __fewer__ than two critical points, return_ `[-1, -1]`.

__Example:__

Example 1:

![2058-1](https://assets.leetcode.com/uploads/2021/10/13/a1.png)

```text
Input: head = [3,1]
Output: [-1,-1]
Explanation: There are no critical points in [3,1].
```

Example 2:

![2058-2](https://assets.leetcode.com/uploads/2021/10/13/a2.png)

```text
Input: head = [5,3,1,2,5,1,2]
Output: [1,3]
Explanation: There are three critical points:
```

- [5,3,1,2,5,1,2]: The third node is a local minima because 1 is less than 3 and 2.
- [5,3,1,2,5,1,2]: The fifth node is a local maxima because 5 is greater than 2 and 1.
- [5,3,1,2,5,1,2]: The sixth node is a local minima because 1 is less than 5 and 2.

The minimum distance is between the fifth and the sixth node. minDistance = 6 - 5 = 1.
The maximum distance is between the third and the sixth node. maxDistance = 6 - 3 = 3.

Example 3:

![2058-3](https://assets.leetcode.com/uploads/2021/10/14/a5.png)

```text
Input: head = [1,3,2,2,3,2,2,2,7]
Output: [3,3]
Explanation: There are two critical points:
```

- [1,3,2,2,3,2,2,2,7]: The second node is a local maxima because 3 is greater than 1 and 2.
- [1,3,2,2,3,2,2,2,7]: The fifth node is a local maxima because 3 is greater than 2 and 2.

Both the minimum and maximum distances are between the second and the fifth node.
Thus, minDistance and maxDistance is 5 - 2 = 3.
Note that the last node is not considered a local maxima because it does not have a next node.

__Constraints:__

- The number of nodes in the list is in the range `[2, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 5`

__题目描述:__

链表中的 临界点 定义为一个 局部极大值点 或 局部极小值点 。

如果当前节点的值 严格大于 前一个节点和后一个节点，那么这个节点就是一个  局部极大值点 。

如果当前节点的值 严格小于 前一个节点和后一个节点，那么这个节点就是一个  局部极小值点 。

注意：节点只有在同时存在前一个节点和后一个节点的情况下，才能成为一个 局部极大值点 / 极小值点 。

给你一个链表 `head` ，返回一个长度为 2 的数组 `[minDistance, maxDistance]` ，其中 `minDistance` 是任意两个不同临界点之间的最小距离， `maxDistance` 是任意两个不同临界点之间的最大距离。如果临界点少于两个，则返回 `[-1，-1]` 。

__示例:__

示例 1：

![2058-4](https://assets.leetcode.com/uploads/2021/10/13/a1.png)

```text
输入：head = [3,1]
输出：[-1,-1]
解释：链表 [3,1] 中不存在临界点。
```

示例 2：

![2058-5](https://assets.leetcode.com/uploads/2021/10/13/a2.png)

```text
输入：head = [5,3,1,2,5,1,2]
输出：[1,3]
解释：存在三个临界点：
```

- [5,3,1,2,5,1,2]：第三个节点是一个局部极小值点，因为 1 比 3 和 2 小。
- [5,3,1,2,5,1,2]：第五个节点是一个局部极大值点，因为 5 比 2 和 1 大。
- [5,3,1,2,5,1,2]：第六个节点是一个局部极小值点，因为 1 比 5 和 2 小。

第五个节点和第六个节点之间距离最小。minDistance = 6 - 5 = 1 。
第三个节点和第六个节点之间距离最大。maxDistance = 6 - 3 = 3 。

示例 3：

![2058-6](https://assets.leetcode.com/uploads/2021/10/14/a5.png)

```text
输入：head = [1,3,2,2,3,2,2,2,7]
输出：[3,3]
解释：存在两个临界点：
```

- [1,3,2,2,3,2,2,2,7]：第二个节点是一个局部极大值点，因为 3 比 1 和 2 大。
- [1,3,2,2,3,2,2,2,7]：第五个节点是一个局部极大值点，因为 3 比 2 和 2 大。

最小和最大距离都存在于第二个节点和第五个节点之间。
因此，minDistance 和 maxDistance 是 5 - 2 = 3 。
注意，最后一个节点不算一个局部极大值点，因为它之后就没有节点了。

示例 4：

![2058-7](https://assets.leetcode.com/uploads/2021/10/13/a4.png)

```text
输入：head = [2,3,3,2]
输出：[-1,-1]
解释：链表 [2,3,3,2] 中不存在临界点。
```

__提示：__

- 链表中节点的数量在范围 `[2, 10 ^ 5]` 内
- `1 <= Node.val <= 10 ^ 5`

__思路:__

```text
模拟
初始化所有变量为 -1, pos 为当前节点的位置
记录第一个临界点的位置 first, 最后一个临界点的位置 last
临界点为大于前一个节点和后一个节点的节点, 或者小于前一个节点和后一个节点的节点
遍历链表, 第一次遇到临界点时, 更新 first, 如果当前节点是临界点, 则更新 last
记录 pos - last 的最小值为 a, 最小值一定是两个相邻临界点之间的最小距离
记录 pos - first 的最大值为 b
时间复杂度为 O(N), 空间复杂度为 O(1)
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
    vector<int> nodesBetweenCriticalPoints(ListNode* head) 
    {
        int first = -1, last = -1, a = -1, b = -1, pos = -1;
        while (head -> next -> next) 
        {
            ++pos;
            if ((head -> next -> val > head -> val and head -> next -> val > head -> next -> next -> val) or (head -> next -> val < head -> val and head -> next -> val < head -> next -> next -> val))
            {
                if (last != -1) 
                {
                    a = a == -1 ? pos - last : min(a, pos - last);
                    b = max(b, pos - first);
                }
                if (first == -1) first = pos;
                last = pos;
            }
            head = head -> next;
        }
        return vector<int>{a, b};
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
    public int[] nodesBetweenCriticalPoints(ListNode head) {
        int first = -1, last = -1, a = -1, b = -1, pos = -1;
        while (head.next.next != null) {
            ++pos;
            if ((head.next.val > head.val && head.next.val > head.next.next.val) || (head.next.val < head.val && head.next.val < head.next.next.val)) {
                if (last != -1) {
                    a = a == -1 ? pos - last : Math.min(a, pos - last);
                    b = Math.max(b, pos - first);
                }
                if (first == -1) first = pos;
                last = pos;
            }
            head = head.next;
        }
        return new int[]{a, b};
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
    def nodesBetweenCriticalPoints(self, head: Optional[ListNode]) -> List[int]:
        a = b = first = last = pos = -1
        while head.next.next:
            pos += 1
            if head.next.val > max(head.val, head.next.next.val) or head.next.val < min(head.val, head.next.next.val):
                if last != -1:
                    a, b = (pos - last if a == -1 else min(a, pos - last)), max(b, pos - first)
                if first == -1:
                    first = pos
                last = pos
            head = head.next
        return [a, b]
```
