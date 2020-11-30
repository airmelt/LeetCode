__Description__:
Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.

__Follow up:__
What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?

__Example:__

// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
solution.getRandom();

__题目描述__:
给定一个单链表，随机选择链表的一个节点，并返回相应的节点值。保证每个节点被选的概率一样。

__进阶:__
如果链表十分大且长度未知，如何解决这个问题？你能否使用常数级空间复杂度实现？

__示例 :__

// 初始化一个单链表 [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom()方法应随机返回1,2,3中的一个，保证每个元素被返回的概率相等。
solution.getRandom();

__思路__:
取最后一个节点并替换为结果的概率为 1 / n
设取倒数第二个节点的概率为 x, 则 x * (1 - 1 / n) = 1 / n, x = 1 / (n - 1), 表示取倒数第二个节点, 并且不取最后一个节点
那么可以每次生成一个 [0, k]的随机数, 如果随机到 0就替换
这样就可以保证每次取出的节点的概率为 1 / n
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
class Solution {
public:
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head): head(head) {

    }
    
    /** Returns a random node's value. */
    int getRandom() {
        ListNode* cur = head -> next;
        int result = head -> val, i = 2;
        while (cur) 
        {
            if (!(rand() % i)) result = cur -> val;
            i++;
            cur = cur -> next;
        }
        return result;
    }
private:
    ListNode* head;
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */
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
    
    private ListNode head;

    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    public Solution(ListNode head) {
        this.head = head;
    }
    
    /** Returns a random node's value. */
    public int getRandom() {
        ListNode cur = head.next;
        int result = head.val, i = 2;
        Random random = new Random();
        while (cur != null) {
            if (random.nextInt(i) == 0) result = cur.val;
            i++;
            cur = cur.next;
        }
        return result;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(head);
 * int param_1 = obj.getRandom();
 */
```

__Python__:
```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:

    def __init__(self, head: ListNode):
        """
        @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node.
        """
        self.head = head
        

    def getRandom(self) -> int:
        """
        Returns a random node's value.
        """
        result, i, cur = self.head.val, 2, self.head.next
        while cur:
            if not random.randrange(i):
                result = cur.val
            i += 1
            cur = cur.next
        return result
        


# Your Solution object will be instantiated and called as such:
# obj = Solution(head)
# param_1 = obj.getRandom()
```