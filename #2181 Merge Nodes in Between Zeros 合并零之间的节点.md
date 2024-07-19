# 2181 Merge Nodes in Between Zeros 合并零之间的节点

__Description:__

You are given the `head` of a linked list, which contains a series of integers __separated__ by `0`'s. The __beginning__ and __end__ of the linked list will have `Node.val == 0`.

For __every__ two consecutive `0`'s, __merge__ all the nodes lying in between them into a single node whose value is the __sum__ of all the merged nodes. The modified list should not contain any `0`'s.

Return _the_ `head` _of the modified linked list_.

__Example:__

Example 1:

![2181-1](https://assets.leetcode.com/uploads/2022/02/02/ex1-1.png)

```text
Input: head = [0,3,1,0,4,5,2,0]
Output: [4,11]
Explanation: 
The above figure represents the given linked list. The modified list contains
```

- The sum of the nodes marked in green: 3 + 1 = 4.
- The sum of the nodes marked in red: 4 + 5 + 2 = 11.

Example 2:

![2181-2](https://assets.leetcode.com/uploads/2022/02/02/ex2-1.png)

```text
Input: head = [0,1,0,3,0,2,2,0]
Output: [1,3,4]
Explanation: 
The above figure represents the given linked list. The modified list contains
```

- The sum of the nodes marked in green: 1 = 1.
- The sum of the nodes marked in red: 3 = 3.
- The sum of the nodes marked in yellow: 2 + 2 = 4.

__Constraints:__

- The number of nodes in the list is in the range `[3, 2 * 10 ^ 5]`.
- `0 <= Node.val <= 1000`
- There are __no__ two consecutive nodes with `Node.val == 0`.
- The __beginning__ and __end__ of the linked list have `Node.val == 0`.

__题目描述:__

给你一个链表的头节点 `head` ，该链表包含由 `0` 分隔开的一连串整数。链表的 __开端__ 和 __末尾__ 的节点都满足 `Node.val == 0` 。

对于每两个相邻的 `0` ，请你将它们之间的所有节点合并成一个节点，其值是所有已合并节点的值之和。然后将所有 `0` 移除，修改后的链表不应该含有任何 `0` 。

返回修改后链表的头节点 `head` 。

__示例:__

示例 1：

![2181-3](https://assets.leetcode.com/uploads/2022/02/02/ex1-1.png)

```text
输入：head = [0,3,1,0,4,5,2,0]
输出：[4,11]
解释：
上图表示输入的链表。修改后的链表包含：
```

- 标记为绿色的节点之和：3 + 1 = 4
- 标记为红色的节点之和：4 + 5 + 2 = 11

示例 2：

![2181-4](https://assets.leetcode.com/uploads/2022/02/02/ex2-1.png)

```text
输入：head = [0,1,0,3,0,2,2,0]
输出：[1,3,4]
解释：
上图表示输入的链表。修改后的链表包含：
```

- 标记为绿色的节点之和：1 = 1
- 标记为红色的节点之和：3 = 3
- 标记为黄色的节点之和：2 + 2 = 4

__提示：__

- 列表中的节点数目在范围 `[3, 2 * 10 ^ 5]` 内
- `0 <= Node.val <= 1000`
- __不__ 存在连续两个 `Node.val == 0` 的节点
- 链表的 __开端__ 和 __末尾__ 节点都满足 `Node.val == 0`

__思路:__

```text
模拟
q 初始化为 head, p 初始化为 head.next
注意到 head 的值为 0
从 head.next 开始遍历链表
如果当前值不为 0 则累加
否则将累加值赋给 q 的后一个节点, 并将累加值清零, 并移动 q 节点
最后需要将 q 的 next 节点置为 nullptr
返回 head 的 next 节点
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
    ListNode* mergeNodes(ListNode* head) 
    {
        ListNode *q = head, *p = head -> next;
        for (int cur = 0; p; p = p -> next)
        {
            if (p -> val > 0) cur += p -> val;
            else
            {
                q -> next -> val = cur;
                q = q -> next;
                cur = 0;
            }
        }    
        q -> next = nullptr;
        return head -> next;
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
    public ListNode mergeNodes(ListNode head) {
        ListNode p = head.next, q = head;
        for (int cur = 0; p != null; p = p.next) {
            if (p.val > 0) cur += p.val;
            else {
                q.next.val = cur;
                q = q.next;
                cur = 0;
            }
        }
        q.next = null;
        return head.next;
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
    def mergeNodes(self, head: Optional[ListNode]) -> Optional[ListNode]:
        p, q, cur = head, head, 0
        while (p := p.next):
            if p.val:
                cur += p.val
            else:
                q.next.val, q, cur = cur, q.next, 0
        q.next = None
        return head.next
```
