__Description__:
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

__Example__:
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4

__题目描述__:
将两个有序链表合并为一个新的有序链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。

__示例__:
输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

__[链表 Linked list-wiki]([https://en.wikipedia.org/wiki/Linked_list](https://en.wikipedia.org/wiki/Linked_list)
)__:
> 不同于顺序表, 链表中存储的为值(val)以及指向下一个结点的__指针__(*next)
> 插入的时间复杂度为O(1), 随机访问的时间复杂度为O(n)
> 结构类似于: val(1), next -> val(2), next -> ...
> C++定义链表:
> ```
> struct ListNode {
>     // 值
>     int val;
>     // 指向下一个结点的指针
>     ListNode *next;
>     // 初始化链表
>     ListNode(int x) : val(x), next(NULL) {}
> };
> ```

__思路__:
1. 递归. l1的值小于l2的值, l1指向next结点, l2不变.
2. 迭代. 结束条件为l1和l2指向空.
时间复杂度O(n + m), 其中n和m分别为两个链表的长度, 空间复杂度O(1)

__代码__:
__C++__:
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if (!l1) return l2;
        if (!l2) return l1;
        if (l1 -> val < l2 -> val) {
            l1 -> next = mergeTwoLists(l1 -> next, l2);
            return l1;
        } else {
            l2 -> next = mergeTwoLists(l1, l2 -> next);
            return l2;
        }
    }
};
```

__Java__:
```
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        if (l1 == null) return l2;
        if (l2 == null) return l1;
        if (l1.val < l2.val) {
            l1.next = mergeTwoLists(l1.next, l2);
            return l1;
        } else {
            l2.next = mergeTwoLists(l1, l2.next);
            return l2;
        }
    }
}
```

__Python__:
```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        if not l1:
            return l2
        if not l2:
            return l1
        # result/p初始化头结点
        result = p = ListNode(0)
        while l1 and l2:
            if l1.val < l2.val:
                p.next = l1
                l1 = l1.next
            else:
                p.next = l2
                l2 = l2.next
            p = p.next
        if l1:
            p.next = l1
        else:
            p.next = l2
        return result.next
```
