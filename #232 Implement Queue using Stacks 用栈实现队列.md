__Description__:
Implement the following operations of a queue using stacks.

push(x) -- Push element x to the back of queue.
pop() -- Removes the element from in front of queue.
peek() -- Get the front element.
empty() -- Return whether the queue is empty.

**Example:**
```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```
__Notes:__

You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).

__题目描述__:
使用栈实现队列的下列操作：

push(x) -- 将一个元素放入队列的尾部。
pop() -- 从队列首部移除元素。
peek() -- 返回队列首部的元素。
empty() -- 返回队列是否为空。

**示例：**
```
MyQueue queue = new MyQueue();

queue.push(1);
queue.push(2);  
queue.peek();  // 返回 1
queue.pop();   // 返回 1
queue.empty(); // 返回 false
```
__说明:__

你只能使用标准的栈操作 -- 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）。

__思路__:
参考[LeetCode #225 Implement Stack using Queues 用队列实现栈](https://www.jianshu.com/p/194e7074a0f6)
使用 2个栈完成队列
- push()时间复杂度O(1), 空间复杂度O(1)
- pop()时间复杂度O(n), 空间复杂度O(n)
- top()时间复杂度O(n), 空间复杂度O(n)
- empty()时间复杂度O(1), 空间复杂度O(1)


__代码__:
__C++__:
```
class MyQueue {
public:
    /** Initialize your data structure here. */
    MyQueue() {

    }

    /** Push element x to the back of queue. */
    void push(int x) {
        in_stack.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        move(in_stack, out_stack);
        int result = out_stack.top();
        out_stack.pop();
        move(out_stack, in_stack);
        return result;
    }

    /** Get the front element. */
    int peek() {
        move(in_stack, out_stack);
        int result = out_stack.top();
        move(out_stack, in_stack);
        return result;
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        return in_stack.empty();
    }
private:
    stack<int> in_stack;
    stack<int> out_stack;
    void move(stack<int> &a, stack<int> &b) {
        while (!a.empty()) {
            b.push(a.top());
            a.pop();
        }
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

__Java__:
```
class MyQueue {

    private Stack<Integer> stack;
    private Stack<Integer> temp;
    /** Initialize your data structure here. */
    public MyQueue() {
        stack = new Stack<>();
        temp = new Stack<>();
    }

    /** Push element x to the back of queue. */
    public void push(int x) {
        stack.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    public int pop() {
        while (!stack.empty()) {
            temp.push(stack.pop());
        }
        int result = temp.pop();
        while (!temp.empty()) {
            stack.push(temp.pop());
        }
        return result;
    }

    /** Get the front element. */
    public int peek() {
        while (!stack.empty()) {
            temp.push(stack.pop());
        }
        int result = temp.peek();
        while (!temp.empty()) {
            stack.push(temp.pop());
        }
        return result;
    }

    /** Returns whether the queue is empty. */
    public boolean empty() {
        return stack.empty();
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue obj = new MyQueue();
 * obj.push(x);
 * int param_2 = obj.pop();
 * int param_3 = obj.peek();
 * boolean param_4 = obj.empty();
 */
```

__Python__:
```
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.stack = []

    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        self.stack.append(x)

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        temp = []
        while self.stack:
            temp.append(self.stack.pop())
        result = temp.pop()
        while temp:
            self.stack.append(temp.pop())
        return result

    def peek(self) -> int:
        """
        Get the front element.
        """
        temp = []
        while self.stack:
            temp.append(self.stack.pop())
        result = temp[-1]
        while temp:
            self.stack.append(temp.pop())
        return result

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        return len(self.stack) == 0


# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```
