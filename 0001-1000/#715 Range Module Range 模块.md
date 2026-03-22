# 715 Range Module Range 模块

__Description__:
A Range Module is a module that tracks ranges of numbers. Design a data structure to track the ranges represented as half-open intervals and query about them.

A half-open interval [left, right) denotes all the real numbers x where left <= x < right.

Implement the RangeModule class:

RangeModule() Initializes the object of the data structure.
void addRange(int left, int right) Adds the half-open interval [left, right), tracking every real number in that interval. Adding an interval that partially overlaps with currently tracked numbers should add any numbers in the interval [left, right) that are not already tracked.
boolean queryRange(int left, int right) Returns true if every real number in the interval [left, right) is currently being tracked, and false otherwise.
void removeRange(int left, int right) Stops tracking every real number currently being tracked in the half-open interval [left, right).

__Example:__

Example 1:

Input
["RangeModule", "addRange", "removeRange", "queryRange", "queryRange", "queryRange"]
[[], [10, 20], [14, 16], [10, 14], [13, 15], [16, 17]]
Output
[null, null, null, true, false, true]

Explanation

```Java
RangeModule rangeModule = new RangeModule();
rangeModule.addRange(10, 20);
rangeModule.removeRange(14, 16);
rangeModule.queryRange(10, 14); // return True,(Every number in [10, 14) is being tracked)
rangeModule.queryRange(13, 15); // return False,(Numbers like 14, 14.03, 14.17 in [13, 15) are not being tracked)
rangeModule.queryRange(16, 17); // return True, (The number 16 in [16, 17) is still being tracked, despite the remove operation)
```

__Constraints:__

1 <= left < right <= 10^9
At most 10^4 calls will be made to addRange, queryRange, and removeRange.

__题目描述__:
Range 模块是跟踪数字范围的模块。你的任务是以一种有效的方式设计和实现以下接口。

addRange(int left, int right) 添加半开区间 [left, right)，跟踪该区间中的每个实数。添加与当前跟踪的数字部分重叠的区间时，应当添加在区间 [left, right) 中尚未跟踪的任何数字到该区间中。
queryRange(int left, int right) 只有在当前正在跟踪区间 [left, right) 中的每一个实数时，才返回 true。
removeRange(int left, int right) 停止跟踪区间 [left, right) 中当前正在跟踪的每个实数。

__示例 :__

addRange(10, 20): null
removeRange(14, 16): null
queryRange(10, 14): true （区间 [10, 14) 中的每个数都正在被跟踪）
queryRange(13, 15): false （未跟踪区间 [13, 15) 中像 14, 14.03, 14.17 这样的数字）
queryRange(16, 17): true （尽管执行了删除操作，区间 [16, 17) 中的数字 16 仍然会被跟踪）

__提示:__

半开区间 [left, right) 表示所有满足 left <= x < right 的实数。
对 addRange, queryRange, removeRange 的所有调用中 0 < left < right < 10^9。
在单个测试用例中，对 addRange 的调用总数不超过 1000 次。
在单个测试用例中，对  queryRange 的调用总数不超过 5000 次。
在单个测试用例中，对 removeRange 的调用总数不超过 1000 次。

__思路__:

利用二分法或者 TreeMap 加快查询
区间加入时需要合并重合的区间, 删除时可能需要增加新区间
addRange() 时间复杂度为 O(n), queryRange() 时间复杂度为 O(lgn), removeRange() 时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class RangeModule 
{
private:
    set<pair<int, int>> s;
public:
    RangeModule() 
    {

    }
    
    void addRange(int left, int right) 
    {
        if (s.empty())
        {
            s.insert({ left, right });
            return;
        }
        else
        {
            auto it = s.lower_bound({ left, left });
            if (it != s.begin()) --it;
            while (it != s.end() and it -> first <= right)
            {
                if (it -> second < left)
                {
                    ++it;
                    continue;
                }
                left = min(left, it -> first);
                right = max(right, it -> second);
                s.erase(it++);
            }
            s.insert({ left, right });
        }
    }
    
    bool queryRange(int left, int right) 
    {
        auto it = s.lower_bound({ left, left });
        if (it -> first <= left and it -> second >= right) return true;
        if (it != s.begin()) --it;
        if (it -> first <= left and it -> second >= right) return true;
        return false;
    }
    
    void removeRange(int left, int right) 
    {
        if (s.empty()) return;
        auto it = s.lower_bound({ left, left });
        if (it != s.begin()) --it;
        while (it != s.end() and it -> first < right)
            {
                if (it -> second <= left)
                {
                    ++it;
                    continue;
                }
                if (it -> first < left) s.insert({ it -> first, left });
                if (it -> second > right) s.insert({ right, it -> second });
                s.erase(it++);
            }
    }
};

/**
 * Your RangeModule object will be instantiated and called as such:
 * RangeModule* obj = new RangeModule();
 * obj->addRange(left,right);
 * bool param_2 = obj->queryRange(left,right);
 * obj->removeRange(left,right);
 */
```

__Java__:

```Java
class RangeModule {
    
    private TreeMap<Integer, Integer> tree;

    public RangeModule() {
        tree = new TreeMap<Integer, Integer>();
    }
    
    public void addRange(int left, int right) {
        while (true) {
            Map.Entry<Integer, Integer> entry = tree.floorEntry(right);
            if (entry != null && entry.getValue() >= left) {
                right = Math.max(entry.getValue(), right);
                left = Math.min(entry.getKey(), left);
                tree.remove(entry.getKey());
            } else break;
        }
        tree.put(left, right);
    }
    
    public boolean queryRange(int left, int right) {
        return tree.floorEntry(left) != null && tree.floorEntry(left).getValue() >= right;
    }
    
    public void removeRange(int left, int right) {
        Map.Entry<Integer, Integer> entry = tree.lowerEntry(left);
        if (entry != null && entry.getValue() > left) {
            tree.put(entry.getKey(), left);
            if (entry.getValue() > right) {
                tree.put(right, entry.getValue());
                return;
            }
        }
        while (true) {
            entry = tree.ceilingEntry(left);
            if (entry != null && entry.getKey() < right) {
                tree.remove(entry.getKey());
                if (entry.getValue() > right) {
                    tree.put(right, entry.getValue());
                    return;
                }
            } else break;
        }
    }
}

/**
 * Your RangeModule object will be instantiated and called as such:
 * RangeModule obj = new RangeModule();
 * obj.addRange(left,right);
 * boolean param_2 = obj.queryRange(left,right);
 * obj.removeRange(left,right);
 */
```

__Python__:

```Python
class RangeModule:

    def __init__(self):
        self.data = []


    def addRange(self, left: int, right: int) -> None:
        self.data.append([left, right])
        self.data.sort()
        i = 1
        while i < len(self.data):
            (left1, right1), (left2, right2) = self.data[i - 1], self.data[i]
            if right1 >= right2:
                del self.data[i]
            elif right1 >= left2:
                self.data[i - 1][1] = right2
                del self.data[i]
            else:
                i += 1
                

    def queryRange(self, left: int, right: int) -> bool:
        return any(l <= left and right <= r for l, r in self.data)


    def removeRange(self, left: int, right: int) -> None:
        for i in range(len(self.data)):
            if right <= self.data[i][0]:
                break
            if left <= self.data[i][1]:
                if right <= self.data[i][1]:
                    self.data.append([right, self.data[i][1]])
                    self.data[i][1] = left
                    self.data.sort()
                    break
                else:
                    left, self.data[i][1] = self.data[i][1], left


# Your RangeModule object will be instantiated and called as such:
# obj = RangeModule()
# obj.addRange(left,right)
# param_2 = obj.queryRange(left,right)
# obj.removeRange(left,right)
```
