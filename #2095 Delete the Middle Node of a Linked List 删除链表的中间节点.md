# 2095 Delete the Middle Node of a Linked List 删除链表的中间节点

__Description:__

You are given the `head` of a linked list. __Delete__ the __middle node__, and return _the_ `head` _of the modified linked list_.

The __middle node__ of a linked list of size `n` is the `⌊n / 2⌋ ^ th` node from the _start_ using __0-based indexing__, where `⌊x⌋` denotes the largest integer less than or equal to `x`.

- For `n` = `1`, `2`, `3`, `4`, and `5`, the middle nodes are `0`, `1`, `1`, `2`, and `2`, respectively.

__Example:__

Example 1:

![2095-1](https://assets.leetcode.com/uploads/2021/11/16/eg1drawio.png)

```text
Input: head = [1,3,4,7,1,2,6]
Output: [1,3,4,1,2,6]
Explanation:
The above figure represents the given linked list. The indices of the nodes are written below.
Since n = 7, node 3 with value 7 is the middle node, which is marked in red.
We return the new list after removing this node.
```

Example 2:

![2095-2](https://assets.leetcode.com/uploads/2021/11/16/eg2drawio.png)

```text
Input: head = [1,2,3,4]
Output: [1,2,4]
Explanation:
The above figure represents the given linked list.
For n = 4, node 2 with value 3 is the middle node, which is marked in red.
```

Example 3:

![2095-3](https://assets.leetcode.com/uploads/2021/11/16/eg3drawio.png)

```text
Input: head = [2,1]
Output: [2]
Explanation:
The above figure represents the given linked list.
For n = 2, node 1 with value 1 is the middle node, which is marked in red.
Node 0 with value 2 is the only node remaining after removing node 1.
```

__Constraints:__

- The number of nodes in the list is in the range `[1, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 5`

__题目描述:__

给你一个链表的头节点 `head` 。__删除__ 链表的 __中间节点__ ，并返回修改后的链表的头节点 `head` 。

长度为 `n` 链表的中间节点是从头数起第 `⌊n / 2⌋` 个节点（下标从 __0__ 开始），其中 `⌊x⌋` 表示小于或等于 `x` 的最大整数。

- 对于 `n` = `1`、 `2`、 `3`、 `4` 和 `5` 的情况，中间节点的下标分别是 `0`、 `1`、 `1`、 `2` 和 `2` 。

__示例:__

示例 1：

![2095-4](https://assets.leetcode.com/uploads/2021/11/16/eg1drawio.png)

```text
输入：head = [1,3,4,7,1,2,6]
输出：[1,3,4,1,2,6]
解释：
上图表示给出的链表。节点的下标分别标注在每个节点的下方。
由于 n = 7 ，值为 7 的节点 3 是中间节点，用红色标注。
返回结果为移除节点后的新链表。
```

示例 2：

![2095-5](https://assets.leetcode.com/uploads/2021/11/16/eg2drawio.png)

```text
输入：head = [1,2,3,4]
输出：[1,2,4]
解释：
上图表示给出的链表。
对于 n = 4 ，值为 3 的节点 2 是中间节点，用红色标注。
```

示例 3：

![2095-6](https://assets.leetcode.com/uploads/2021/11/16/eg3drawio.png)

```text
输入：head = [2,1]
输出：[2]
解释：
上图表示给出的链表。
对于 n = 2 ，值为 1 的节点 1 是中间节点，用红色标注。
值为 2 的节点 0 是移除节点 1 后剩下的唯一一个节点。
```

__提示：__

- 链表中节点的数目在范围 `[1, 10 ^ 5]` 内
- `1 <= Node.val <= 10 ^ 5`

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
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
class Solution {
public:
    ListNode* deleteMiddle(ListNode* head) {
        
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
    public ListNode deleteMiddle(ListNode head) {

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
    def deleteMiddle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head.next is None:
            return None
        slow, fast, pre = head, head, None
        while fast and fast.next:
            fast = fast.next.next
            pre = slow
            slow = slow.next
        pre.next = pre.next.next
        return head
```
