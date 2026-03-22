# 901 Online Stock Span 股票价格跨度

__Description__:
Design an algorithm that collects daily price quotes for some stock and returns the span of that stock's price for the current day.

The span of the stock's price today is defined as the maximum number of consecutive days (starting from today and going backward) for which the stock price was less than or equal to today's price.

For example, if the price of a stock over the next 7 days were [100,80,60,70,60,75,85], then the stock spans would be [1,1,1,2,1,4,6].
Implement the StockSpanner class:

StockSpanner() Initializes the object of the class.
int next(int price) Returns the span of the stock's price given that today's price is price.

__Example:__

Example 1:

Input
["StockSpanner", "next", "next", "next", "next", "next", "next", "next"]
[[], [100], [80], [60], [70], [60], [75], [85]]
Output
[null, 1, 1, 1, 2, 1, 4, 6]

Explanation

```Java
StockSpanner stockSpanner = new StockSpanner();
stockSpanner.next(100); // return 1
stockSpanner.next(80);  // return 1
stockSpanner.next(60);  // return 1
stockSpanner.next(70);  // return 2
stockSpanner.next(60);  // return 1
stockSpanner.next(75);  // return 4, because the last 4 prices (including today's price of 75) were less than or equal to today's price.
stockSpanner.next(85);  // return 6
```

__Constraints:__

1 <= price <= 10^5
At most 10^4 calls will be made to next.

__题目描述__:
编写一个 StockSpanner 类，它收集某些股票的每日报价，并返回该股票当日价格的跨度。

今天股票价格的跨度被定义为股票价格小于或等于今天价格的最大连续日数（从今天开始往回数，包括今天）。

例如，如果未来7天股票的价格是 [100, 80, 60, 70, 60, 75, 85]，那么股票跨度将是 [1, 1, 1, 2, 1, 4, 6]。

__示例 :__

输入：["StockSpanner","next","next","next","next","next","next","next"], [[],[100],[80],[60],[70],[60],[75],[85]]
输出：[null,1,1,1,2,1,4,6]
解释：
首先，初始化 S = StockSpanner()，然后：
S.next(100) 被调用并返回 1，
S.next(80) 被调用并返回 1，
S.next(60) 被调用并返回 1，
S.next(70) 被调用并返回 2，
S.next(60) 被调用并返回 1，
S.next(75) 被调用并返回 4，
S.next(85) 被调用并返回 6。

注意 (例如) S.next(75) 返回 4，因为截至今天的最后 4 个价格
(包括今天的价格 75) 小于或等于今天的价格。

__提示:__

调用 StockSpanner.next(int price) 时，将有 1 <= price <= 10^5。
每个测试用例最多可以调用  10000 次 StockSpanner.next。
在所有测试用例中，最多调用 150000 次 StockSpanner.next。
此问题的总时间限制减少了 50%。

__思路__:

模拟
记录下标的位置
如果下标指向的元素个数小于 n, 则移动下标, 并更新 n
否则减少下标指向的元素个数, 并将 n 置零
返回时比较下标和 encoding 的长度返回 -1 或对应位置的元素
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class StockSpanner 
{
private:
    stack<pair<int, int>> s;
public:
    StockSpanner() {}
    
    int next(int price) 
    {
        int result = 1;
        if (s.empty() or price < s.top().first)
        {
            s.push({ price, result });
            return result;
        }
        while (!s.empty() and price >= s.top().first)
        {
            result += s.top().second;
            s.pop();
        }
        s.push({ price, result });
        return result;
    }
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```

__Java__:

```Java
class StockSpanner {
    private Stack<Integer> prices, days;

    public StockSpanner() {
        prices = new Stack<>();
        days = new Stack<>();
    }
    
    public int next(int price) {
        int result = 1;
        if (prices.isEmpty() || price < prices.peek()) {
            prices.push(price);
            days.push(result);
            return result;
        }
        while (!prices.isEmpty() && price >= prices.peek()) {
            result += days.pop();
            prices.pop();
        }
        prices.push(price);
        days.push(result);
        return result;
    }
}

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner obj = new StockSpanner();
 * int param_1 = obj.next(price);
 */
```

__Python__:

```Python
class StockSpanner:

    def __init__(self):
        self.stack = []


    def next(self, price: int) -> int:
        result = 1
        if not self.stack or price < self.stack[-1][0]:
            self.stack.append((price, result))
            return result
        while self.stack and price >= self.stack[-1][0]:
            result += self.stack.pop()[1]
        self.stack.append((price, result))
        return result
        


# Your StockSpanner object will be instantiated and called as such:
# obj = StockSpanner()
# param_1 = obj.next(price)
```
