__Description__:
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

The Linked List is represented in the input/output as a list of n nodes. Each node is represented as a pair of [val, random_index] where:

val: an integer representing Node.val
random_index: the index of the node (range from 0 to n-1) where random pointer points to, or null if it does not point to any node.
 
__Example:__
Example 1:
![graph 1](https://upload-images.jianshu.io/upload_images/16639143-dd37f2b7272cf688.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]

Example 2:
![graph 2](https://upload-images.jianshu.io/upload_images/16639143-5ab044f5f287170e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: head = [[1,1],[2,1]]
Output: [[1,1],[2,1]]

Example 3:
![graph 3](https://upload-images.jianshu.io/upload_images/16639143-fd09d9175032443f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Input: head = [[3,null],[3,0],[3,null]]
Output: [[3,null],[3,0],[3,null]]

Example 4:

Input: head = []
Output: []
Explanation: Given linked list is empty (null pointer), so return null.

__Constraints:__

-10000 <= Node.val <= 10000
Node.random is null or pointing to a node in the linked list.
Number of Nodes will not exceed 1000.

__题目描述__:
给定一个链表，每个节点包含一个额外增加的随机指针，该指针可以指向链表中的任何节点或空节点。

要求返回这个链表的 深拷贝。 

我们用一个由 n 个节点组成的链表来表示输入/输出中的链表。每个节点用一个 [val, random_index] 表示：

val：一个表示 Node.val 的整数。
random_index：随机指针指向的节点索引（范围从 0 到 n-1）；如果不指向任何节点，则为  null 。
 
__示例 :__
示例 1：
![图 1](https://upload-images.jianshu.io/upload_images/16639143-dd37f2b7272cf688.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
输出：[[7,null],[13,0],[11,4],[10,2],[1,0]]

示例 2：
![图 2](https://upload-images.jianshu.io/upload_images/16639143-5ab044f5f287170e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：head = [[1,1],[2,1]]
输出：[[1,1],[2,1]]

示例 3：
![图 3](https://upload-images.jianshu.io/upload_images/16639143-fd09d9175032443f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
输入：head = [[3,null],[3,0],[3,null]]
输出：[[3,null],[3,0],[3,null]]

示例 4：

输入：head = []
输出：[]
解释：给定的链表为空（空指针），因此返回 null。

__提示：__

-10000 <= Node.val <= 10000
Node.random 为空（null）或指向链表中的节点。
节点数目不超过 1000 。

__思路__:
将新结点先插入到原链表中, 就插入到原结点边上
```
1 -> 1' -> 2 -> 2' -> 3 -> 3'
```
然后处理 random结点
最后分离出拷贝后的结点
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/

class Solution 
{
public:
    Node* copyRandomList(Node* head) 
    {
        if (!head) return head;
        Node *p = head;
        while (p)
        {
            Node *q = new Node(p -> val), *r = p -> next;
            q -> next = p -> next;
            p -> next = q;
            p = r;
        }
        p = head;
        while (p)
        {
            if (p -> random) p -> next -> random = p -> random -> next;
            p = p -> next -> next;
        }
        p = head;
        Node *result = head -> next;
        while (p -> next)
        {
            Node *q = p -> next;
            p -> next = p -> next -> next;
            p = q;
        }
        return result;
    }
};
```

__Java__:
```Java
/*
// Definition for a Node.
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
*/

class Solution {
    public Node copyRandomList(Node head) {
        if (head == null) return head;
        Node p = head;
        while (p != null) {
            Node q = new Node(p.val), r = p.next;
            q.next = p.next;
            p.next = q;
            p = r;
        }
        p = head;
        while (p != null) {
            if (p.random != null) p.next.random = p.random.next;
            p = p.next.next;
        }
        p = head;
        Node result = head.next;
        while (p.next != null) {
            Node q = p.next;
            p.next = p.next.next;
            p = q;
        }
        return result;
    }
}
```

__Python__:
```Python
"""
# Definition for a Node.
class Node:
    def __init__(self, x: int, next: 'Node' = None, random: 'Node' = None):
        self.val = int(x)
        self.next = next
        self.random = random
"""

class Solution:
    def copyRandomList(self, head: 'Node') -> 'Node':
        return copy.deepcopy(head)
```