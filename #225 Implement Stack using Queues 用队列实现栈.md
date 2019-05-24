__Description__:
Implement the following operations of a stack using queues.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- empty() -- Return whether the stack is empty.

**Example:**
```
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
```

__Notes:__

- You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.
- Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
- You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).

__题目描述__:
使用队列实现栈的下列操作：

- push(x) -- 元素 x 入栈
- pop() -- 移除栈顶元素
- top() -- 获取栈顶元素
- empty() -- 返回栈是否为空

__注意:__

- 你只能使用队列的基本操作-- 也就是 push to back, peek/pop from front, size, 和 is empty 这些操作是合法的。
- 你所使用的语言也许不支持队列。 你可以使用 list 或者 deque（双端队列）来模拟一个队列 , 只要是标准的队列操作即可。
- 你可以假设所有操作都是有效的（例如, 对一个空的栈不会调用 pop 或者 top 操作）。

__思路__:
队列和栈 push操作一样, 但是队列只能从队首(front)弹出, 栈只能从栈顶(back)弹出
1. push的时候将队列的元素依次弹出, push到队尾, 其他函数可以设计的一样
2. 设计两个队列, 一个队列用来记录输入, 一个队列用来弹出
- push()时间复杂度O(n), 空间复杂度O(n)
- pop()时间复杂度O(1), 空间复杂度O(1)
- top()时间复杂度O(1), 空间复杂度O(1)
- empty()时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```
class MyStack {
public:
    /** Initialize your data structure here. */
    queue<int> q;
    MyStack() {
    }

    /** Push element x onto stack. */
    void push(int x) {
        q.push(x);
        for (int i = 0; i < q.size() - 1; i++) {
            q.push(q.front());
            q.pop();
        }
    }

    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        int result = q.front();
        q.pop();
        return result;
    }

    /** Get the top element. */
    int top() {
        return q.front();
    }

    /** Returns whether the stack is empty. */
    bool empty() {
        return q.empty();
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

__Java__:
```
class MyStack {

    /** Initialize your data structure here. */
    public Queue<Integer> stack;
    public MyStack() {
        stack = new LinkedList<>();
    }

    /** Push element x onto stack. */
    public void push(int x) {
        stack.offer(x);
        for (int i = 0; i < stack.size() - 1; i++) {
            Integer result = stack.poll();
            stack.offer(result);
        }
    }

    /** Removes the element on top of the stack and returns that element. */
    public int pop() {
        return stack.poll();
    }

    /** Get the top element. */
    public int top() {
        return stack.peek();
    }

    /** Returns whether the stack is empty. */
    public boolean empty() {
        return stack.isEmpty();
    }
}

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack obj = new MyStack();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.top();
 * boolean param_4 = obj.empty();
 */
```

__Python__:
```
class MyStack:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.input = []
        self.output = []

    def push(self, x: int) -> None:
        """
        Push element x onto stack.
        """
        self.input.append(x)
        while self.output:
            self.input.append(self.output.pop(0))
        self.output, self.input = self.input, []

    def pop(self) -> int:
        """
        Removes the element on top of the stack and returns that element.
        """
        return self.output.pop(0)

    def top(self) -> int:
        """
        Get the top element.
        """
        return self.output[0]


    def empty(self) -> bool:
        """
        Returns whether the stack is empty.
        """
        return not self.output


# Your MyStack object will be instantiated and called as such:
# obj = MyStack()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.top()
# param_4 = obj.empty()
```
