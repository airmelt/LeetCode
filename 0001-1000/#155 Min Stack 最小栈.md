# 155 Min Stack 最小栈

__Description__:
Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.

**Example:**

**Example 1:**

```text
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> Returns -3.
minStack.pop();
minStack.top();      --> Returns 0.
minStack.getMin();   --> Returns -2.
```

__题目描述__:
设计一个支持 push，pop，top 操作，并能在常数时间内检索到最小元素的栈。

push(x) -- 将元素 x 推入栈中。
pop() -- 删除栈顶的元素。
top() -- 获取栈顶元素。
getMin() -- 检索栈中的最小元素。

**示例:**

```text
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

__思路__:

栈可以用数组实现或者链表实现

1. 用一个栈记录最小值
2. 将最小值直接记录在这个栈
如, 输入 -2 -> 0 -> -3, 栈中的记录依次为 -2 -> -2 -> 0 -> -2 -> -3 -> -3
获取最小值直接返回最后一项即可

空间换时间
时间复杂度O(1), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class MinStack 
{
public:
    /** initialize your data structure here. */
    vector<int> data;
    MinStack() {}

    void push(int x) 
    {
        if (data.empty()) data.push_back(x);
        else data.push_back(min(x, data[data.size() - 2]));
        data.push_back(x);
    }

    void pop() 
    {
        data.pop_back();
        data.pop_back();
    }

    int top() 
    {
        return data.back();
    }

    int getMin() 
    {
        return data[data.size() - 2];
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

__Java__:

```Java
class MinStack {

    /** initialize your data structure here. */
    private List<Integer> list;
    public MinStack() {
        list = new ArrayList<>();
    }

    public void push(int x) {
        if (list.isEmpty()) list.add(x);
        else list.add(Math.min(x, list.get(list.size() - 2)));
        list.add(x);
    }

    public void pop() {
        list.remove(list.size() - 1);
        list.remove(list.size() - 1);
    }

    public int top() {
        if (list.size() != 0) return list.get(list.size() - 1);
        return 0;
    }

    public int getMin() {
        if (list.size() != 0) return list.get(list.size() - 2);
        return 0;
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

__Python__:

```Python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.data = []

    def push(self, x: int) -> None:
        if not self.data:
            self.data.append(x)
        else:
            self.data.append(min(self.data[-2], x))
        self.data.append(x)

    def pop(self) -> None:
        self.data.pop()
        self.data.pop()

    def top(self) -> int:
        if self.data:
            return self.data[-1]

    def getMin(self) -> int:
        if self.data:
            return self.data[-2]


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(x)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```
