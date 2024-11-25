# 2349 Design a Number Container System 设计数字容器系统

__Description:__

Design a number container system that can do the following:

- __Insert__ or __Replace__ a number at the given index in the system.
- __Return__ the smallest index for the given number in the system.

Implement the `NumberContainers` class:

- `NumberContainers()` Initializes the number container system.
- `void change(int index, int number)` Fills the container at `index` with the `number`. If there is already a number at that `index`, replace it.
- `int find(int number)` Returns the smallest index for the given `number`, or `-1` if there is no index that is filled by `number` in the system.

__Example:__

Example 1:

```text
Input
["NumberContainers", "find", "change", "change", "change", "change", "find", "change", "find"]
[[], [10], [2, 10], [1, 10], [3, 10], [5, 10], [10], [1, 20], [10]]
Output
[null, -1, null, null, null, null, 1, null, 2]

Explanation
```

```Java
NumberContainers nc = new NumberContainers();
nc.find(10); // There is no index that is filled with number 10. Therefore, we return -1.
nc.change(2, 10); // Your container at index 2 will be filled with number 10.
nc.change(1, 10); // Your container at index 1 will be filled with number 10.
nc.change(3, 10); // Your container at index 3 will be filled with number 10.
nc.change(5, 10); // Your container at index 5 will be filled with number 10.
nc.find(10); // Number 10 is at the indices 1, 2, 3, and 5. Since the smallest index that is filled with 10 is 1, we return 1.
nc.change(1, 20); // Your container at index 1 will be filled with number 20. Note that index 1 was filled with 10 and then replaced with 20. 
nc.find(10); // Number 10 is at the indices 2, 3, and 5. The smallest index that is filled with 10 is 2. Therefore, we return 2.
```

__Constraints:__

- `1 <= index, number <= 10 ^ 9`
- At most `10 ^ 5` calls will be made __in total__ to `change` and `find`.

__题目描述:__

设计一个数字容器系统，可以实现以下功能：

- 在系统中给定下标处 __插入__ 或者 __替换__ 一个数字。
- __返回__ 系统中给定数字的最小下标。

请你实现一个 `NumberContainers` 类:

- `NumberContainers()` 初始化数字容器系统。
- `void change(int index, int number)` 在下标 `index` 处填入 `number` 。如果该下标 `index` 处已经有数字了，那么用 `number` 替换该数字。
- `int find(int number)` 返回给定数字 `number` 在系统中的最小下标。如果系统中没有 `number` ，那么返回 `-1` 。

__示例:__

示例：

```text
输入：
["NumberContainers", "find", "change", "change", "change", "change", "find", "change", "find"]
[[], [10], [2, 10], [1, 10], [3, 10], [5, 10], [10], [1, 20], [10]]
输出：
[null, -1, null, null, null, null, 1, null, 2]

解释：
```

```Java
NumberContainers nc = new NumberContainers();
nc.find(10); // 没有数字 10 ，所以返回 -1 。
nc.change(2, 10); // 容器中下标为 2 处填入数字 10 。
nc.change(1, 10); // 容器中下标为 1 处填入数字 10 。
nc.change(3, 10); // 容器中下标为 3 处填入数字 10 。
nc.change(5, 10); // 容器中下标为 5 处填入数字 10 。
nc.find(10); // 数字 10 所在的下标为 1 ，2 ，3 和 5 。因为最小下标为 1 ，所以返回 1 。
nc.change(1, 20); // 容器中下标为 1 处填入数字 20 。注意，下标 1 处之前为 10 ，现在被替换为 20 。
nc.find(10); // 数字 10 所在下标为 2 ，3 和 5 。最小下标为 2 ，所以返回 2 。
```

__提示：__

- `1 <= index, number <= 10 ^ 9`
- 调用 `change` 和 `find` 的 __总次数__ 不超过 `10 ^ 5` 次。

__思路:__

```text
哈希表 ➕ 小根堆
用哈希表记录每个下标处的数字
替换时直接更新哈希表
用小根堆记录每个数字的下标
替换时直接将下标插入小根堆
查找的时候将不属于该数字的下标弹出
change() 和 find() 时间复杂度均为 O(logN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class NumberContainers 
{
public:
    void change(int index, int number) 
    {
        m[index] = number;
        q[number].push(index);
    }

    int find(int number) 
    {
        auto it = q.find(number);
        if (it == q.end()) return -1;
        auto &h = it -> second;
        while (!h.empty() and m[h.top()] != number) h.pop();
        return h.empty() ? -1 : h.top();
    }
private:
    unordered_map<int, int> m;
    unordered_map<int, priority_queue<int, vector<int>, greater<>>> q;
};

/**
 * Your NumberContainers object will be instantiated and called as such:
 * NumberContainers* obj = new NumberContainers();
 * obj->change(index,number);
 * int param_2 = obj->find(number);
 */
```

__Java__:

```Java
class NumberContainers {
    private Map<Integer, Integer> m = new HashMap<>();
    private Map<Integer, Queue<Integer>> q = new HashMap<>();
    
    public void change(int index, int number) {
        m.put(index, number);
        q.computeIfAbsent(number, k -> new PriorityQueue<>()).offer(index);
    }
    
    public int find(int number) {
        var h = q.get(number);
        while (h != null && !h.isEmpty() && m.get(h.peek()) != number) h.poll();
        return h == null || h.isEmpty() ? -1 : h.peek();
    }
}

/**
 * Your NumberContainers object will be instantiated and called as such:
 * NumberContainers obj = new NumberContainers();
 * obj.change(index,number);
 * int param_2 = obj.find(number);
 */
```

__Python__:

```Python
class NumberContainers:
    def __init__(self):
        self.m = {}
        self.q = defaultdict(list)

    def change(self, index: int, number: int) -> None:
        self.m[index] = number
        heappush(self.q[number], index)
        
    def find(self, number: int) -> int:
        h = self.q[number]
        while h and self.m[h[0]] != number:
            heappop(h)
        return h[0] if h else -1



# Your NumberContainers object will be instantiated and called as such:
# obj = NumberContainers()
# obj.change(index,number)
# param_2 = obj.find(number)
```
