# 895 Maximum Frequency Stack 最大频率栈

__Description__:
Design a stack-like data structure to push elements to the stack and pop the most frequent element from the stack.

Implement the FreqStack class:

FreqStack() constructs an empty frequency stack.
void push(int val) pushes an integer val onto the top of the stack.
int pop() removes and returns the most frequent element in the stack.
If there is a tie for the most frequent element, the element closest to the stack's top is removed and returned.

__Example:__

Example 1:

Input
["FreqStack", "push", "push", "push", "push", "push", "push", "pop", "pop", "pop", "pop"]
[[], [5], [7], [5], [7], [4], [5], [], [], [], []]
Output
[null, null, null, null, null, null, null, 5, 7, 5, 4]

Explanation

```Java
FreqStack freqStack = new FreqStack();
freqStack.push(5); // The stack is [5]
freqStack.push(7); // The stack is [5,7]
freqStack.push(5); // The stack is [5,7,5]
freqStack.push(7); // The stack is [5,7,5,7]
freqStack.push(4); // The stack is [5,7,5,7,4]
freqStack.push(5); // The stack is [5,7,5,7,4,5]
freqStack.pop();   // return 5, as 5 is the most frequent. The stack becomes [5,7,5,7,4].
freqStack.pop();   // return 7, as 5 and 7 is the most frequent, but 7 is closest to the top. The stack becomes [5,7,5,4].
freqStack.pop();   // return 5, as 5 is the most frequent. The stack becomes [5,7,4].
freqStack.pop();   // return 4, as 4, 5 and 7 is the most frequent, but 4 is closest to the top. The stack becomes [5,7].
```

__Constraints:__

0 <= val <= 10^9
At most 2 * 10^4 calls will be made to push and pop.
It is guaranteed that there will be at least one element in the stack before calling pop.

__题目描述__:
实现 FreqStack，模拟类似栈的数据结构的操作的一个类。

FreqStack 有两个函数：

push(int x)，将整数 x 推入栈中。
pop()，它移除并返回栈中出现最频繁的元素。
如果最频繁的元素不只一个，则移除并返回最接近栈顶的元素。

__示例 :__

输入：
["FreqStack","push","push","push","push","push","push","pop","pop","pop","pop"],
[[],[5],[7],[5],[7],[4],[5],[],[],[],[]]
输出：[null,null,null,null,null,null,null,5,7,5,4]
解释：
执行六次 .push 操作后，栈自底向上为 [5,7,5,7,4,5]。然后：

pop() -> 返回 5，因为 5 是出现频率最高的。
栈变成 [5,7,5,7,4]。

pop() -> 返回 7，因为 5 和 7 都是频率最高的，但 7 最接近栈顶。
栈变成 [5,7,5,4]。

pop() -> 返回 5 。
栈变成 [5,7,4]。

pop() -> 返回 4 。
栈变成 [5,7]。

__提示:__

对 FreqStack.push(int x) 的调用中 0 <= x <= 10^9。
如果栈的元素数目为零，则保证不会调用  FreqStack.pop()。
单个测试样例中，对 FreqStack.push 的总调用次数不会超过 10000。
单个测试样例中，对 FreqStack.pop 的总调用次数不会超过 10000。
所有测试样例中，对 FreqStack.push 和 FreqStack.pop 的总调用次数不会超过 150000。

__思路__:

哈希表
建立两个哈希表
一个表对应值出现的次数, 即频率
另一个表对应频率的相应分组, 值用栈
再记录最大的频率, 弹出元素的时候使用
插入的时候记录频率, 插入相应分组, 并更新最大频率
弹出的时候找到频率组对应的元素并弹出, 更新频率, 如果最大频率组的栈为空, 还需要更新最大频率
时间复杂度为 O(2 ^ n), 空间复杂度为 O(2 ^ n)

__代码__:
__C++__:

```C++
class FreqStack 
{
private:
    map<int, int> freq;
    map<int, stack<int>> group;
    int maxfreq;
public:
    FreqStack() 
    {
        maxfreq = 0;
    }
    
    void push(int val) 
    {
        int f = ++freq[val];
        if (f > maxfreq) maxfreq = f;
        group[f].push(val);
    }
    
    int pop() 
    {
        int val = group[maxfreq].top();
        group[maxfreq].pop();
        --freq[val];
        if (group[maxfreq].empty()) --maxfreq;
        return val;
    }
};

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack* obj = new FreqStack();
 * obj->push(val);
 * int param_2 = obj->pop();
 */
```

__Java__:

```Java
class FreqStack {
    private Map<Integer, Integer> freq;
    private Map<Integer, Stack<Integer>> group;
    private int maxfreq;

    public FreqStack() {
        freq = new HashMap();
        group = new HashMap();
        maxfreq = 0;
    }
    
    public void push(int val) {
        int f = freq.getOrDefault(val, 0) + 1;
        freq.put(val, f);
        if (f > maxfreq) maxfreq = f;
        group.computeIfAbsent(f, z -> new Stack()).push(val);
    }
    
    public int pop() {
        int val = group.get(maxfreq).pop();
        freq.put(val, freq.get(val) - 1);
        if (group.get(maxfreq).isEmpty()) --maxfreq;
        return val;
    }
}

/**
 * Your FreqStack object will be instantiated and called as such:
 * FreqStack obj = new FreqStack();
 * obj.push(val);
 * int param_2 = obj.pop();
 */
```

__Python__:

```Python
class FreqStack:

    def __init__(self):
        self.freq = Counter()
        self.group = defaultdict(list)
        self.maxfreq = 0

        
    def push(self, val: int) -> None:
        self.freq[val] = (f := self.freq[val] + 1)
        if f > self.maxfreq:
            self.maxfreq = f
        self.group[f].append(val)
        

    def pop(self) -> int:
        self.freq[(val := self.group[self.maxfreq].pop())] -= 1
        if not self.group[self.maxfreq]:
            self.maxfreq -= 1
        return val


# Your FreqStack object will be instantiated and called as such:
# obj = FreqStack()
# obj.push(val)
# param_2 = obj.pop()
```
