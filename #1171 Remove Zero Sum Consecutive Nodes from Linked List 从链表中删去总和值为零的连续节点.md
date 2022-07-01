# 1171 Remove Zero Sum Consecutive Nodes from Linked List 从链表中删去总和值为零的连续节点

__Description__:
Given the head of a linked list, we repeatedly delete consecutive sequences of nodes that sum to 0 until there are no such sequences.

After doing so, return the head of the final linked list.  You may return any such answer.

(Note that in the examples below, all sequences are serializations of ListNode objects.)

__Example:__

Example 1:

Input: head = [1,2,-3,3,1]
Output: [3,1]
Note: The answer [1,2,1] would also be accepted.

Example 2:

Input: head = [1,2,3,-3,4]
Output: [1,2,4]

Example 3:

Input: head = [1,2,3,-3,-2]
Output: [1]

__Constraints:__

The given linked list will contain between 1 and 1000 nodes.
Each node in the linked list has -1000 <= node.val <= 1000.

__题目描述__:
给你一个链表的头节点 head，请你编写代码，反复删去链表中由 总和 值为 0 的连续节点组成的序列，直到不存在这样的序列为止。

删除完毕后，请你返回最终结果链表的头节点。

你可以返回任何满足题目要求的答案。

（注意，下面示例中的所有序列，都是对 ListNode 对象序列化的表示。）

__示例 :__

示例 1：

输入：head = [1,2,-3,3,1]
输出：[3,1]
提示：答案 [1,2,1] 也是正确的。

示例 2：

输入：head = [1,2,3,-3,4]
输出：[1,2,4]

示例 3：

输入：head = [1,2,3,-3,-2]
输出：[1]

__提示:__

给你的链表中可能有 1 到 1000 个节点。
对于链表中的每个节点，节点的值：-1000 <= node.val <= 1000.

__思路__:

1. 前缀和 ➕ 哈希表
用哈希表记录前缀和及对应的结点
如果前缀和已经出现过说明两个相同前缀和中间的结点的和为 0
直接将中间结点全部舍去, 将下一个结点赋值给当前结点
为了保证能返回空结点, 可以使用哑结点初始化
时间复杂度为 O(n), 空间复杂度为 O(n)
2. 递归
递归终点 head 为空返回 null 即可
从 head 开始如果有和为 0 跳过这些结点
否则 head 的下一个递归调用函数
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n), 递归需要额外栈空间

__代码__:
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
    ListNode* removeZeroSumSublists(ListNode* head) 
    {
        if (!head) return head;
        int s = 0;
        for (ListNode* cur = head; cur; cur = cur -> next) if (!(s += cur -> val)) return removeZeroSumSublists(cur -> next);
        head -> next = removeZeroSumSublists(head -> next);
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
    public ListNode removeZeroSumSublists(ListNode head) {
        if (head == null) return head;
        int s = 0;
        for (ListNode cur = head; cur != null; cur = cur.next) if ((s += cur.val) == 0) return removeZeroSumSublists(cur.next);
        head.next = removeZeroSumSublists(head.next);
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
    def removeZeroSumSublists(self, head: ListNode) -> ListNode:
        d, p, s = dict(), ListNode(0, head), 0
        q = p
        while q:
            d[s := s + q.val], q = q, q.next
        s, q = 0, p
        while q:
            q.next, q = d[s := s + q.val].next, q.next
        return p.next
            
```
