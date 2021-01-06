# 23 Merge k Sorted Lists 合并K个排序链表

__Description__:
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

__Example:__

Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6

__题目描述__:
合并 k 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

__示例 :__

输入:
[
  1->4->5,
  1->3->4,
  2->6
]
输出: 1->1->2->3->4->4->5->6

__思路__:

1. 暴力法, 可以将所有链表元素放到一个数组中, 对链表中的值进行排序即可
时间复杂度O(nlgn), 空间复杂度O(n)
2. 由于结点已经有序, 只要每次比较链表开头的元素即可, 是暴力法的优化
时间复杂度O(kn), 空间复杂度O(1)
3. 对方法 2中的比较, 采用优先队列实现
时间复杂度O(nlgk), 空间复杂度O(k)
4. 方法 2中另一种实现方式, 两两合并
时间复杂度O(kn), 空间复杂度O(1)
5. 采用归并的思想, 对 方法4进行优化
时间复杂度O(nlgk), 空间复杂度O(1)

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
    ListNode* mergeKLists(vector<ListNode*>& lists) 
    {
        int step = 1, n = lists.size();
        while (step < n)
        {
            for (int i = 0; i < n - step; i += (step << 1)) lists[i] = merge_two_lists(lists[i], lists[i + step]);
            step <<= 1;
        }
        return n ? lists[0] : nullptr;
    }
private:
    ListNode* merge_two_lists(ListNode* p, ListNode* q)
    {
        if (!p) return q;
        if (!q) return p;
        ListNode *result;
        if (p -> val > q -> val)
        {
            result = q;
            result -> next = merge_two_lists(p, q -> next);
        }
        else
        {
            result = p;
            result -> next = merge_two_lists(p -> next, q);
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
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        int step = 1, n = lists.length;
        while (step < n) {
            for (int i = 0; i < n - step; i += (step << 1)) lists[i] = merge_two_lists(lists[i], lists[i + step]);
            step <<= 1;
        }
        return n > 0 ? lists[0] : null;
    }
    private ListNode merge_two_lists(ListNode p, ListNode q) {
        if (p == null) return q;
        if (q == null) return p;
        ListNode result = new ListNode(0);
        if (p.val > q.val) {
            result = q;
            result.next = merge_two_lists(p, q.next);
        } else {
            result = p;
            result.next = merge_two_lists(p.next, q);
        }
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
import heapq
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        p = result = ListNode(0)
        head = []
        for i in range(len(lists)):
            if lists[i] :
                heapq.heappush(head, (lists[i].val, i))
                lists[i] = lists[i].next
        while head:
            val, i = heapq.heappop(head)
            p.next = ListNode(val)
            p = p.next
            if lists[i]:
                heapq.heappush(head, (lists[i].val, i))
                lists[i] = lists[i].next
        return result.next
```
