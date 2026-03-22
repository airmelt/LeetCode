# 2074 Reverse Nodes in Even Length Groups 反转偶数长度组的节点

__Description:__

You are given the `head` of a linked list.

The nodes in the linked list are __sequentially__ assigned to __non-empty__ groups whose lengths form the sequence of the natural numbers ( `1, 2, 3, 4, ...`). The __length__ of a group is the number of nodes assigned to it. In other words,

- The `1 ^ st` node is assigned to the first group.
- The `2 ^ nd` and the `3 ^ rd` nodes are assigned to the second group.
- The `4 ^ th`, `5 ^ th`, and `6 ^ th` nodes are assigned to the third group, and so on.

Note that the length of the last group may be less than or equal to `1 + the length of the second to last group`.

__Reverse__ the nodes in each group with an __even__ length, and return _the_ `head` _of the modified linked list_.

__Example:__

Example 1:

![2074-1](https://assets.leetcode.com/uploads/2021/10/25/eg1.png)

```text
Input: head = [5,2,6,3,9,1,7,3,8,4]
Output: [5,6,2,3,9,1,4,8,3,7]
Explanation:
- The length of the first group is 1, which is odd, hence no reversal occurs.
- The length of the second group is 2, which is even, hence the nodes are reversed.
- The length of the third group is 3, which is odd, hence no reversal occurs.
- The length of the last group is 4, which is even, hence the nodes are reversed.
```

Example 2:

![2074-2](https://assets.leetcode.com/uploads/2021/10/25/eg2.png)

```text
Input: head = [1,1,0,6]
Output: [1,0,1,6]
Explanation:
- The length of the first group is 1. No reversal occurs.
- The length of the second group is 2. The nodes are reversed.
- The length of the last group is 1. No reversal occurs.
```

Example 3:

![2074-3](https://assets.leetcode.com/uploads/2021/11/17/ex3.png)

```text
Input: head = [1,1,0,6,5]
Output: [1,0,1,5,6]
Explanation:
- The length of the first group is 1. No reversal occurs.
- The length of the second group is 2. The nodes are reversed.
- The length of the last group is 2. The nodes are reversed.
```

__Constraints:__

- The number of nodes in the list is in the range `[1, 10 ^ 5]`.
- `0 <= Node.val <= 10 ^ 5`

__题目描述:__

给你一个链表的头节点 `head` 。

链表中的节点 __按顺序__ 划分成若干 __非空__ 组，这些非空组的长度构成一个自然数序列（ `1, 2, 3, 4, ...`）。一个组的 __长度__ 就是组中分配到的节点数目。换句话说:

- 节点 `1` 分配给第一组
- 节点 `2` 和 `3` 分配给第二组
- 节点 `4`、 `5` 和 `6` 分配给第三组，以此类推

注意，最后一组的长度可能小于或者等于 `1 + 倒数第二组的长度` 。

__反转__ 每个 __偶数__ 长度组中的节点，并返回修改后链表的头节点 `head` 。

__示例:__

示例 1：

![2074-4](https://assets.leetcode.com/uploads/2021/10/25/eg1.png)

```text
输入：head = [5,2,6,3,9,1,7,3,8,4]
输出：[5,6,2,3,9,1,4,8,3,7]
解释：
- 第一组长度为 1 ，奇数，没有发生反转。
- 第二组长度为 2 ，偶数，节点反转。
- 第三组长度为 3 ，奇数，没有发生反转。
- 最后一组长度为 4 ，偶数，节点反转。
```

示例 2：

![2074-5](https://assets.leetcode.com/uploads/2021/10/25/eg2.png)

```text
输入：head = [1,1,0,6]
输出：[1,0,1,6]
解释：
- 第一组长度为 1 ，没有发生反转。
- 第二组长度为 2 ，节点反转。
- 最后一组长度为 1 ，没有发生反转。
```

示例 3：

![2074-6](https://assets.leetcode.com/uploads/2021/10/28/eg3.png)

```text
输入：head = [2,1]
输出：[2,1]
解释：
- 第一组长度为 1 ，没有发生反转。
- 最后一组长度为 1 ，没有发生反转。
```

__提示：__

- 链表中节点数目范围是 `[1, 10 ^ 5]`
- `0 <= Node.val <= 10 ^ 5`

__思路:__

```text
模拟
特别注意这里要求的是偶数长度的链表反转
用 i 表示第 i 轮的链表长度, 每轮 i 自增
用 s 表示当前链表长度, 遍历链表 s 自增
用 cur 和 pre 作为每轮的工作指针, 分别指向需要修改的当前节点和前一个节点
用 p 作为遍历指针, 每轮遍历 s 次直到链表末尾
当 s 为奇数时, 用 pre 和 cur 指针直接移动
当 s 为偶数时, 用 pre, cur 和 p 指针进行节点反转
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
    ListNode* reverseEvenLengthGroups(ListNode* head) 
    {
        int i = 0, s = 0;
        ListNode *cur = head, *pre = nullptr, *p = nullptr;
        while (cur)
        {
            ++i;
            p = cur;
            for (s = 0; s < i and p; s++) p = p -> next;
            if (s & 1) while (s-- > 1) tie(pre, cur) = tuple{cur, cur -> next};
            else while (s-- > 1) tie(pre -> next, cur -> next, cur -> next -> next) = tuple{cur -> next, cur -> next -> next, pre -> next};
            tie(pre, cur) = tuple{cur, cur -> next};
        }
        return head;
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
    public ListNode reverseEvenLengthGroups(ListNode head) {
        int i = 0, s = 0;
        ListNode pre = null, cur = head, p = cur;
        while (cur != null) {
            p = cur;
            ++i;
            for (s = 0; s < i && p != null; s++) p = p.next;
            if ((s & 1) == 1) {
                while (s-- > 1) {
                    pre = cur;
                    cur = cur.next;
                }
            } else {
                while (s-- > 1) {
                    p = cur.next;
                    cur.next = p.next;
                    p.next = pre.next;
                    pre.next = p;
                }
            }
            pre = cur;
            cur = cur.next;
        }
        return head;
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
    def reverseEvenLengthGroups(self, head: Optional[ListNode]) -> Optional[ListNode]:
        i, cur, pre = 0, head, None
        while cur:
            i += 1
            p, s = cur, 0
            while s < i and p:
                s += 1
                p = p.next
            if s & 1:
                for _ in range(s - 1):
                    pre, cur = cur, cur.next
            else:
                for _ in range(s - 1):
                    pre.next, cur.next.next, cur.next = cur.next, pre.next, cur.next.next
            pre, cur = cur, cur.next
        return head
```
