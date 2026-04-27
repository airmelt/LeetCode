# 2807 Insert Greatest Common Divisors in Linked List 在链表中插入最大公约数

__Description:__

Given the head of a linked list `head`, in which each node contains an integer value.

Between every pair of adjacent nodes, insert a new node with a value equal to the greatest common divisor of them.

Return the linked list after insertion.

The greatest common divisor of two numbers is the largest positive integer that evenly divides both numbers.

__Example:__

Example 1:

![2807-1](https://assets.leetcode.com/uploads/2023/07/18/ex1_copy.png)

```text
Input: head = [18,6,10,3]
Output: [18,6,6,2,10,1,3]
Explanation: The 1st diagram denotes the initial linked list and the 2nd diagram denotes the linked list after inserting the new nodes (nodes in blue are the inserted nodes).
```

- We insert the greatest common divisor of 18 and 6 = 6 between the 1st and the 2nd nodes.
- We insert the greatest common divisor of 6 and 10 = 2 between the 2nd and the 3rd nodes.
- We insert the greatest common divisor of 10 and 3 = 1 between the 3rd and the 4th nodes.

There are no more adjacent nodes, so we return the linked list.

Example 2:

![2807-2](https://assets.leetcode.com/uploads/2023/07/18/ex2_copy1.png)

```text
Input: head = [7]
Output: [7]
Explanation: The 1st diagram denotes the initial linked list and the 2nd diagram denotes the linked list after inserting the new nodes.
There are no pairs of adjacent nodes, so we return the initial linked list.
```

__Constraints:__

- The number of nodes in the list is in the range `[1, 5000]`.
- `1 <= Node.val <= 1000`

__题目描述:__

给你一个链表的头 `head` ，每个结点包含一个整数值。

在相邻结点之间，请你插入一个新的结点，结点值为这两个相邻结点值的 最大公约数 。

请你返回插入之后的链表。

两个数的 最大公约数 是可以被两个数字整除的最大正整数。

__示例:__

示例 1：

![2807-3](https://assets.leetcode.com/uploads/2023/07/18/ex1_copy.png)

```text
输入：head = [18,6,10,3]
输出：[18,6,6,2,10,1,3]
解释：第一幅图是一开始的链表，第二幅图是插入新结点后的图（蓝色结点为新插入结点）。
```

- 18 和 6 的最大公约数为 6 ，插入第一和第二个结点之间。
- 6 和 10 的最大公约数为 2 ，插入第二和第三个结点之间。
- 10 和 3 的最大公约数为 1 ，插入第三和第四个结点之间。

所有相邻结点之间都插入完毕，返回链表。

示例 2：

![2807-4](https://assets.leetcode.com/uploads/2023/07/18/ex2_copy1.png)

```text
输入：head = [7]
输出：[7]
解释：第一幅图是一开始的链表，第二幅图是插入新结点后的图（蓝色结点为新插入结点）。
没有相邻结点，所以返回初始链表。
```

__提示：__

- 链表中结点数目在 `[1, 5000]` 之间。
- `1 <= Node.val <= 1000`

__思路:__

```text
模拟
如果当前节点有下一个节点
那么将两个节点的 gcd 值创建一个新的节点, 新节点的下一个节点就设置为当前工作节点的下一个节点
然后把工作节点移动到原本的下一个节点
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
    ListNode* insertGreatestCommonDivisors(ListNode* head) 
    {
        for (auto cur = head; cur -> next; cur = cur -> next -> next) cur -> next = new ListNode(gcd(cur -> val, cur -> next -> val), cur -> next);
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
    public ListNode insertGreatestCommonDivisors(ListNode head) {
        for (ListNode cur = head; cur.next != null; cur = cur.next.next) cur.next = new ListNode(gcd(cur.val, cur.next.val), cur.next);
        return head;
    }

    private int gcd(int a, int b) {
        return b == 0 ? a : gcd(b, a % b);
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
    def insertGreatestCommonDivisors(self, head: Optional[ListNode]) -> Optional[ListNode]:
        cur = head
        while cur.next:
            cur.next = ListNode(gcd(cur.val, cur.next.val), cur.next)
            cur = cur.next.next
        return head
```
