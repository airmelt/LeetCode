# 1206 Design Skiplist 设计跳表

__Description__:
Design a Skiplist without using any built-in libraries.

A skiplist is a data structure that takes O(log(n)) time to add, erase and search. Comparing with treap and red-black tree which has the same function and performance, the code length of Skiplist can be comparatively short and the idea behind Skiplists is just simple linked lists.

For example, we have a Skiplist containing [30,40,50,60,70,90] and we want to add 80 and 45 into it. The Skiplist works this way:

![skip_list](https://assets.leetcode.com/uploads/2019/09/27/1506_skiplist.gif)

Artyom Kalinin [CC BY-SA 3.0], via Wikimedia Commons

You can see there are many layers in the Skiplist. Each layer is a sorted linked list. With the help of the top layers, add, erase and search can be faster than O(n). It can be proven that the average time complexity for each operation is O(log(n)) and space complexity is O(n).

See more about Skiplist: <https://en.wikipedia.org/wiki/Skip_list>

Implement the Skiplist class:

Skiplist() Initializes the object of the skiplist.
bool search(int target) Returns true if the integer target exists in the Skiplist or false otherwise.
void add(int num) Inserts the value num into the SkipList.
bool erase(int num) Removes the value num from the Skiplist and returns true. If num does not exist in the Skiplist, do nothing and return false. If there exist multiple num values, removing any one of them is fine.
Note that duplicates may exist in the Skiplist, your code needs to handle this situation.

__Example:__

Example 1:

Input
["Skiplist", "add", "add", "add", "search", "add", "search", "erase", "erase", "search"]
[[], [1], [2], [3], [0], [4], [1], [0], [1], [1]]
Output
[null, null, null, null, false, null, true, false, true, false]

Explanation

```Java
Skiplist skiplist = new Skiplist();
skiplist.add(1);
skiplist.add(2);
skiplist.add(3);
skiplist.search(0); // return False
skiplist.add(4);
skiplist.search(1); // return True
skiplist.erase(0);  // return False, 0 is not in skiplist.
skiplist.erase(1);  // return True
skiplist.search(1); // return False, 1 has already been erased.
```

__Constraints:__

0 <= num, target <= 2 *10^4
At most 5* 10^4 calls will be made to search, add, and erase.

__题目描述__:
不使用任何库函数，设计一个 跳表 。

跳表 是在 O(log(n)) 时间内完成增加、删除、搜索操作的数据结构。跳表相比于树堆与红黑树，其功能与性能相当，并且跳表的代码长度相较下更短，其设计思想与链表相似。

例如，一个跳表包含 [30, 40, 50, 60, 70, 90] ，然后增加 80、45 到跳表中，以下图的方式操作：

![跳表](https://assets.leetcode.com/uploads/2019/09/27/1506_skiplist.gif)

Artyom Kalinin [CC BY-SA 3.0], via Wikimedia Commons

跳表中有很多层，每一层是一个短的链表。在第一层的作用下，增加、删除和搜索操作的时间复杂度不超过 O(n)。跳表的每一个操作的平均时间复杂度是 O(log(n))，空间复杂度是 O(n)。

了解更多 : <https://en.wikipedia.org/wiki/Skip_list>

在本题中，你的设计应该要包含这些函数：

bool search(int target) : 返回target是否存在于跳表中。
void add(int num): 插入一个元素到跳表。
bool erase(int num): 在跳表中删除一个值，如果 num 不存在，直接返回false. 如果存在多个 num ，删除其中任意一个即可。
注意，跳表中可能存在多个相同的值，你的代码需要处理这种情况。

__示例 :__

示例 1:

输入
["Skiplist", "add", "add", "add", "search", "add", "search", "erase", "erase", "search"]
[[], [1], [2], [3], [0], [4], [1], [0], [1], [1]]
输出
[null, null, null, null, false, null, true, false, true, false]

解释

```Java
Skiplist skiplist = new Skiplist();
skiplist.add(1);
skiplist.add(2);
skiplist.add(3);
skiplist.search(0);   // 返回 false
skiplist.add(4);
skiplist.search(1);   // 返回 true
skiplist.erase(0);    // 返回 false，0 不在跳表中
skiplist.erase(1);    // 返回 true
skiplist.search(1);   // 返回 false，1 已被擦除
```

__提示:__

0 <= num, target <= 2 *10^4
调用search, add,  erase操作次数不大于 5* 10^4

__思路__:

Redis 实现跳表
随机给跳表中的结点分配层数, 即这个结点会影响的层数, 实现为 randomLevel(), 在 Redis 中参数 p 为 1 / 4, 最大层数 maxLevel 为 32
结点包含两个参数, 第一个为值, 可以用模板, 本题中只用到正整数值, 可以设置头结点的值为 -1; 第二个值为对应每层之后的直接结点
插入结点时, 每次需要在某层找到刚好不小于结点值的位置, 并将新结点插入到其后, 实现为 find(), 如果找不到, 说明结点值为最大值, 实际上放在空结点的位置
删除时, 在每一层都查找结点值并进行删除, 只要删除了一个结点就返回 true
时间复杂度为 O(m + n ^ 2), 空间复杂度为 O(m + n ^ 2)

__代码__:
__C++__:

```C++
struct Node
{
    int val;
    vector<Node*> next;
    Node(int _val, int size): val(_val)
    {
        next.resize(size);
    }
};
class Skiplist
{
private:
    Node* head;
    int cur_level;
    constexpr static double P = 0.25;
    constexpr static int MAX_LEVEL = 32;
    
    static int random_level()
    {
        int level = 1;
        while (1.0 * rand() / RAND_MAX < P and level < MAX_LEVEL) ++level;
        return level;
    }
    
    Node* find(Node* node, int level, int val)
    {
        while (node -> next[level] and val > node -> next[level] -> val) node = node -> next[level];
        return node;
    }
public:
    Skiplist() 
    {
        head = new Node(-1, MAX_LEVEL);
        cur_level = 1;
    }
    
    bool search(int target) 
    {
        Node* cur = head;
        for (int i = cur_level - 1; i > -1; i--) 
        {
            cur = find(cur, i, target);
            if (cur -> next[i] and cur -> next[i] -> val == target) return true;
        }
        return false;
    }
    
    void add(int num) 
    {
        int level = random_level();
        Node* new_node = new Node(num, level);
        Node* update_node = head;
        for (int i = cur_level - 1; i > -1; i--) 
        {
            update_node = find(update_node, i, num);
            if (i < level)
            {
                if (!update_node -> next[i]) update_node -> next[i] = new_node;
                else
                {
                    Node* temp = update_node -> next[i];
                    update_node -> next[i] = new_node;
                    new_node -> next[i] = temp;
                }
            }
        }
        if (level > cur_level)
        {
            for (int i = cur_level; i < level; i++) head -> next[i] = new_node;
            cur_level = level;
        }
    }
    
    bool erase(int num) 
    {
        bool flag = false;
        Node* search_node = head;
        for (int i = cur_level - 1; i > -1; i--) 
        {
            search_node = find(search_node, i, num);
            if (search_node -> next[i] and search_node -> next[i] -> val == num) 
            {
                search_node -> next[i] = search_node -> next[i] -> next[i];
                flag = true;
                continue;
            }
        }
        return flag;
    }
};

/**
 * Your Skiplist object will be instantiated and called as such:
 * Skiplist* obj = new Skiplist();
 * bool param_1 = obj->search(target);
 * obj->add(num);
 * bool param_3 = obj->erase(num);
 */
```

__Java__:

```Java
class Skiplist {
    private static int DEFAULT_MAX_LEVEL = 32;
    private static double DEFAULT_P_FACTOR = 0.25;
    private Node head;
    private int currentLevel;
    
    public Skiplist() {
        head = new Node(-1, DEFAULT_MAX_LEVEL);
        currentLevel = 1;
    }
    
    class Node {
        int val;
        Node[] next;

        public Node(int val, int size) {
            this.val = val;
            this.next = new Node[size];
        }
    }

    public boolean search(int target) {
        Node cur = head;
        for (int i = currentLevel - 1; i > -1; i--) {
            cur = find(cur, i, target);
            if (cur.next[i] != null && cur.next[i].val == target) return true;
        }
        return false;
    }

    private Node find(Node node, int level, int val) {
        while (node.next[level] != null && val > node.next[level].val) node = node.next[level];
        return node;
    }

    private static int randomLevel() {
        int level = 1;
        while (Math.random() < DEFAULT_P_FACTOR && level < DEFAULT_MAX_LEVEL) ++level;
        return level;
    }
    
    public void add(int num) {
        int level = randomLevel();
        Node updateNode = head;
        Node newNode = new Node(num, level);
        for (int i = currentLevel - 1; i > -1; i--) {
            updateNode = find(updateNode, i, num);
            if (i < level) {
                if (updateNode.next[i] == null) updateNode.next[i] = newNode;
                else {
                    Node temp = updateNode.next[i];
                    updateNode.next[i] = newNode;
                    newNode.next[i] = temp;
                }
            }
        }
        if (level > currentLevel) {
            for (int i = currentLevel; i < level; i++) head.next[i] = newNode;
            currentLevel = level;
        }
    }

    public boolean erase(int num) {
        boolean flag = false;
        Node searchNode = head;
        for (int i = currentLevel - 1; i > -1; i--) {
            searchNode = find(searchNode, i, num);
            if (searchNode.next[i] != null && searchNode.next[i].val == num) {
                searchNode.next[i] = searchNode.next[i].next[i];
                flag = true;
                continue;
            }
        }
        return flag;
    }
}

/**
 * Your Skiplist object will be instantiated and called as such:
 * Skiplist obj = new Skiplist();
 * boolean param_1 = obj.search(target);
 * obj.add(num);
 * boolean param_3 = obj.erase(num);
 */
```

__Python__:

```Python
class Skiplist:

    def __init__(self):
        self.data = defaultdict(int)
        

    def search(self, target: int) -> bool:
        return self.data[target] > 0


    def add(self, num: int) -> None:
        self.data[num] += 1


    def erase(self, num: int) -> bool:
        if (n := self.data[num]) > 0:
            self.data[num] -= 1
        return n > 0



# Your Skiplist object will be instantiated and called as such:
# obj = Skiplist()
# param_1 = obj.search(target)
# obj.add(num)
# param_3 = obj.erase(num)
```
