# 2034 Stock Price Fluctuation 股票价格波动

__Description:__

You are given a stream of records about a particular stock. Each record contains a timestamp and the corresponding price of the stock at that timestamp.

Unfortunately due to the volatile nature of the stock market, the records do not come in order. Even worse, some records may be incorrect. Another record with the same timestamp may appear later in the stream correcting the price of the previous wrong record.

Design an algorithm that:

- __Updates__ the price of the stock at a particular timestamp, __correcting__ the price from any previous records at the timestamp.
- Finds the __latest price__ of the stock based on the current records. The __latest price__ is the price at the latest timestamp recorded.
- Finds the __maximum price__ the stock has been based on the current records.
- Finds the __minimum price__ the stock has been based on the current records.

Implement the `StockPrice` class:

- `StockPrice()` Initializes the object with no price records.
- `void update(int timestamp, int price)` Updates the `price` of the stock at the given `timestamp`.
- `int current()` Returns the __latest price__ of the stock.
- `int maximum()` Returns the __maximum price__ of the stock.
- `int minimum()` Returns the __minimum price__ of the stock.

__Example:__

Example 1:

```text
Input
["StockPrice", "update", "update", "current", "maximum", "update", "maximum", "update", "minimum"]
[[], [1, 10], [2, 5], [], [], [1, 3], [], [4, 2], []]
Output
[null, null, null, 5, 10, null, 5, null, 2]

Explanation
```

```Java
StockPrice stockPrice = new StockPrice();
stockPrice.update(1, 10); // Timestamps are [1] with corresponding prices [10].
stockPrice.update(2, 5);  // Timestamps are [1,2] with corresponding prices [10,5].
stockPrice.current();     // return 5, the latest timestamp is 2 with the price being 5.
stockPrice.maximum();     // return 10, the maximum price is 10 at timestamp 1.
stockPrice.update(1, 3);  // The previous timestamp 1 had the wrong price, so it is updated to 3.
                          // Timestamps are [1,2] with corresponding prices [3,5].
stockPrice.maximum();     // return 5, the maximum price is 5 after the correction.
stockPrice.update(4, 2);  // Timestamps are [1,2,4] with corresponding prices [3,5,2].
stockPrice.minimum();     // return 2, the minimum price is 2 at timestamp 4.
```

__Constraints:__

- `1 <= timestamp, price <= 10 ^ 9`
- At most `10 ^ 5` calls will be made __in total__ to `update`, `current`, `maximum`, and `minimum`.
- `current`, `maximum`, and `minimum` will be called __only after__ `update` has been called __at least once__.

__题目描述:__

给你一支股票价格的数据流。数据流中每一条记录包含一个 时间戳 和该时间点股票对应的 价格 。

不巧的是，由于股票市场内在的波动性，股票价格记录可能不是按时间顺序到来的。某些情况下，有的记录可能是错的。如果两个有相同时间戳的记录出现在数据流中，前一条记录视为错误记录，后出现的记录 更正 前一条错误的记录。

请你设计一个算法，实现：

- __更新__ 股票在某一时间戳的股票价格，如果有之前同一时间戳的价格，这一操作将 __更正__ 之前的错误价格。
- 找到当前记录里 _最新股票价格_ 。__最新股票价格__ 定义为时间戳最晚的股票价格。
- 找到当前记录里股票的 __最高价格__ 。
- 找到当前记录里股票的 __最低价格__ 。

请你实现 `StockPrice` 类:

- `StockPrice()` 初始化对象，当前无股票价格记录。
- `void update(int timestamp, int price)` 在时间点 `timestamp` 更新股票价格为 `price` 。
- `int current()` 返回股票 __最新价格__ 。
- `int maximum()` 返回股票 __最高价格__ 。
- `int minimum()` 返回股票 __最低价格__ 。

__示例:__

示例 1：

```text
输入：
["StockPrice", "update", "update", "current", "maximum", "update", "maximum", "update", "minimum"]
[[], [1, 10], [2, 5], [], [], [1, 3], [], [4, 2], []]
输出：
[null, null, null, 5, 10, null, 5, null, 2]

解释：
```

```Java
StockPrice stockPrice = new StockPrice();
stockPrice.update(1, 10); // 时间戳为 [1] ，对应的股票价格为 [10] 。
stockPrice.update(2, 5);  // 时间戳为 [1,2] ，对应的股票价格为 [10,5] 。
stockPrice.current();     // 返回 5 ，最新时间戳为 2 ，对应价格为 5 。
stockPrice.maximum();     // 返回 10 ，最高价格的时间戳为 1 ，价格为 10 。
stockPrice.update(1, 3);  // 之前时间戳为 1 的价格错误，价格更新为 3 。
                          // 时间戳为 [1,2] ，对应股票价格为 [3,5] 。
stockPrice.maximum();     // 返回 5 ，更正后最高价格为 5 。
stockPrice.update(4, 2);  // 时间戳为 [1,2,4] ，对应价格为 [3,5,2] 。
stockPrice.minimum();     // 返回 2 ，最低价格时间戳为 4 ，价格为 2 。
```

__提示：__

- `1 <= timestamp, price <= 10 ^ 9`
- `update`， `current`， `maximum` 和 `minimum` __总__ 调用次数不超过 `10 ^ 5` 。
- `current`， `maximum` 和 `minimum` 被调用时， `update` 操作 __至少__ 已经被调用过 __一次__ 。

__思路:__

```text
有序哈希表
使用哈希表记录时间戳和价格的对应关系
后面的记录会覆盖之前的记录
当前价格使用哈希表记录的最新时间戳对应的价格
有序哈希表的最大值和最小值即为最大价格和最小价格
更新时删除之前的价格，插入新的价格
初始化时间复杂度为 O(1), update() 时间复杂度为 O(logN), current() 时间复杂度为 O(1), maximum() 时间复杂度为 O(logN), minimum() 时间复杂度为 O(logN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class StockPrice 
{
public:
    StockPrice() 
    {
        this -> cur = 0;
    }
    
    void update(int timestamp, int price) 
    {
        cur = max(cur, timestamp);
        int pre = data.count(timestamp) ? data[timestamp] : 0;
        data[timestamp] = price;
        if (pre > 0) 
        {
            auto it = prices.find(pre);
            if (it != prices.end()) prices.erase(it);
        }
        prices.emplace(price);
    }
    
    int current() 
    {
        return data[cur];
    }
    
    int maximum() 
    {
        return *prices.rbegin();
    }
    
    int minimum() 
    {
        return *prices.begin();
    }
private:
    int cur;
    unordered_map<int, int> data;
    multiset<int> prices;
};

/**
 * Your StockPrice object will be instantiated and called as such:
 * StockPrice* obj = new StockPrice();
 * obj->update(timestamp,price);
 * int param_2 = obj->current();
 * int param_3 = obj->maximum();
 * int param_4 = obj->minimum();
 */
```

__Java__:

```Java
class StockPrice {
    int cur;
    HashMap<Integer, Integer> data;
    TreeMap<Integer, Integer> prices;

    public StockPrice() {
        cur = 0;
        data = new HashMap<Integer, Integer>();
        prices = new TreeMap<Integer, Integer>();
    }
    
    public void update(int timestamp, int price) {
        cur = Math.max(cur, timestamp);
        int pre = data.getOrDefault(timestamp, 0);
        data.put(timestamp, price);
        if (pre > 0) {
            prices.put(pre, prices.get(pre) - 1);
            if (prices.get(pre) == 0) prices.remove(pre);
        }
        prices.put(price, prices.getOrDefault(price, 0) + 1);
    }
    
    public int current() {
        return data.get(cur);
    }
    
    public int maximum() {
        return prices.lastKey();
    }
    
    public int minimum() {
        return prices.firstKey();
    }
}

/**
 * Your StockPrice object will be instantiated and called as such:
 * StockPrice obj = new StockPrice();
 * obj.update(timestamp,price);
 * int param_2 = obj.current();
 * int param_3 = obj.maximum();
 * int param_4 = obj.minimum();
 */
```

__Python__:

```Python
from sortedcontainers import SortedList, SortedDict
class StockPrice:

    def __init__(self):
        self.data = defaultdict(int)
        self.prices = SortedList()
        self.timestamp = 0


    def update(self, timestamp: int, price: int) -> None:
        self.timestamp = max(timestamp, self.timestamp)
        if timestamp in self.data:
            del self.prices[self.prices.index(self.data[timestamp])]
        self.data[timestamp] = price
        self.prices.add(price)

    def current(self) -> int:
        return self.data[self.timestamp]
    

    def maximum(self) -> int:
        return self.prices[-1]
    

    def minimum(self) -> int:
        return self.prices[0]


# Your StockPrice object will be instantiated and called as such:
# obj = StockPrice()
# obj.update(timestamp,price)
# param_2 = obj.current()
# param_3 = obj.maximum()
# param_4 = obj.minimum()
```
