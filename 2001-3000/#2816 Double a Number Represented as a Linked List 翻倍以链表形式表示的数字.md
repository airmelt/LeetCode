# 2816 Double a Number Represented as a Linked List 翻倍以链表形式表示的数字

__Description:__

You are given the `head` of a __non-empty__ linked list representing a non-negative integer without leading zeroes.

Return _the_ `head` _of the linked list after __doubling__ it_.

__Example:__

Example 1:

![2816-1](https://assets.leetcode.com/uploads/2023/05/28/example.png)

```text
Input: head = [1,8,9]
Output: [3,7,8]
Explanation: The figure above corresponds to the given linked list which represents the number 189. Hence, the returned linked list represents the number 189 * 2 = 378.
```

Example 2:

![2816-2](https://assets.leetcode.com/uploads/2023/05/28/example2.png)

```text
Input: head = [9,9,9]
Output: [1,9,9,8]
Explanation: The figure above corresponds to the given linked list which represents the number 999. Hence, the returned linked list represents the number 999 * 2 = 1998.
```

__Constraints:__

- The number of nodes in the list is in the range `[1, 10 ^ 4]`
- `0 <= Node.val <= 9`
- The input is generated such that the list represents a number that does not have leading zeros, except the number `0` itself.

__题目描述:__

给你一个 __非空__ 链表的头节点 `head` ，表示一个不含前导零的非负数整数。

将链表 __翻倍__ 后，返回头节点 `head` 。

__示例:__

示例 1：

![2816-3](https://assets.leetcode.com/uploads/2023/05/28/example.png)

```text
输入：head = [1,8,9]
输出：[3,7,8]
解释：上图中给出的链表，表示数字 189 。返回的链表表示数字 189 * 2 = 378 。
```

示例 2：

![2816-4](https://assets.leetcode.com/uploads/2023/05/28/example2.png)

```text
输入：head = [9,9,9]
输出：[1,9,9,8]
解释：上图中给出的链表，表示数字 999 。返回的链表表示数字 999 * 2 = 1998 。
```

__提示：__

- 链表中节点的数目在范围 `[1, 10 ^ 4]` 内
- `0 <= Node.val <= 9`
- 生成的输入满足:链表表示一个不含前导零的数字，除了数字 `0` 本身。

__思路:__

```text
模拟
如果当前值大于 4 就要往前添加节点
所以先判断 head 是否大于 4
如果大于 4 新建一个头节点 head = ListNode(0, head)
然后逐个节点翻倍并进位
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
    ListNode* doubleIt(ListNode* head) 
    {
        if (head -> val > 4) head = new ListNode(0, head);
        for (auto cur = head; cur; cur -> val += cur -> next && cur -> next -> val > 4, cur = cur -> next) cur -> val = (cur -> val << 1) % 10;
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
    public ListNode doubleIt(ListNode head) {
        if (head.val > 4) head = new ListNode(0, head);
        for (ListNode cur = head; cur != null; cur.val += (cur.next != null && cur.next.val > 4) ? 1 : 0, cur = cur.next) cur.val = (cur.val << 1) % 10;
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
    def doubleIt(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head.val > 4:
            head = ListNode(0, head)
        cur = head
        while cur:
            cur.val = (cur.val << 1) % 10
            if cur.next and cur.next.val > 4:
                cur.val += 1
            cur = cur.next
        return head
```
