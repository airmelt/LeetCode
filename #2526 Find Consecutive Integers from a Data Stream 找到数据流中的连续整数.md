# 2526 Find Consecutive Integers from a Data Stream 找到数据流中的连续整数

__Description:__

For a stream of integers, implement a data structure that checks if the last `k` integers parsed in the stream are __equal__ to `value`.

Implement the DataStream class:

- `DataStream(int value, int k)` Initializes the object with an empty integer stream and the two integers `value` and `k`.
- `boolean consec(int num)` Adds `num` to the stream of integers. Returns `true` if the last `k` integers are equal to `value`, and `false` otherwise. If there are less than `k` integers, the condition does not hold true, so returns `false`.

__Example:__

Example 1:

```text
Input
["DataStream", "consec", "consec", "consec", "consec"]
[[4, 3], [4], [4], [4], [3]]
Output
[null, false, false, true, false]

Explanation
```

```Java
DataStream dataStream = new DataStream(4, 3); //value = 4, k = 3 
dataStream.consec(4); // Only 1 integer is parsed, so returns False. 
dataStream.consec(4); // Only 2 integers are parsed.
                      // Since 2 is less than k, returns False. 
dataStream.consec(4); // The 3 integers parsed are all equal to value, so returns True. 
dataStream.consec(3); // The last k integers parsed in the stream are [4,4,3].
                      // Since 3 is not equal to value, it returns False.
```

__Constraints:__

- `1 <= value, num <= 10 ^ 9`
- `1 <= k <= 10 ^ 5`
- At most `10 ^ 5` calls will be made to `consec`.

__题目描述:__

给你一个整数数据流，请你实现一个数据结构，检查数据流中最后 `k` 个整数是否 __等于__ 给定值 `value` 。

请你实现 DataStream 类：

- `DataStream(int value, int k)` 用两个整数 `value` 和 `k` 初始化一个空的整数数据流。
- `boolean consec(int num)` 将 `num` 添加到整数数据流。如果后 `k` 个整数都等于 `value` ，返回 `true` ，否则返回 `false` 。如果少于 `k` 个整数，条件不满足，所以也返回 `false` 。

__示例:__

示例 1：

```text
输入：
["DataStream", "consec", "consec", "consec", "consec"]
[[4, 3], [4], [4], [4], [3]]
输出：
[null, false, false, true, false]

解释：
```

```Java
DataStream dataStream = new DataStream(4, 3); // value = 4, k = 3 
dataStream.consec(4); // 数据流中只有 1 个整数，所以返回 False 。
dataStream.consec(4); // 数据流中只有 2 个整数
                      // 由于 2 小于 k ，返回 False 。
dataStream.consec(4); // 数据流最后 3 个整数都等于 value， 所以返回 True 。
dataStream.consec(3); // 最后 k 个整数分别是 [4,4,3] 。
                      // 由于 3 不等于 value ，返回 False 。
```

__提示：__

- `1 <= value, num <= 10 ^ 9`
- `1 <= k <= 10 ^ 5`
- 至多调用 `consec` 次数为 `10 ^ 5` 次。

__思路:__

```text
模拟
只需要记录当前连续的数字个数即可
如果当前数字不等于 value, count 置为 0
如果当前数字等于 value, count + 1
如果 count >= k, 返回 true
否则返回 false
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class DataStream 
{
public:
    DataStream(int _value, int _k): value(_value), k(_k) {}
    
    bool consec(int num) 
    {
        count = num != value ? 0 : count + 1;
        return count >= k;
    }
private:
    int value, k, count = 0;
};

/**
 * Your DataStream object will be instantiated and called as such:
 * DataStream* obj = new DataStream(value, k);
 * bool param_1 = obj->consec(num);
 */
```

__Java__:

```Java
class DataStream {
    private int value, k, count = 0;

    public DataStream(int value, int k) {
        this.value = value;
        this.k = k;
    }
    
    public boolean consec(int num) {
        count = num != value ? 0 : count + 1;
        return count >= k;
    }
}

/**
 * Your DataStream object will be instantiated and called as such:
 * DataStream obj = new DataStream(value, k);
 * boolean param_1 = obj.consec(num);
 */
```

__Python__:

```Python
class DataStream:

    def __init__(self, value: int, k: int):
        self.count = 0
        self.k = k
        self.value = value
        

    def consec(self, num: int) -> bool:
        self.count = 0 if num != self.value else self.count + 1
        return self.count >= self.k
        


# Your DataStream object will be instantiated and called as such:
# obj = DataStream(value, k)
# param_1 = obj.consec(num)
```
