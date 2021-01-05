# 160 Intersection of Two Linked Lists 相交链表

__Description__:
Write a program to find the node at which the intersection of two singly linked lists begins.

For example, the following two linked lists:

![begin to intersect at node c1.](http://upload-images.jianshu.io/upload_images/16639143-348b5e896614d8a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Example:**

**Example 1:**
![Example 1](http://upload-images.jianshu.io/upload_images/16639143-8d696a5194f34d29.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**Input:** intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
**Output:** Reference of the node with value = 8
**Input Explanation:** The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,0,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.

**Example 2:**
![Example 2](http://upload-images.jianshu.io/upload_images/16639143-1ed1c8167936468d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**Input:** intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
**Output:** Reference of the node with value = 2
**Input Explanation:** The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [0,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.

**Example 3:**
![Example 3](http://upload-images.jianshu.io/upload_images/16639143-1a78d20283d58283.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**Input:** intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
**Output:** null
**Input Explanation:** From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
**Explanation:** The two lists do not intersect, so return null.

**Notes:**

* If the two linked lists have no intersection at all, return `null`.
* The linked lists must retain their original structure after the function returns.
* You may assume there are no cycles anywhere in the entire linked structure.
* Your code should preferably run in O(n) time and use only O(1) memory.

__题目描述__:
编写一个程序，找到两个单链表相交的起始节点。

如下面的两个链表
![在节点 c1 开始相交。](http://upload-images.jianshu.io/upload_images/16639143-83f90f164b21120a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**示例：**

**示例 1：**

![示例 1](http://upload-images.jianshu.io/upload_images/16639143-be7d536652d208bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**输入：**
intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
**输出：**
Reference of the node with value = 8
**输入解释：**
相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。

**示例 2：**

![示例 2](http://upload-images.jianshu.io/upload_images/16639143-faaeb1ed1f9def72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**输入：**
intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
**输出：**
Reference of the node with value = 2
**输入解释：**
相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。

**示例 3：**

![示例 3](http://upload-images.jianshu.io/upload_images/16639143-1747bf35ae64a1f3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**输入：**
intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
**输出：**
null
**输入解释：**
从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
**解释：**
这两个链表不相交，因此返回 null。

**注意：**

* 如果两个链表没有交点，返回 `null`.
* 在返回结果后，两个链表仍须保持原有的结构。
* 可假定整个链表结构中没有循环。
* 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。

__思路__:

1. 计算两链表长度, 选择较长的链表前移两者长度之差, 然后同步比较
2. 可以将链表首尾相连, 这样可以转化为一条链表是否有环
比如 2 -> 3 -> 1 和 3 -> 1, 分别连接起来为
2 -> 3 -> 1 -> 3 -> 1
3 -> 1 -> 2 -> 3 -> 1
返回两者相等的位置即可
时间复杂度O(n), 空间复杂度O(1)

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
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode *p = headA, *q = headB;
        while (p != q) 
        {
            p = !p ? headB : p -> next;
            q = !q ? headA : q -> next;
        }
        return p;
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
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        ListNode p = headA;
        ListNode q = headB;
        int len = 0;
        boolean flag = true;
        while (headA != null && headB != null) {
            headA = headA.next;
            headB = headB.next;
        }
        while (headA != null) {
            headA = headA.next;
            len++;
        }
        while (headB != null) {
            headB = headB.next;
            flag = false;
            len++;
        }
        if (flag) {
            while (len != 0) {
                p = p.next;
                len--;
            }
        } else {
            while (len != 0) {
                q = q.next;
                len--;
            }
        }
        while (p != q) {
            p = p.next;
            q = q.next;
        }
        return p;
    }
}
```

__Python__:

```Python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution(object):
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        p, q = headA, headB
        while p != q:
            # Python中的 and从左到右计算表达式，若所有值均为真，则返回最后一个值，若存在假，返回第一个假值。
            # or 也是从左到右计算表达式，则返回第一个为真的值，若均为假，则返回最后一个值。
            # 也可以使用 condition if else
            p, q = not p and headB or p.next, not q and headA or q.next
        return p
```
