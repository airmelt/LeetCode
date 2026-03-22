# 1019 Next Greater Node In Linked List 链表中的下一个更大节点

__Description__:
You are given the head of a linked list with n nodes.

For each node in the list, find the value of the next greater node. That is, for each node, find the value of the first node that is next to it and has a strictly larger value than it.

Return an integer array answer where answer[i] is the value of the next greater node of the ith node (1-indexed). If the ith node does not have a next greater node, set answer[i] = 0.

__Example:__

Example 1:

![Linked List 1](https://upload-images.jianshu.io/upload_images/16639143-4020f73df0543313.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input: head = [2,1,5]
Output: [5,5,0]

Example 2:

![Linked List 2](https://upload-images.jianshu.io/upload_images/16639143-e26d175f7cb479dd.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Input: head = [2,7,4,3,5]
Output: [7,0,5,5,0]

__Constraints:__

The number of nodes in the list is n.
1 <= n <= 10^4
1 <= Node.val <= 10^9

__题目描述__:
给定一个长度为 n 的链表 head

对于列表中的每个节点，查找下一个 更大节点 的值。也就是说，对于每个节点，找到它旁边的第一个节点的值，这个节点的值 严格大于 它的值。

返回一个整数数组 answer ，其中 answer[i] 是第 i 个节点( 从1开始 )的下一个更大的节点的值。如果第 i 个节点没有下一个更大的节点，设置 answer[i] = 0 。

__示例 :__

示例 1：

![链表 1](https://upload-images.jianshu.io/upload_images/16639143-c38a63e2b590dd35.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入：head = [2,1,5]
输出：[5,5,0]

示例 2：

![链表 2](https://upload-images.jianshu.io/upload_images/16639143-5bc9b6a7c095e7e2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入：head = [2,7,4,3,5]
输出：[7,0,5,5,0]

__提示:__

链表中节点数为 n
1 <= n <= 10^4
1 <= Node.val <= 10^9

__思路__:

单调栈
遍历链表获取链表的长度及对应的下标
单调栈中存放递增的链表值对应的下标
遍历下标, 遇到比当前值小的栈中值就弹出并将对应结果赋予当前链表的值
时间复杂度为 O(n), 空间复杂度为 O(n)

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
    vector<int> nextLargerNodes(ListNode* head) 
    {
        vector<int> nums;
        while (head) 
        {
            nums.push_back(head -> val);
            head = head -> next;
        }
        int n = nums.size();
        vector<int> result(n, 0);
        stack<int> s;
        for (int i = 0; i < n; i++) 
        {
            while (!s.empty() and nums[i] > nums[s.top()]) 
            {
                result[s.top()] = nums[i];
                s.pop();
            }
            s.push(i);
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
    public int[] nextLargerNodes(ListNode head) {
        List<Integer> list = new ArrayList<>();
        while (head != null) {
            list.add(head.val);
            head = head.next;
        }
        int n = list.size(), result[] = new int[n];
        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && list.get(i) > list.get(stack.peek()))     result[stack.pop()] = list.get(i);
            stack.push(i);
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
    def nextLargerNodes(self, head: Optional[ListNode]) -> List[int]:
        result, stack, i = [], [], 0
        while head:
            while stack and stack[-1][1] < head.val:
                result[stack.pop()[0]] = head.val
            stack.append((i, head.val))
            result.append(0)
            i += 1
            head = head.next
        return result
```
