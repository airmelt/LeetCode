# 1172 Dinner Plate Stacks 餐盘栈

__Description__:
You have an infinite number of stacks arranged in a row and numbered (left to right) from 0, each of the stacks has the same maximum capacity.

Implement the DinnerPlates class:

DinnerPlates(int capacity) Initializes the object with the maximum capacity of the stacks capacity.
void push(int val) Pushes the given integer val into the leftmost stack with a size less than capacity.
int pop() Returns the value at the top of the rightmost non-empty stack and removes it from that stack, and returns -1 if all the stacks are empty.
int popAtStack(int index) Returns the value at the top of the stack with the given index index and removes it from that stack or returns -1 if the stack with that given index is empty.

__Example:__

Example 1:

Input
["DinnerPlates", "push", "push", "push", "push", "push", "popAtStack", "push", "push", "popAtStack", "popAtStack", "pop", "pop", "pop", "pop", "pop"]
[[2], [1], [2], [3], [4], [5], [0], [20], [21], [0], [2], [], [], [], [], []]
Output
[null, null, null, null, null, null, 2, null, null, 20, 21, 5, 4, 3, 1, -1]

Explanation:

```Java
DinnerPlates D = DinnerPlates(2);  // Initialize with capacity = 2
D.push(1);
D.push(2);
D.push(3);
D.push(4);
D.push(5);         // The stacks are now:  2  4
                                           1  3  5
                                           ﹈ ﹈ ﹈
D.popAtStack(0);   // Returns 2.  The stacks are now:     4
                                                       1  3  5
                                                       ﹈ ﹈ ﹈
D.push(20);        // The stacks are now: 20  4
                                           1  3  5
                                           ﹈ ﹈ ﹈
D.push(21);        // The stacks are now: 20  4 21
                                           1  3  5
                                           ﹈ ﹈ ﹈
D.popAtStack(0);   // Returns 20.  The stacks are now:     4 21
                                                        1  3  5
                                                        ﹈ ﹈ ﹈
D.popAtStack(2);   // Returns 21.  The stacks are now:     4
                                                        1  3  5
                                                        ﹈ ﹈ ﹈ 
D.pop()            // Returns 5.  The stacks are now:      4
                                                        1  3 
                                                        ﹈ ﹈  
D.pop()            // Returns 4.  The stacks are now:   1  3 
                                                        ﹈ ﹈   
D.pop()            // Returns 3.  The stacks are now:   1 
                                                        ﹈   
D.pop()            // Returns 1.  There are no stacks.
D.pop()            // Returns -1.  There are still no stacks.
```

__Constraints:__

1 <= capacity <= 2 \* 10^4
1 <= val <= 2 \* 10^4
0 <= index <= 10^5
At most 2 \* 10^5 calls will be made to push, pop, and popAtStack.

__题目描述__:
我们把无限数量 ∞ 的栈排成一行，按从左到右的次序从 0 开始编号。每个栈的的最大容量 capacity 都相同。

实现一个叫「餐盘」的类 DinnerPlates：

DinnerPlates(int capacity) - 给出栈的最大容量 capacity。
void push(int val) - 将给出的正整数 val 推入 从左往右第一个 没有满的栈。
int pop() - 返回 从右往左第一个 非空栈顶部的值，并将其从栈中删除；如果所有的栈都是空的，请返回 -1。
int popAtStack(int index) - 返回编号 index 的栈顶部的值，并将其从栈中删除；如果编号 index 的栈是空的，请返回 -1。

__示例 :__

输入：
["DinnerPlates","push","push","push","push","push","popAtStack","push","push","popAtStack","popAtStack","pop","pop","pop","pop","pop"]
[[2],[1],[2],[3],[4],[5],[0],[20],[21],[0],[2],[],[],[],[],[]]
输出：
[null,null,null,null,null,null,2,null,null,20,21,5,4,3,1,-1]

解释：

```Java
DinnerPlates D = DinnerPlates(2);  // 初始化，栈最大容量 capacity = 2
D.push(1);
D.push(2);
D.push(3);
D.push(4);
D.push(5);         // 栈的现状为：    2  4
                                    1  3  5
                                    ﹈ ﹈ ﹈
D.popAtStack(0);   // 返回 2。栈的现状为：      4
                                          1  3  5
                                          ﹈ ﹈ ﹈
D.push(20);        // 栈的现状为：  20  4
                                   1  3  5
                                   ﹈ ﹈ ﹈
D.push(21);        // 栈的现状为：  20  4 21
                                   1  3  5
                                   ﹈ ﹈ ﹈
D.popAtStack(0);   // 返回 20。栈的现状为：       4 21
                                            1  3  5
                                            ﹈ ﹈ ﹈
D.popAtStack(2);   // 返回 21。栈的现状为：       4
                                            1  3  5
                                            ﹈ ﹈ ﹈ 
D.pop()            // 返回 5。栈的现状为：        4
                                            1  3 
                                            ﹈ ﹈  
D.pop()            // 返回 4。栈的现状为：    1  3 
                                           ﹈ ﹈   
D.pop()            // 返回 3。栈的现状为：    1 
                                           ﹈   
D.pop()            // 返回 1。现在没有栈。
D.pop()            // 返回 -1。仍然没有栈。
```

__提示:__

1 <= capacity <= 20000
1 <= val <= 20000
0 <= index <= 100000
最多会对 push，pop，和 popAtStack 进行 200000 次调用。

__思路__:

小根堆
用小根堆记录没有满的栈的下标, 这样堆顶就是最小的坐标
如果堆顶比栈的列表长度还要长, 说明堆顶已经失效, 可以直接清空
其他操作按题意模拟即可
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class DinnerPlates 
{
private:
    int capacity;
    vector<stack<int>> stacks;
    priority_queue<int, vector<int>, greater<int>>pq;
public:
    DinnerPlates(int capacity) 
    {
        this -> capacity = capacity;
        stacks.clear();
        pq.push(0);
    }
    
    void push(int val) 
    {
        int top = pq.top();
        if (top < stacks.size())
        {
            stacks[top].push(val);
            if (stacks[top].size() == capacity)
            {
                pq.pop();
                if (pq.empty()) pq.push(stacks.size());
            }
            return;
        }
        stacks.push_back(stack<int>());
        stacks.back().push(val);
        if (stacks[top].size() == capacity)
        {
            pq.pop();
            if (pq.empty()) pq.push(stacks.size());
        }
    }
    
    int pop() 
    {
        if (stacks.empty()) return -1;
        int result = stacks.back().top();
        stacks.back().pop();
        if (stacks.back().size() == capacity - 1) pq.push(stacks.size() - 1);
        while (!stacks.empty() and stacks.back().empty()) stacks.pop_back();
        return result;
    }
    
    int popAtStack(int index) 
    {
        if (index >= stacks.size() or stacks[index].empty()) return -1;
        int result = stacks[index].top();
        stacks[index].pop();
        if (stacks[index].size() == capacity - 1) pq.push(index);
        if (index == stacks.size() - 1 and stacks[index].empty()) stacks.pop_back();
        while (!stacks.empty() and stacks.back().empty()) stacks.pop_back();
        return result;
    }
};

/**
 * Your DinnerPlates object will be instantiated and called as such:
 * DinnerPlates* obj = new DinnerPlates(capacity);
 * obj->push(val);
 * int param_2 = obj->pop();
 * int param_3 = obj->popAtStack(index);
 */
```

__Java__:

```Java
class DinnerPlates {
        private List<Stack<Integer>> stacks;
        private int capacity;
        private PriorityQueue<Integer> queue;
    
        public DinnerPlates(int capacity) {
            stacks = new ArrayList<>();
            this.capacity = capacity;
            queue = new PriorityQueue<>();
        }

        public void push(int val) {
            if (!queue.isEmpty() && queue.peek() >= stacks.size()) queue.clear();
            if (queue.isEmpty()) {
                Stack<Integer> stack = new Stack<>();
                stack.push(val);
                stacks.add(stack);
                if (capacity > 1) queue.offer(stacks.size() - 1);
            } else {
                int index = queue.poll();
                stacks.get(index).add(val);
                if (stacks.get(index).size() < capacity) queue.offer(index);
            }
        }

        public int pop() {
            while (!stacks.isEmpty() && stacks.get(stacks.size() - 1).isEmpty()) stacks.remove(stacks.size() - 1);
            if (stacks.isEmpty()) return -1;
            int result = stacks.get(stacks.size() - 1).pop();
            if (stacks.get(stacks.size() - 1).isEmpty()) stacks.remove(stacks.size() - 1);
            if (!stacks.isEmpty() && stacks.get(stacks.size() - 1).size() != capacity) queue.offer(stacks.size() - 1);
            return result;
        }

        public int popAtStack(int index) {
            if (index >= stacks.size() || stacks.get(index).isEmpty()) return -1;
            if (stacks.get(index).size() == capacity) queue.offer(index);
            return stacks.get(index).pop();
        }
}

/**
 * Your DinnerPlates object will be instantiated and called as such:
 * DinnerPlates obj = new DinnerPlates(capacity);
 * obj.push(val);
 * int param_2 = obj.pop();
 * int param_3 = obj.popAtStack(index);
 */
```

__Python__:

```Python
class DinnerPlates:

    def __init__(self, capacity: int):
        self.stacks = []
        self.heap = []
        self.capacity = capacity
        

    def push(self, val: int) -> None:
        if self.heap and self.heap[0] >= len(self.stacks):
            self.heap = []
        if not self.heap:
            self.stacks.append([val])
            if self.capacity > 1:
                heappush(self.heap, len(self.stacks) - 1)
        else:
            self.stacks[index := heappop(self.heap)].append(val)
            if len(self.stacks[index]) < self.capacity:
                heappush(self.heap, index)


    def pop(self) -> int:
        while self.stacks and not self.stacks[-1]:
            self.stacks.pop()
        if not self.stacks: 
            return -1
        if self.stacks and len(self.stacks[-1]) == self.capacity:
            heappush(self.heap, len(self.stacks) - 1)
        return self.stacks[-1].pop()
        

    def popAtStack(self, index: int) -> int:
        if index >= len(self.stacks) or not self.stacks[index]: 
            return -1
        if len(self.stacks[index]) == self.capacity:
            heappush(self.heap, index)
        return self.stacks[index].pop()
        


# Your DinnerPlates object will be instantiated and called as such:
# obj = DinnerPlates(capacity)
# obj.push(val)
# param_2 = obj.pop()
# param_3 = obj.popAtStack(index)
```
