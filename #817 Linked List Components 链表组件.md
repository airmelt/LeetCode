# 817 Linked List Components 链表组件

__Description__:
You are given the head of a linked list containing unique integer values and an integer array nums that is a subset of the linked list values.

Return the number of connected components in nums where two values are connected if they appear consecutively in the linked list.

__Example:__

Example 1:

![linkedlistcom 1](https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom1.jpg)

Input: head = [0,1,2,3], nums = [0,1,3]
Output: 2
Explanation: 0 and 1 are connected, so [0, 1] and [3] are the two connected components.

Example 2:

![linkedlistcom 2](https://assets.leetcode.com/uploads/2021/07/22/lc-linkedlistcom2.jpg)

Input: head = [0,1,2,3,4], nums = [0,3,1,4]
Output: 2
Explanation: 0 and 1 are connected, 3 and 4 are connected, so [0, 1] and [3, 4] are the two connected components.

__Constraints:__

The number of nodes in the linked list is n.
1 <= n <= 10^4
0 <= Node.val < n
All the values Node.val are unique.
1 <= nums.length <= n
0 <= nums[i] <= n
All the values of nums are unique.

__题目描述__:
给定链表头结点 head，该链表上的每个结点都有一个 唯一的整型值 。

同时给定列表 G，该列表是上述链表中整型值的一个子集。

返回列表 G 中组件的个数，这里对组件的定义为：链表中一段最长连续结点的值（该值必须在列表 G 中）构成的集合。

__示例 :__

示例 1：

输入:
head: 0->1->2->3
G = [0, 1, 3]
输出: 2
解释:
链表中,0 和 1 是相连接的，且 G 中不包含 2，所以 [0, 1] 是 G 的一个组件，同理 [3] 也是一个组件，故返回 2。

示例 2：

输入:
head: 0->1->2->3->4
G = [0, 3, 1, 4]
输出: 2
解释:
链表中，0 和 1 是相连接的，3 和 4 是相连接的，所以 [0, 1] 和 [3, 4] 是两个组件，故返回 2。

__提示:__

如果 N 是给定链表 head 的长度，1 <= N <= 10000。
链表中每个结点的值所在范围为 [0, N - 1]。
1 <= G.length <= 10000
G 是链表中所有结点的值的一个子集.

__思路__:

模拟
将数组放到集合中, 加快查找
链表每个元素是独特的, 所以每次找到不在集合中或者链表结尾就给结果 ➕ 1
时间复杂度为 O(n + m), 空间复杂度为 O(m), n 为链表的长度, m 为数组的长度

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
class Solution {
public:
    int numComponents(ListNode* head, vector<int>& nums) 
    {
        unordered_set<int> s(nums.begin(), nums.end());
        int result = 0;
        while (head) 
        {
            if (s.find(head -> val) != s.end()) if (!head -> next or s.find(head -> next -> val) == s.end()) ++result;
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
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public int numComponents(ListNode head, int[] nums) {
        Set<Integer> set = new HashSet<>();
        for (int num : nums) set.add(num);
        int result = 0;
        while (head != null) {
            if (set.contains(head.val) && (head.next == null || !set.contains(head.next.val))) ++result;
            head = head.next;
        }
        return result;
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
    def numComponents(self, head: ListNode, nums: List[int]) -> int:
        nums, result = set(nums), 0
        while head:
            if head.val in nums and (not head.next or head.next.val not in nums):
                result += 1
            head = head.next
        return result
```
