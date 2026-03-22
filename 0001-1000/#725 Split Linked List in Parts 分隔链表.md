# 725 Split Linked List in Parts 分隔链表

__Description__:
Given the head of a singly linked list and an integer k, split the linked list into k consecutive linked list parts.

The length of each part should be as equal as possible: no two parts should have a size differing by more than one. This may lead to some parts being null.

The parts should be in the order of occurrence in the input list, and parts occurring earlier should always have a size greater than or equal to parts occurring later.

Return an array of the k parts.

__Example:__

Example 1:

![Split Linked List1](https://assets.leetcode.com/uploads/2021/06/13/split1-lc.jpg)

Input: head = [1,2,3], k = 5
Output: [[1],[2],[3],[],[]]
Explanation:
The first element output[0] has output[0].val = 1, output[0].next = null.
The last element output[4] is null, but its string representation as a ListNode is [].

Example 2:

![Split Linked List2](https://assets.leetcode.com/uploads/2021/06/13/split2-lc.jpg)

Input: head = [1,2,3,4,5,6,7,8,9,10], k = 3
Output: [[1,2,3,4],[5,6,7],[8,9,10]]
Explanation:
The input has been split into consecutive parts with size difference at most 1, and earlier parts are a larger size than the later parts.

__Constraints:__

The number of nodes in the list is in the range [0, 1000].
0 <= Node.val <= 1000
1 <= k <= 50

__题目描述__:
给定一个头结点为 root 的链表, 编写一个函数以将链表分隔为 k 个连续的部分。

每部分的长度应该尽可能的相等: 任意两部分的长度差距不能超过 1，也就是说可能有些部分为 null。

这k个部分应该按照在链表中出现的顺序进行输出，并且排在前面的部分的长度应该大于或等于后面的长度。

返回一个符合上述规则的链表的列表。

举例： 1->2->3->4, k = 5 // 5 结果 [ [1], [2], [3], [4], null ]

__示例 :__

示例 1：

输入:
root = [1, 2, 3], k = 5
输出: [[1],[2],[3],[],[]]
解释:
输入输出各部分都应该是链表，而不是数组。
例如, 输入的结点 root 的 val= 1, root.next.val = 2, \root.next.next.val = 3, 且 root.next.next.next = null。
第一个输出 output[0] 是 output[0].val = 1, output[0].next = null。
最后一个元素 output[4] 为 null, 它代表了最后一个部分为空链表。

示例 2：

输入:
root = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 3
输出: [[1, 2, 3, 4], [5, 6, 7], [8, 9, 10]]
解释:
输入被分成了几个连续的部分，并且每部分的长度相差不超过1.前面部分的长度大于等于后面部分的长度。

__提示:__

root 的长度范围： [0, 1000].
输入的每个节点的大小范围：[0, 999].
k 的取值范围： [1, 50].

__思路__:

快慢指针
设置 k 个指针分别指向 root 的第 k 个节点
每次循环的时候第 k 个指针向前移动 k 步

```text
e.g. root = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10], k = 3
result[0] = [1] -> [2] -> [3] -> [4]
result[1] = [2] -> [4] -> [6] -> [7]
result[2] = [3] -> [6] -> [9] -> [10] -> None
```

最后一次只移动指针一步, 因为最后一个指针已经走到尽头
此时 result[i] 刚好就是 result[i + 1] 的起点
反向遍历结果数组令 result[i] = result[i - 1] -> next, result[i - 1],next = nullptr, 分割开链表
题目中已经给定 root 非空, 否则在程序开头需要对特例进行判断
时间复杂度为 O(n), 空间复杂度为 O(1), 不需要额外空间

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
    vector<ListNode*> splitListToParts(ListNode* head, int k) 
    {
        vector<ListNode*> result(k);
        result.front() = head;
        for (int i = 1; i < k; i++) if (result[i - 1]) result[i] = result[i - 1] -> next;
        while (result[k - 1]) 
        {
            for (int i = 0; i < k - 1; i++)
            {
                result[k - 1] = result[k - 1] -> next;
                if (!result[k - 1]) break;
                for (int j = i; j < k - 1; j++) result[j] = result[j] -> next;
            }
            if (result[k - 1]) result[k - 1] = result[k - 1] -> next;
        }
        for (int i = k; i > 0; i--) 
        {
            if (result[i - 1]) 
            {
                result[i] = result[i - 1] -> next;
                result[i - 1] -> next = nullptr;
            }
        }
        result.front() = head;
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
    public ListNode[] splitListToParts(ListNode head, int k) {
        ListNode[] result = new ListNode[k];
        result[0] = head;
        for (int i = 1; i < k; i++) if (result[i - 1] != null) result[i] = result[i - 1].next;
        while (result[k - 1] != null) {
            for (int i = 0; i < k - 1; i++) {
                result[k - 1] = result[k - 1].next;
                if (result[k - 1] == null) break;
                for (int j = i; j < k - 1; j++) result[j] = result[j].next;
            }
            if (result[k - 1] != null) result[k - 1] = result[k - 1].next;
        }
        for (int i = k; i > 0; i--) {
            if (result[i - 1] != null) {
                result[i] = result[i - 1].next;
                result[i - 1].next = null;
            }
        }
        result[0] = head;
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
    def splitListToParts(self, head: ListNode, k: int) -> List[ListNode]:
        result = [head] + [None] * (k - 1)
        for i in range(1, k):
            if result[i - 1]:
                result[i] = result[i - 1].next
        while result[k - 1]:
            for i in range(k - 1):
                result[k - 1] = result[k - 1].next
                if not result[k - 1]:
                    break
                for j in range(i, k - 1):
                    result[j] = result[j].next
            result[k - 1] = result[k - 1].next if result[k - 1] else None
        for i in range(k - 1, 0, -1):
            if result[i - 1]:
                result[i], result[i - 1].next = result[i - 1].next, None
        result[0] = head
        return result
```
