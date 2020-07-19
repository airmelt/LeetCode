__Description__:
Sort a linked list using insertion sort.


A graphical example of insertion sort. The partial sorted list (black) initially contains only the first element in the list.
With each iteration one element (red) is removed from the input data and inserted in-place into the sorted list
![Insertion-sort-example](https://upload-images.jianshu.io/upload_images/16639143-367a2af5bcacb2d0.gif?imageMogr2/auto-orient/strip)

Algorithm of Insertion Sort:

Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
It repeats until no input elements remain.

__Example:__
Example 1:

Input: 4->2->1->3
Output: 1->2->3->4

Example 2:

Input: -1->5->3->4->0
Output: -1->0->3->4->5

__题目描述__:
对链表进行插入排序。
![插入排序](https://upload-images.jianshu.io/upload_images/16639143-a88700905814607e.gif?imageMogr2/auto-orient/strip)
插入排序的动画演示如上。从第一个元素开始，该链表可以被认为已经部分排序（用黑色表示）。
每次迭代时，从输入数据中移除一个元素（用红色表示），并原地将其插入到已排好序的链表中。

 

插入排序算法：

插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
重复直到所有输入数据插入完为止。
 
__示例 :__
示例 1：

输入: 4->2->1->3
输出: 1->2->3->4

示例 2：

输入: -1->5->3->4->0
输出: -1->0->3->4->5

__思路__:
按照插入排序头插法插入链表即可
时间复杂度O(n ^ 2), 空间复杂度O(1)

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
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        ListNode *result = new ListNode(0), *temp;
        for (auto p = head; p; p = temp)
        {
            auto q = result;
            while (q -> next and p -> val > q -> next -> val) q = q -> next;
            temp = p -> next;
            p -> next = q -> next;
            q -> next = p;
        }
        return result -> next;
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
    public ListNode insertionSortList(ListNode head) {
        ListNode result = new ListNode(0), temp;
        for (ListNode p = head; p != null; p = temp) {
            ListNode q = result;
            while (q.next != null && p.val > q.next.val) q = q.next;
            temp = p.next;
            p.next = q.next;
            q.next = p;
        }
        return result.next;
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
    def insertionSortList(self, head: ListNode) -> ListNode:
        result, pre = ListNode(0), None
        result.next = head
        while head and head.next:
            if not head.val > head.next.val:
                head = head.next
                continue
            pre = result
            while pre.next.val < head.next.val:
                pre = pre.next
            cur = head.next
            head.next, cur.next, pre.next = cur.next, pre.next, cur
        return result.next
```