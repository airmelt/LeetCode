# 2130 Maximum Twin Sum of a Linked List 链表最大孪生和

__Description:__

In a linked list of size `n`, where `n` is __even__, the `i ^ th` node (__0-indexed__) of the linked list is known as the __twin__ of the `(n-1-i) ^ th` node, if `0 <= i <= (n / 2) - 1`.

- For example, if `n = 4`, then node `0` is the twin of node `3`, and node `1` is the twin of node `2`. These are the only nodes with twins for `n = 4`.

The twin sum is defined as the sum of a node and its twin.

Given the `head` of a linked list with even length, return _the __maximum twin sum__ of the linked list_.

__Example:__

Example 1:

![2130-1](https://assets.leetcode.com/uploads/2021/12/03/eg1drawio.png)

```text
Input: head = [5,4,2,1]
Output: 6
Explanation:
Nodes 0 and 1 are the twins of nodes 3 and 2, respectively. All have twin sum = 6.
There are no other nodes with twins in the linked list.
Thus, the maximum twin sum of the linked list is 6.
```

Example 2:

![2130-2](https://assets.leetcode.com/uploads/2021/12/03/eg2drawio.png)

```text
Input: head = [4,2,2,3]
Output: 7
Explanation:
The nodes with twins present in this linked list are:
```

- Node 0 is the twin of node 3 having a twin sum of 4 + 3 = 7.
- Node 1 is the twin of node 2 having a twin sum of 2 + 2 = 4.

Thus, the maximum twin sum of the linked list is max(7, 4) = 7.

Example 3:

![2130-3](https://assets.leetcode.com/uploads/2021/12/03/eg3drawio.png)

```text
Input: head = [1,100000]
Output: 100001
Explanation:
There is only one node with a twin in the linked list having twin sum of 1 + 100000 = 100001.
```

__Constraints:__

- The number of nodes in the list is an __even__ integer in the range `[2, 10 ^ 5]`.
- `1 <= Node.val <= 10 ^ 5`

__题目描述:__

在一个大小为 `n` 且 `n` 为 __偶数__ 的链表中，对于 `0 <= i <= (n / 2) - 1` 的 `i` ，第 `i` 个节点（下标从 __0__ 开始）的孪生节点为第 `(n-1-i)` 个节点 。

- 比方说， `n = 4` 那么节点 `0` 是节点 `3` 的孪生节点，节点 `1` 是节点 `2` 的孪生节点。这是长度为 `<span>n = 4</span>` 的链表中所有的孪生节点。

孪生和 定义为一个节点和它孪生节点两者值之和。

给你一个长度为偶数的链表的头节点 `head` ，请你返回链表的 __最大孪生和__ 。

__示例:__

示例 1：

![2130-4](https://assets.leetcode.com/uploads/2021/12/03/eg1drawio.png)

```text
输入：head = [5,4,2,1]
输出：6
解释：
节点 0 和节点 1 分别是节点 3 和 2 的孪生节点。孪生和都为 6 。
链表中没有其他孪生节点。
所以，链表的最大孪生和是 6 。
```

示例 2：

![2130-5](https://assets.leetcode.com/uploads/2021/12/03/eg2drawio.png)

```text
输入：head = [4,2,2,3]
输出：7
解释：
链表中的孪生节点为：
```

- 节点 0 是节点 3 的孪生节点，孪生和为 4 + 3 = 7 。
- 节点 1 是节点 2 的孪生节点，孪生和为 2 + 2 = 4 。

所以，最大孪生和为 max(7, 4) = 7 。

示例 3：

![2130-6](https://assets.leetcode.com/uploads/2021/12/03/eg3drawio.png)

```text
输入：head = [1,100000]
输出：100001
解释：
链表中只有一对孪生节点，孪生和为 1 + 100000 = 100001 。
```

__提示：__

- 链表的节点数目是 `[2, 10 ^ 5]` 中的 __偶数__ 。
- `1 <= Node.val <= 10 ^ 5`

__思路:__

```text
1. 数组
转化为数组
遍历 arr[i] + arr[-i - 1] 取最大值
时间复杂度为 O(N), 空间复杂度为 O(N)
2. 反转链表 ➕ 快慢指针
用快慢指针找到链表的中点
将链表中点之后的链表反转
分别遍历 head 和反转的链表
记录值的和的最大值
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
    int pairSum(ListNode* head) 
    {
        ListNode *slow = head, *fast = head -> next;
        while (fast -> next)
        {
            slow = slow -> next;
            fast = fast -> next -> next;
        }
        ListNode* last = slow -> next;
        while (last -> next)
        {
            ListNode* cur = last -> next;
            last -> next = cur -> next;
            cur -> next = slow -> next;
            slow -> next = cur;
        }
        int result = 0;
        while (slow -> next)
        {
            result = max(result, head -> val + slow -> next -> val);
            head = head -> next;
            slow = slow -> next;
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
    public int pairSum(ListNode head) {
        List<Integer> list = new ArrayList<>();
        while (head != null) {
            list.add(head.val);
            head = head.next;
        }
        int result = 0;
        for (int i = 0, n = list.size(), half = n >> 1; i < half; i++) result = Math.max(result, list.get(i) + list.get(n - i - 1));
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
    def pairSum(self, head: Optional[ListNode]) -> int:
        arr = []
        while head:
            arr.append(head.val)
            head = head.next
        return max(arr[i] + arr[-i - 1] for i in range(len(arr) >> 1))
```
