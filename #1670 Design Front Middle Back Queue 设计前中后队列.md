# 1670 Design Front Middle Back Queue 设计前中后队列

__Description:__

Design a queue that supports `push` and `pop` operations in the front, middle, and back.

Implement the `FrontMiddleBack` class:

- `FrontMiddleBack()` Initializes the queue.
- `void pushFront(int val)` Adds `val` to the __front__ of the queue.
- `void pushMiddle(int val)` Adds `val` to the __middle__ of the queue.
- `void pushBack(int val)` Adds `val` to the __back__ of the queue.
- `int popFront()` Removes the __front__ element of the queue and returns it. If the queue is empty, return `-1`.
- `int popMiddle()` Removes the __middle__ element of the queue and returns it. If the queue is empty, return `-1`.
- `int popBack()` Removes the __back__ element of the queue and returns it. If the queue is empty, return `-1`.

Notice that when there are two middle position choices, the operation is performed on the frontmost middle position choice. For example:

- Pushing `6` into the middle of `[1, 2, 3, 4, 5]` results in `[1, 2, 6, 3, 4, 5]`.
- Popping the middle from `[1, 2, 3, 4, 5, 6]` returns `3` and results in `[1, 2, 4, 5, 6]`.

__Example:__

Example 1:

```text
Input:
["FrontMiddleBackQueue", "pushFront", "pushBack", "pushMiddle", "pushMiddle", "popFront", "popMiddle", "popMiddle", "popBack", "popFront"]
[[], [1], [2], [3], [4], [], [], [], [], []]
Output:
[null, null, null, null, null, 1, 3, 4, 2, -1]

Explanation:
FrontMiddleBackQueue q = new FrontMiddleBackQueue();
q.pushFront(1);   // [1]
q.pushBack(2);    // [1, 2]
q.pushMiddle(3);  // [1, 3, 2]
q.pushMiddle(4);  // [1, 4, 3, 2]
q.popFront();     // return 1 -> [4, 3, 2]
q.popMiddle();    // return 3 -> [4, 2]
q.popMiddle();    // return 4 -> [2]
q.popBack();      // return 2 -> []
q.popFront();     // return -1 -> [] (The queue is empty)
```

__Constraints:__

- `1 <= val <= 10 ^ 9`
- At most `1000` calls will be made to `pushFront`, `pushMiddle`, `pushBack`, `popFront`, `popMiddle`, and `popBack`.

__题目描述:__

请你设计一个队列，支持在前，中，后三个位置的 `push` 和 `pop` 操作。

请你完成 `FrontMiddleBack` 类:

- `FrontMiddleBack()` 初始化队列。
- `void pushFront(int val)` 将 `val` 添加到队列的 __最前面__ 。
- `void pushMiddle(int val)` 将 `val` 添加到队列的 __正中间__ 。
- `void pushBack(int val)` 将 `val` 添加到队里的 __最后面__ 。
- `int popFront()` 将 __最前面__ 的元素从队列中删除并返回值，如果删除之前队列为空，那么返回 `-1` 。
- `int popMiddle()` 将 _正中间_ 的元素从队列中删除并返回值，如果删除之前队列为空，那么返回 `-1` 。
- `int popBack()` 将 __最后面__ 的元素从队列中删除并返回值，如果删除之前队列为空，那么返回 `-1` 。

请注意当有 两个 中间位置的时候，选择靠前面的位置进行操作。比方说：

- 将 `6` 添加到 `[1, 2, 3, 4, 5]` 的中间位置，结果数组为 `[1, 2, __6__, 3, 4, 5]` 。
- 从 `[1, 2, __3__, 4, 5, 6]` 的中间位置弹出元素，返回 `3` ，数组变为 `[1, 2, 4, 5, 6]` 。

__示例:__

示例 1：

```text
输入：
["FrontMiddleBackQueue", "pushFront", "pushBack", "pushMiddle", "pushMiddle", "popFront", "popMiddle", "popMiddle", "popBack", "popFront"]
[[], [1], [2], [3], [4], [], [], [], [], []]
输出：
[null, null, null, null, null, 1, 3, 4, 2, -1]

解释：
FrontMiddleBackQueue q = new FrontMiddleBackQueue();
q.pushFront(1);   // [1]
q.pushBack(2);    // [1, 2]
q.pushMiddle(3);  // [1, 3, 2]
q.pushMiddle(4);  // [1, 4, 3, 2]
q.popFront();     // 返回 1 -> [4, 3, 2]
q.popMiddle();    // 返回 3 -> [4, 2]
q.popMiddle();    // 返回 4 -> [2]
q.popBack();      // 返回 2 -> []
q.popFront();     // 返回 -1 -> [] （队列为空）
```

__提示：__

- `1 <= val <= 10 ^ 9`
- 最多调用 `1000` 次 `pushFront`， `pushMiddle`， `pushBack`， `popFront`， `popMiddle` 和 `popBack` 。

__思路:__

```text
两个双端队列
before 保存前半部分元素
after 保存后半部分元素
保证 after.size() - before.size() <= 1
这样每次取中间的数或者插入到中间, 可以通过插入到 before 尾端或者 after 前端来实现
每个操作函数的时间复杂度均为 O(1), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class FrontMiddleBackQueue 
{
public:
    FrontMiddleBackQueue() {}
    
    void pushFront(int val) 
    {
        before.push_front(val);
        if (before.size() - after.size() > 0) 
        {
            after.push_front(before.back());
            before.pop_back();
        }
    }
    
    void pushMiddle(int val) 
    {
        if (before.size() < after.size()) before.push_back(val);
        else after.push_front(val);
    }
    
    void pushBack(int val) 
    {
        after.push_back(val);
        if (after.size() - before.size() > 1) 
        {
            before.push_back(after.front());
            after.pop_front();
        }
    }
    
    int popFront() 
    {
        int result = -1;
        if (!before.empty()) 
        {
            result = before.front();
            before.pop_front();
            if (after.size() - before.size() > 1) 
            {
                before.push_back(after.front());
                after.pop_front();
            }
        }
        else if (!after.empty()) 
        {
            result = after.front();
            after.pop_front();
        }
        return result;
    }
    
    int popMiddle() 
    {
        int result = -1;
        if (after.size() > before.size()) 
        {
            result = after.front();
            after.pop_front();
        }
        else if (before.size())
        {
            result = before.back();
            before.pop_back();
        }
        return result;
    }
    
    int popBack() 
    {
        if (after.empty()) return -1;
        int result = after.back();
        after.pop_back();
        if (before.size() > after.size()) 
        {
            after.push_front(before.back());
            before.pop_back();
        }
        return result;
    }
private:
    deque<int> before, after;
};

/**
 * Your FrontMiddleBackQueue object will be instantiated and called as such:
 * FrontMiddleBackQueue* obj = new FrontMiddleBackQueue();
 * obj->pushFront(val);
 * obj->pushMiddle(val);
 * obj->pushBack(val);
 * int param_4 = obj->popFront();
 * int param_5 = obj->popMiddle();
 * int param_6 = obj->popBack();
 */
```

__Java__:

```Java
class FrontMiddleBackQueue {
    private LinkedList<Integer> list = new LinkedList<>();

    public FrontMiddleBackQueue() {}
    
    public void pushFront(int val) {
        list.add(0, val);
    }
    
    public void pushMiddle(int val) {
        list.add(list.size() >>> 1, val);
    }
    
    public void pushBack(int val) {
        list.add(val);
    }
    
    public int popFront() {
        if (list.isEmpty()) return -1;
        int result = list.get(0);
        list.removeFirst();
        return result;
    }
    
    public int popMiddle() {
        if (list.isEmpty()) return -1;
        int mid = (list.size() - 1) >>> 1, result = list.get(mid);
        list.remove(mid);
        return result;
    }
    
    public int popBack() {
        if (list.isEmpty()) return -1;
        int result = list.get(list.size() - 1);
        list.removeLast();
        return result;
    }
}

/**
 * Your FrontMiddleBackQueue object will be instantiated and called as such:
 * FrontMiddleBackQueue obj = new FrontMiddleBackQueue();
 * obj.pushFront(val);
 * obj.pushMiddle(val);
 * obj.pushBack(val);
 * int param_4 = obj.popFront();
 * int param_5 = obj.popMiddle();
 * int param_6 = obj.popBack();
 */
```

__Python__:

```Python
class FrontMiddleBackQueue:

    def __init__(self):
        self.data = []


    def pushFront(self, val: int) -> None:
        self.data = [val] + self.data


    def pushMiddle(self, val: int) -> None:
        self.data[(mid := len(self.data) >> 1):mid] = [val]


    def pushBack(self, val: int) -> None:
        self.data.append(val)


    def popFront(self) -> int:
        if not self.data:
            return -1
        result = self.data[0]
        del self.data[0]
        return result


    def popMiddle(self) -> int:
        if not self.data:
            return -1
        result = self.data[mid := ((len(self.data) - 1) >> 1)]
        del self.data[mid]
        return result


    def popBack(self) -> int:
        if not self.data:
            return -1
        result = self.data[-1]
        del self.data[-1]
        return result


# Your FrontMiddleBackQueue object will be instantiated and called as such:
# obj = FrontMiddleBackQueue()
# obj.pushFront(val)
# obj.pushMiddle(val)
# obj.pushBack(val)
# param_4 = obj.popFront()
# param_5 = obj.popMiddle()
# param_6 = obj.popBack()
```
