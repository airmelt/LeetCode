__Description__:
Given head which is a reference node to a singly-linked list. The value of each node in the linked list is either 0 or 1. The linked list holds the binary representation of a number.

Return the decimal value of the number in the linked list.

__Example:__
Example 1:
![Linked List](https://upload-images.jianshu.io/upload_images/16639143-3da180847a9ce307.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: head = [1,0,1]
Output: 5
Explanation: (101) in base 2 = (5) in base 10

Example 2:

Input: head = [0]
Output: 0

Example 3:

Input: head = [1]
Output: 1

Example 4:

Input: head = [1,0,0,1,0,0,1,1,1,0,0,0,0,0,0]
Output: 18880

Example 5:

Input: head = [0,0]
Output: 0
 
__Constraints:__

The Linked List is not empty.
Number of nodes will not exceed 30.
Each node's value is either 0 or 1.

__题目描述__:
给你一个单链表的引用结点 head。链表中每个结点的值不是 0 就是 1。已知此链表是一个整数数字的二进制表示形式。

请你返回该链表所表示数字的 十进制值 。

__示例 :__
示例 1：
![单链表](https://upload-images.jianshu.io/upload_images/16639143-a8c3b0be8f77ea9e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：head = [1,0,1]
输出：5
解释：二进制数 (101) 转化为十进制数 (5)

示例 2：

输入：head = [0]
输出：0

示例 3：

输入：head = [1]
输出：1

示例 4：

输入：head = [1,0,0,1,0,0,1,1,1,0,0,0,0,0,0]
输出：18880

示例 5：

输入：head = [0,0]
输出：0
 
__提示：__

链表不为空。
链表的结点总数不超过 30。
每个结点的值不是 0 就是 1。

__思路__:
相当于按位将二进制转化为十进制
比如 1010 添上 1 -> 1010 * 2 + 1 -> 10100 + 1 -> 10100 | 1 -> 10101
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
    int getDecimalValue(ListNode* head) 
    {
        int result = 0;
        while (head)
        {
            result <<= 1;
            result |= head -> val;
            head = head -> next;
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
    private int level = 0;
    
    public int getDecimalValue(ListNode head) {
        if (head == null) return 0;
        return getDecimalValue(head.next) + (int)(Math.pow(2, level++) * head.val);
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
    def getDecimalValue(self, head: ListNode) -> int:
        result = 0
        while head:
            result <<= 1
            result |= head.val
            head = head.next
        return result
```