# 2671 Frequency Tracker 频率跟踪器

__Description:__

Design a data structure that keeps track of the values in it and answers some queries regarding their frequencies.

Implement the `FrequencyTracker` class.

- `FrequencyTracker()`: Initializes the `FrequencyTracker` object with an empty array initially.
- `void add(int number)`: Adds `number` to the data structure.
- `void deleteOne(int number)`: Deletes __one__ occurrence of `number` from the data structure. The data structure __may not contain__ `number`, and in this case nothing is deleted.
- `bool hasFrequency(int frequency)`: Returns `true` if there is a number in the data structure that occurs `frequency` number of times, otherwise, it returns `false`.

__Example:__

Example 1:

```text
Input
["FrequencyTracker", "add", "add", "hasFrequency"]
[[], [3], [3], [2]]
Output
[null, null, null, true]

Explanation
```

```Java
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.add(3); // The data structure now contains [3]
frequencyTracker.add(3); // The data structure now contains [3, 3]
frequencyTracker.hasFrequency(2); // Returns true, because 3 occurs twice
```

Example 2:

```text
Input
["FrequencyTracker", "add", "deleteOne", "hasFrequency"]
[[], [1], [1], [1]]
Output
[null, null, null, false]

Explanation
```

```Java
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.add(1); // The data structure now contains [1]
frequencyTracker.deleteOne(1); // The data structure becomes empty []
frequencyTracker.hasFrequency(1); // Returns false, because the data structure is empty
```

Example 3:

```text
Input
["FrequencyTracker", "hasFrequency", "add", "hasFrequency"]
[[], [2], [3], [1]]
Output
[null, false, null, true]

Explanation
```

```Java
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.hasFrequency(2); // Returns false, because the data structure is empty
frequencyTracker.add(3); // The data structure now contains [3]
frequencyTracker.hasFrequency(1); // Returns true, because 3 occurs once
```

__Constraints:__

- `1 <= number <= 10 ^ 5`
- `1 <= frequency <= 10 ^ 5`
- At most, `2 * 10 ^ 5` calls will be made to `add`, `deleteOne`, and `hasFrequency` in __total__.

__题目描述:__

请你设计并实现一个能够对其中的值进行跟踪的数据结构，并支持对频率相关查询进行应答。

实现 `FrequencyTracker` 类:

- `FrequencyTracker()`:使用一个空数组初始化 `FrequencyTracker` 对象。
- `void add(int number)`:添加一个 `number` 到数据结构中。
- `void deleteOne(int number)`:从数据结构中删除一个 `number` 。数据结构 __可能不包含__ `number` ，在这种情况下不删除任何内容。
- `bool hasFrequency(int frequency)`: 如果数据结构中存在出现 `frequency` 次的数字，则返回 `true`，否则返回 `false`。

__示例:__

示例 1：

```text
输入
["FrequencyTracker", "add", "add", "hasFrequency"]
[[], [3], [3], [2]]
输出
[null, null, null, true]

解释
```

```Java
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.add(3); // 数据结构现在包含 [3]
frequencyTracker.add(3); // 数据结构现在包含 [3, 3]
frequencyTracker.hasFrequency(2); // 返回 true ，因为 3 出现 2 次
```

示例 2：

```text
输入
["FrequencyTracker", "add", "deleteOne", "hasFrequency"]
[[], [1], [1], [1]]
输出
[null, null, null, false]

解释
```

```Java
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.add(1); // 数据结构现在包含 [1]
frequencyTracker.deleteOne(1); // 数据结构现在为空 []
frequencyTracker.hasFrequency(1); // 返回 false ，因为数据结构为空
```

示例 3：

```text
输入
["FrequencyTracker", "hasFrequency", "add", "hasFrequency"]
[[], [2], [3], [1]]
输出
[null, false, null, true]

解释
```

```Java
FrequencyTracker frequencyTracker = new FrequencyTracker();
frequencyTracker.hasFrequency(2); // 返回 false ，因为数据结构为空
frequencyTracker.add(3); // 数据结构现在包含 [3]
frequencyTracker.hasFrequency(1); // 返回 true ，因为 3 出现 1 次
```

__提示：__

- `1 <= number <= 10 ^ 5`
- `1 <= frequency <= 10 ^ 5`
- 最多调用 `add`、 `deleteOne` 和 `hasFrequency` __共计__ `2 * 10 ^ 5` 次

__思路:__

```text
哈希表
使用两个哈希表
一个哈希表记录每个数字出现的次数
另一个哈希表记录出现次数的频率
每次添加的时候先减少频率, 再增加出现次数, 最后增加对应数字的频率
减少的时候把增加出现次数改成减少
判断频率直接判断是否存在即可
所有函数的时间复杂度为 O(1), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class FrequencyTracker 
{
public:
    FrequencyTracker() {}
    
    void add(int number) 
    {
        --freq[data[number]];
        ++data[number];
        ++freq[data[number]];
    }
    
    void deleteOne(int number) 
    {
        if (data[number])
        {
            --freq[data[number]];
            --data[number];
            ++freq[data[number]];
        }
    }
    
    bool hasFrequency(int frequency) 
    {
        return freq[frequency] > 0;
    }
private:
    unordered_map<int, int> data, freq;
};

/**
 * Your FrequencyTracker object will be instantiated and called as such:
 * FrequencyTracker* obj = new FrequencyTracker();
 * obj->add(number);
 * obj->deleteOne(number);
 * bool param_3 = obj->hasFrequency(frequency);
 */
```

__Java__:

```Java
class FrequencyTracker {
    private Map<Integer, Integer> data = new HashMap<>();
    private Map<Integer, Integer> freq = new HashMap<>();

    public FrequencyTracker() {}
    
    public void add(int number) {
        freq.merge(data.getOrDefault(number, 0), -1, Integer::sum);
        data.merge(number, 1, Integer::sum);
        freq.merge(data.getOrDefault(number, 0), 1, Integer::sum);
    }
    
    public void deleteOne(int number) {
        if (data.getOrDefault(number, 0) > 0) {
            freq.merge(data.getOrDefault(number, 0), -1, Integer::sum);
            data.merge(number, -1, Integer::sum);
            freq.merge(data.getOrDefault(number, 0), 1, Integer::sum);
        }
    }
    
    public boolean hasFrequency(int frequency) {
        return freq.getOrDefault(frequency, 0) > 0;
    }
}

/**
 * Your FrequencyTracker object will be instantiated and called as such:
 * FrequencyTracker obj = new FrequencyTracker();
 * obj.add(number);
 * obj.deleteOne(number);
 * boolean param_3 = obj.hasFrequency(frequency);
 */
```

__Python__:

```Python
class FrequencyTracker:

    def __init__(self):
        self.data = Counter()
        self.freq = Counter()


    def add(self, number: int) -> None:
        self.freq[self.data[number]] -= 1
        self.data[number] += 1
        self.freq[self.data[number]] += 1


    def deleteOne(self, number: int) -> None:
        if self.data[number]:
            self.freq[self.data[number]] -= 1
            self.data[number] -= 1
            self.freq[self.data[number]] += 1


    def hasFrequency(self, frequency: int) -> bool:
        return self.freq[frequency] > 0



# Your FrequencyTracker object will be instantiated and called as such:
# obj = FrequencyTracker()
# obj.add(number)
# obj.deleteOne(number)
# param_3 = obj.hasFrequency(frequency)
```
