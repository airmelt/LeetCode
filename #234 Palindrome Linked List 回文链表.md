__Description__:
Given a singly linked list, determine if it is a palindrome.

**Example:**
Example 1:
Input: 1->2
Output: false

Example 2:
Input: 1->2->2->1
Output: true

__Follow up:__
Could you do it in O(n) time and O(1) space?

__题目描述__:
请判断一个链表是否为回文链表。

**示例：**
示例 1:
输入: 1->2
输出: false

示例 2:
输入: 1->2->2->1
输出: true

__进阶：__
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？

__思路__:
1. 用快慢指针找到链表的中点, 将中点以后的链表反转, 从开头和反转的链表比较即可
2. 将链表里的数值取出转化为数值
时间复杂度O(n), 空间复杂度O(1)

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
    bool isPalindrome(ListNode* head) {
        if (!head || !head -> next) return true;
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast && fast -> next) {
            fast = fast -> next -> next;
            slow = slow -> next;
        }
        ListNode* mid = NULL;
        while (slow) {
            ListNode* p = slow -> next;
            slow -> next = mid;
            mid = slow;
            slow = p;
        }
        while (head && mid) {
            if (head -> val != mid -> val) return false;
            head = head -> next;
            mid = mid -> next;
        }
        return true;
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
    public boolean isPalindrome(ListNode head) {
        int a = 0;
        int b = 0;
        int c = 0;
        while (head != null) {
            a *= 10;
            a += head.val;
            c *= 10;
            if (c == 0) c = 1;
            b += c * head.val;
            head = head.next;
        }
        return a == b;
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
    def isPalindrome(self, head: ListNode) -> bool:
        a = b = c = 0
        while head:
            a *= 10
            a += head.val
            c *= 10
            if not c:
                c = 1
            b += c * head.val
            head = head.next
        return a == b
```
