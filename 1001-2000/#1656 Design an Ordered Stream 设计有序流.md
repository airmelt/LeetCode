# 1656 Design an Ordered Stream 设计有序流

__Description:__

There is a stream of `n` `(idKey, value)` pairs arriving in an __arbitrary__ order, where `idKey` is an integer between `1` and `n` and `value` is a string. No two pairs have the same `id`.

Design a stream that returns the values in increasing order of their IDs by returning a chunk (list) of values after each insertion. The concatenation of all the chunks should result in a list of the sorted values.

Implement the `OrderedStream` class:

- `OrderedStream(int n)` Constructs the stream to take `n` values.
- `String[] insert(int idKey, String value)` Inserts the pair `(idKey, value)` into the stream, then returns the __largest possible chunk__ of currently inserted values that appear next in the order.

__Example:__

Example:

![1656-1](https://assets.leetcode.com/uploads/2020/11/10/q1.gif)

```text
Input
["OrderedStream", "insert", "insert", "insert", "insert", "insert"]
[[5], [3, "ccccc"], [1, "aaaaa"], [2, "bbbbb"], [5, "eeeee"], [4, "ddddd"]]
Output
[null, [], ["aaaaa"], ["bbbbb", "ccccc"], [], ["ddddd", "eeeee"]]

Explanation
```

```Java
// Note that the values ordered by ID is ["aaaaa", "bbbbb", "ccccc", "ddddd", "eeeee"].
OrderedStream os = new OrderedStream(5);
os.insert(3, "ccccc"); // Inserts (3, "ccccc"), returns [].
os.insert(1, "aaaaa"); // Inserts (1, "aaaaa"), returns ["aaaaa"].
os.insert(2, "bbbbb"); // Inserts (2, "bbbbb"), returns ["bbbbb", "ccccc"].
os.insert(5, "eeeee"); // Inserts (5, "eeeee"), returns [].
os.insert(4, "ddddd"); // Inserts (4, "ddddd"), returns ["ddddd", "eeeee"].
// Concatentating all the chunks returned:
// [] + ["aaaaa"] + ["bbbbb", "ccccc"] + [] + ["ddddd", "eeeee"] = ["aaaaa", "bbbbb", "ccccc", "ddddd", "eeeee"]
// The resulting order is the same as the order above.
```

__Constraints:__

- `1 <= n <= 1000`
- `1 <= id <= n`
- `value.length == 5`
- `value` consists only of lowercase letters.
- Each call to `insert` will have a unique `id.`
- Exactly `n` calls will be made to `insert`.

__题目描述:__

有 `n` 个 `(id, value)` 对，其中 `id` 是 `1` 到 `n` 之间的一个整数， `value` 是一个字符串。不存在 `id` 相同的两个 `(id, value)` 对。

设计一个流，以 __任意__ 顺序获取 `n` 个 `(id, value)` 对，并在多次调用时 __按 `id` 递增的顺序__ 返回一些值。

实现 `OrderedStream` 类:

- `OrderedStream(int n)` 构造一个能接收 `n` 个值的流，并将当前指针 `ptr` 设为 `1` 。
- `String[] insert(int id, String value)` 向流中存储新的 `(id, value)` 对。存储后:
  - 如果流存储有 `id = ptr` 的 `(id, value)` 对，则找出从 `id = ptr` 开始的 __最长 id 连续递增序列__ ，并 __按顺序__ 返回与这些 id 关联的值的列表。然后，将 `ptr` 更新为最后那个  `id + 1` 。
  - 否则，返回一个空列表。
- 如果流存储有 `id = ptr` 的 `(id, value)` 对，则找出从 `id = ptr` 开始的 __最长 id 连续递增序列__ ，并 __按顺序__ 返回与这些 id 关联的值的列表。然后，将 `ptr` 更新为最后那个  `id + 1` 。
- 否则，返回一个空列表。

- 如果流存储有 `id = ptr` 的 `(id, value)` 对，则找出从 `id = ptr` 开始的 __最长 id 连续递增序列__ ，并 __按顺序__ 返回与这些 id 关联的值的列表。然后，将 `ptr` 更新为最后那个  `id + 1` 。
- 否则，返回一个空列表。

否则，返回一个空列表。

__示例:__

示例：

![1656-2](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/15/q1.gif)

```text
输入
["OrderedStream", "insert", "insert", "insert", "insert", "insert"]
[[5], [3, "ccccc"], [1, "aaaaa"], [2, "bbbbb"], [5, "eeeee"], [4, "ddddd"]]
输出
[null, [], ["aaaaa"], ["bbbbb", "ccccc"], [], ["ddddd", "eeeee"]]

解释
```

```Java
OrderedStream os= new OrderedStream(5);
os.insert(3, "ccccc"); // 插入 (3, "ccccc")，返回 []
os.insert(1, "aaaaa"); // 插入 (1, "aaaaa")，返回 ["aaaaa"]
os.insert(2, "bbbbb"); // 插入 (2, "bbbbb")，返回 ["bbbbb", "ccccc"]
os.insert(5, "eeeee"); // 插入 (5, "eeeee")，返回 []
os.insert(4, "ddddd"); // 插入 (4, "ddddd")，返回 ["ddddd", "eeeee"]
```

__提示：__

- `1 <= n <= 1000`
- `1 <= id <= n`
- `value.length == 5`
- `value` 仅由小写字母组成
- 每次调用 `insert` 都会使用一个唯一的 `id`
- 恰好调用 `n` 次 `insert`

__思路:__

```text
模拟
题目的意思是, 插入一个字符串到某个位置, 如果下一个位置有字符串, 则将所有连续的字符串返回, 并将指针更新到最后一个字符串的下一个位置, 否则返回空字符串数组, 不更新指针位置
可以用一个哈希表或者长度为 N + 1 的列表存放所有的字符串
每次插入的时候遍历哈希表或者列表，找到连续的字符串，直到找到空字符串或者哈希表的值不存在为止
将找到的所有字符串加入结果, 同时更新指针
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class OrderedStream 
{
public:
    OrderedStream(int n) {}
    
    vector<string> insert(int id, string value) 
    {
        m[id] = value;
        vector<string> result;
        while (m.count(ptr)) result.push_back(m[ptr++]);
        return result;
    }
private:
    unordered_map<int, string> m;
    int ptr = 1;
};

/**
 * Your OrderedStream object will be instantiated and called as such:
 * OrderedStream* obj = new OrderedStream(n);
 * vector<string> param_1 = obj->insert(idKey,value);
 */
```

__Java__:

```Java
class OrderedStream {
    private String[] data;
    private int ptr = 0;
    private int n;

    public OrderedStream(int n) {
        this.n = n;
        data = new String[n + 1];
    }
    
    public List<String> insert(int idKey, String value) {
        data[idKey - 1] = value;
        List<String> result = new ArrayList<>();
        while (data[ptr] != null) result.add(data[ptr++]);
        return result;
    }
}

/**
 * Your OrderedStream object will be instantiated and called as such:
 * OrderedStream obj = new OrderedStream(n);
 * List<String> param_1 = obj.insert(idKey,value);
 */
```

__Python__:

```Python
class OrderedStream:

    def __init__(self, n: int):
        self.data = [None] * (n + 1)
        self.ptr = 0


    def insert(self, idKey: int, value: str) -> List[str]:
        self.data[idKey - 1] = value
        result = deque()
        while self.data[self.ptr]:
            result.append(self.data[self.ptr])
            self.ptr += 1
        return list(result)



# Your OrderedStream object will be instantiated and called as such:
# obj = OrderedStream(n)
# param_1 = obj.insert(idKey,value)
```
