# 61 Rotate List 旋转链表

__Description__:
Given a linked list, rotate the list to the right by k places, where k is non-negative.

__Example:__

Example 1:

Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL

Example 2:

Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL

__题目描述__:
给定一个链表，旋转链表，将链表每个节点向右移动 k 个位置，其中 k 是非负数。

__示例 :__

示例 1:

输入: 1->2->3->4->5->NULL, k = 2
输出: 4->5->1->2->3->NULL
解释:
向右旋转 1 步: 5->1->2->3->4->NULL
向右旋转 2 步: 4->5->1->2->3->NULL

示例 2:

输入: 0->1->2->NULL, k = 4
输出: 2->0->1->NULL
解释:
向右旋转 1 步: 2->0->1->NULL
向右旋转 2 步: 1->2->0->NULL
向右旋转 3 步: 0->1->2->NULL
向右旋转 4 步: 2->0->1->NULL

__思路__:

本质上相当于将链表成环找到尾结点和头结点即可

1. 先遍历一次链表找到链表长度, 此时可以找到尾结点
2. 循环次数 = 链表长度 - (k % 链表长度)
3. 将链表成环, 循环找到头结点和尾结点
4. 返回头结点
时间复杂度O(n), 空间复杂度O(1), 最多遍历 2次链表

__代码__:
__C++__:

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution 
{
public:
    ListNode* rotateRight(ListNode* head, int k) 
    {
        if (!head or k == 0) return head;
        int n = 1;
        ListNode *result = head, *tail = nullptr;
        while (result -> next)
        {
            result = result -> next;
            ++n;
        }
        n = n - (k % n);
        tail = result;
        result -> next = head;
        result = head;
        for (int i = 0; i < n; i++)
        {
            result = result -> next;
            tail = tail -> next;
        }
        tail -> next = nullptr;
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
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if (head == null || k == 0) return head;
        int n = 1;
        ListNode result = head, tail = null;
        while (result.next != null) {
            result = result.next;
            ++n;
        }
        n = n - (k % n);
        tail = result;
        result.next = head;
        result = head;
        for (int i = 0; i < n; i++) {
            result = result.next;
            tail = tail.next;
        }
        tail.next = null;
        return result;
    }
}
```

__Python__:

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def rotateRight(self, head: ListNode, k: int) -> ListNode:
        if not head or not k:
            return head
        n, result, tail = 1, head, None
        while result.next:
            result, n = result.next, n + 1
        n = n - (k % n)
        tail, result.next, result = result, head, head
        for i in range(n):
            result, tail = result.next, tail.next
        tail.next = None
        return result
```
