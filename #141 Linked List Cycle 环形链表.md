# 141 Linked List Cycle 环形链表

__Description__:
Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.

**Example:**

**Example 1:**
**Input:** head = [3,2,0,-4], pos = 1
**Output:** true
**Explanation:** There is a cycle in the linked list, where tail connects to the second node.

![Example 1](http://upload-images.jianshu.io/upload_images/16639143-7fef16085a1e28e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Example 2:**
**Input:** head = [1,2], pos = 0
**Output:** true
**Explanation:** There is a cycle in the linked list, where tail connects to the first node.

![Example 2](http://upload-images.jianshu.io/upload_images/16639143-2fef50260ea974b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Example 3:**
**Input:** head = [1], pos = -1
**Output:** false
**Explanation:** There is no cycle in the linked list.

![Example 3](http://upload-images.jianshu.io/upload_images/16639143-0ee6fb5bf902ef2f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**Follow up:**

Can you solve it using *O(1)* (i.e. constant) memory?

__题目描述__:
给定一个链表，判断链表中是否有环。

为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。

**示例：**

**示例 1：**
**输入：**
head = [3,2,0,-4], pos = 1
**输出：**
true
**解释：**
链表中有一个环，其尾部连接到第二个节点。

![示例 1](http://upload-images.jianshu.io/upload_images/16639143-9ea69fbab01f002b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**示例 2：**
**输入：**
head = [1,2], pos = 0
**输出：**
true
**解释：**
链表中有一个环，其尾部连接到第一个节点。

![示例 2](http://upload-images.jianshu.io/upload_images/16639143-bbd993563a6617b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**示例 3：**
**输入：**
head = [1], pos = -1
**输出：**
false
**解释：**
链表中没有环。

![示例 3](http://upload-images.jianshu.io/upload_images/16639143-6644d6442aba72ed.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**进阶：**
你能用 *O(1)*（即，常量）内存解决此问题吗？

__思路__:

快慢指针
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
    bool hasCycle(ListNode *head) 
    {
        ListNode *fast = head, *slow = head;
        while (fast and fast -> next) 
        {            
            fast = fast -> next -> next;
            slow = slow -> next;
            if (fast == slow) return true;
        }
        return false;
    }
};
```

__Java__:

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        ListNode fast = head, slow = head;
        while (fast != null && fast.next != null) {            
            fast = fast.next.next;
            slow = slow.next;
            if (fast == slow) return true;
        }
        return false;
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
    def hasCycle(self, head: ListNode) -> ListNode:
        slow = fast = head
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
            if slow == fast:
                return True
        return False
```
