# 1669 Merge In Between Linked Lists 合并两个链表

__Description:__

You are given two linked lists: `list1` and `list2` of sizes `n` and `m` respectively.

Remove `list1`'s nodes from the `a ^ th` node to the `b ^ th` node, and put `list2` in their place.

The blue edges and nodes in the following figure indicate the result:

![1669-1](https://assets.leetcode.com/uploads/2020/11/05/fig1.png)

Build the result list and return its head.

__Example:__

Example 1:

![1669-2](https://assets.leetcode.com/uploads/2020/11/05/merge_linked_list_ex1.png)

```text
Input: list1 = [0,1,2,3,4,5], a = 3, b = 4, list2 = [1000000,1000001,1000002]
Output: [0,1,2,1000000,1000001,1000002,5]
Explanation: We remove the nodes 3 and 4 and put the entire list2 in their place. The blue edges and nodes in the above figure indicate the result.
```

Example 2:

![1669-3](https://assets.leetcode.com/uploads/2020/11/05/merge_linked_list_ex2.png)

```text
Input: list1 = [0,1,2,3,4,5,6], a = 2, b = 5, list2 = [1000000,1000001,1000002,1000003,1000004]
Output: [0,1,1000000,1000001,1000002,1000003,1000004,6]
Explanation: The blue edges and nodes in the above figure indicate the result.
```

__Constraints:__

- `3 <= list1.length <= 10 ^ 4`
- `1 <= a <= b < list1.length - 1`
- `1 <= list2.length <= 10 ^ 4`

__题目描述:__

给你两个链表 `list1` 和 `list2` ，它们包含的元素分别为 `n` 个和 `m` 个。

请你将 `list1` 中下标从 `a` 到 `b` 的全部节点都删除，并将 `list2` 接在被删除节点的位置。

下图中蓝色边和节点展示了操作后的结果：

![1669-4](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/28/fig1.png)

请你返回结果链表的头指针。

__示例:__

示例 1：

![1669-5](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/28/merge_linked_list_ex1.png)

```text
输入：list1 = [0,1,2,3,4,5], a = 3, b = 4, list2 = [1000000,1000001,1000002]
输出：[0,1,2,1000000,1000001,1000002,5]
解释：我们删除 list1 中下标为 3 和 4 的两个节点，并将 list2 接在该位置。上图中蓝色的边和节点为答案链表。
```

示例 2：

![1669-6](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/28/merge_linked_list_ex2.png)

```text
输入：list1 = [0,1,2,3,4,5,6], a = 2, b = 5, list2 = [1000000,1000001,1000002,1000003,1000004]
输出：[0,1,1000000,1000001,1000002,1000003,1000004,6]
解释：上图中蓝色的边和节点为答案链表。
```

__提示：__

- `3 <= list1.length <= 10 ^ 4`
- `1 <= a <= b < list1.length - 1`
- `1 <= list2.length <= 10 ^ 4`

__思路:__

```text
模拟
从前往后遍历 list1 找到第 a 个节点的前一个节点 p, 从 p 开始遍历到第 b 个节点的后一个节点 q
然后遍历 list2 找到尾节点 tail
将 tail 的下一个结点连接到 q, 将 p 的下一个结点连接到 list2 的头结点
返回 list1 即可
时间复杂度为 O(N), 空间复杂度为 O(N)
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
    ListNode* mergeInBetween(ListNode* list1, int a, int b, ListNode* list2) 
    {
        ListNode* p = list1;
        for (int i = 0; i < a - 1; i++) p = p -> next;
        ListNode* q = p -> next;
        for (int i = a; i <= b; i++) q = q -> next;
        ListNode* tail = list2;
        while (tail -> next) tail = tail -> next;
        tail -> next = q;
        p -> next = list2;
        return list1;
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
    public ListNode mergeInBetween(ListNode list1, int a, int b, ListNode list2) {
        ListNode p = list1;
        for (int i = 0; i < a - 1; i++) p = p.next;
        ListNode q = p.next;
        for (int i = a; i <= b; i++) q = q.next;
        ListNode tail = list2;
        while (tail.next != null) tail = tail.next;
        tail.next = q;
        p.next = list2;
        return list1;
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
    def mergeInBetween(self, list1: ListNode, a: int, b: int, list2: ListNode) -> ListNode:
        p = list1
        for _ in range(a - 1):
            p = p.next
        q,tail = p.next, list2
        for _ in range(a, b + 1):
            q = q.next
        while tail.next:
            tail = tail.next
        p.next, tail.next = list2, q
        return list1
```
