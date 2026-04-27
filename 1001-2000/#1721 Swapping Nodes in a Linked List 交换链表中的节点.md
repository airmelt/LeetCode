# 1721 Swapping Nodes in a Linked List 交换链表中的节点

__Description:__

You are given the `head` of a linked list, and an integer `k`.

Return _the head of the linked list after __swapping__ the values of the_ `k ^ th` _node from the beginning and the_ `k ^ th` _node from the end (the list is __1-indexed__)._

__Example:__

Example 1:

![1721-1](https://assets.leetcode.com/uploads/2020/09/21/linked1.jpg)

```text
Input: head = [1,2,3,4,5], k = 2
Output: [1,4,3,2,5]
```

Example 2:

```text
Input: head = [7,9,6,6,7,8,3,0,9,5], k = 5
Output: [7,9,6,6,8,7,3,0,9,5]
```

__Constraints:__

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 10 ^ 5`
- `0 <= Node.val <= 100`

__题目描述:__

给你链表的头节点 `head` 和一个整数 `k` 。

__交换__ 链表正数第 `k` 个节点和倒数第 `k` 个节点的值后，返回链表的头节点（链表 __从 1 开始索引__）。

__示例:__

示例 1：

![1721-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/10/linked1.jpg)

```text
输入：head = [1,2,3,4,5], k = 2
输出：[1,4,3,2,5]
```

示例 2：

```text
输入：head = [7,9,6,6,7,8,3,0,9,5], k = 5
输出：[7,9,6,6,8,7,3,0,9,5]
```

示例 3：

```text
输入：head = [1], k = 1
输出：[1]
```

示例 4：

```text
输入：head = [1,2], k = 1
输出：[2,1]
```

示例 5：

```text
输入：head = [1,2,3], k = 2
输出：[1,2,3]
```

__提示：__

- 链表中节点的数目是 `n`
- `1 <= k <= n <= 10 ^ 5`
- `0 <= Node.val <= 100`

__思路:__

```text
模拟
先找到第 k 个结点
然后使用快慢指针找到倒数第 k 个结点
交换两个结点的值
注意不要交换两个结点
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
    ListNode* swapNodes(ListNode* head, int k) 
    {
        ListNode* fast = head;
        for (int i = 1; i < k; i++) fast = fast -> next;
        ListNode* p = fast, *slow = head;
        while (fast -> next)
        {
            fast = fast -> next;
            slow = slow -> next;
        }
        swap(slow -> val, p -> val);
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
    public ListNode swapNodes(ListNode head, int k) {
        ListNode fast = head;
        for (int i = 1; i < k; i++) fast = fast.next;
        ListNode p = fast, slow = head;
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }
        p.val = p.val + slow.val - (slow.val = p.val);
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
    def swapNodes(self, head: Optional[ListNode], k: int) -> Optional[ListNode]:
        fast = head
        while (k := k - 1):
            fast = fast.next
        p, slow = fast, head;
        while fast.next:
            fast, slow = fast.next, slow.next
        p.val, slow.val = slow.val, p.val
        return head
```
