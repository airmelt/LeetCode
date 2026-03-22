# 352 Data Stream as Disjoint Intervals 将数据流变为多个不相交区间

__Description__:
Given a data stream input of non-negative integers a1, a2, ..., an, ..., summarize the numbers seen so far as a list of disjoint intervals.

__Example:__

For example, suppose the integers from the data stream are 1, 3, 7, 2, 6, ..., then the summary will be:

```text
[1, 1]
[1, 1], [3, 3]
[1, 1], [3, 3], [7, 7]
[1, 3], [7, 7]
[1, 3], [6, 7]
```

__Follow up:__

What if there are lots of merges and the number of disjoint intervals are small compared to the data stream's size?

__题目描述__:
给定一个非负整数的数据流输入 a1，a2，…，an，…，将到目前为止看到的数字总结为不相交的区间列表。

__示例 :__
例如，假设数据流中的整数为 1，3，7，2，6，…，每次的总结为：

```text
[1, 1]
[1, 1], [3, 3]
[1, 1], [3, 3], [7, 7]
[1, 3], [7, 7]
[1, 3], [6, 7]
```

__进阶：__
如果有很多合并，并且与数据流的大小相比，不相交区间的数量很小，该怎么办?

__提示：__
特别感谢 @yunhong 提供了本问题和其测试用例。

__思路__:

1. 利用二叉搜索树(set)
插入的时候直接插入到 set
需要区间时合并区间输出
addNum()时间复杂度O(lgn), getIntervals()时间复杂度O(n), 空间复杂度O(n)
2. 插入时用堆或者红黑树直接处理成有序的区间
可以利用二分加快查找速度
输出区间可以直接输出结果
addNum()时间复杂度O(n), getIntervals()时间复杂度O(1), 空间复杂度O(n)

__代码__:
__C++__:

```C++
class SummaryRanges 
{
public:
    /** Initialize your data structure here. */
    SummaryRanges() {}
    
    void addNum(int val) 
    {
        dp.insert(val);
    }
    
    vector<vector<int>> getIntervals() 
    {
        vector<vector<int>> result;
        if (dp.empty()) return result;
        int last = *dp.begin(), begin = *dp.begin();
        for (auto it : dp)
        {
            if (it == last or it - 1 == last) last = it;
            else
            {
                result.push_back({begin, last});
                begin = last = it;
            }
        }
        result.push_back({begin, *dp.rbegin()});
        return result;
    }
private:
    set<int> dp;
};

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges* obj = new SummaryRanges();
 * obj->addNum(val);
 * vector<vector<int>> param_2 = obj->getIntervals();
 */
```

__Java__:

```Java
public class SummaryRanges {
    
    private TreeMap<Integer, int[]> map;

    /**
     * Initialize your data structure here.
     */
    public SummaryRanges() {
        map = new TreeMap<>();
    }

    public void addNum(int val) {
        if (map.isEmpty()) {
            map.put(val, new int[]{val, val});
            return;
        }
        Integer floorKey = map.floorKey(val);
        if (floorKey == null) {
            int[] firstInterval = map.firstEntry().getValue();
            if (map.firstKey() == val + 1) {
                map.remove(map.firstKey());
                map.put(val, new int[]{val, firstInterval[1]});
            } else {
                map.put(val, new int[]{val, val});
            }
        } else {
            int[] floorInterval = map.get(floorKey);
            if (floorInterval[1] < val) {
                if (floorInterval[1] == val - 1) {
                    map.put(floorKey, new int[]{floorKey, val});
                } else if (floorInterval[1] < val - 1) {
                    map.put(val, new int[]{val, val});
                }
                floorKey = map.floorKey(val);
                if (map.containsKey(val + 1)) {
                    map.put(floorKey, new int[]{
                            map.get(floorKey)[0],
                            map.get(val + 1)[1],
                    });
                    map.remove(val + 1);
                }
            }
        }
    }

    public int[][] getIntervals() {
        int len = map.size();
        int[][] result = new int[len][2];
        int i = 0;
        for (int[] interval : map.values()) {
            result[i++] = interval;
        }
        return result;
    }
}

/**
 * Your SummaryRanges object will be instantiated and called as such:
 * SummaryRanges obj = new SummaryRanges();
 * obj.addNum(val);
 * int[][] param_2 = obj.getIntervals();
 */
```

__Python__:

```Python
class SummaryRanges:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.min_heap = []
        

    def addNum(self, val: int) -> None:
        heapq.heappush(self.min_heap, [val, val])
        

    def getIntervals(self):
        merged = []
        while self.min_heap:
            x = heapq.heappop(self.min_heap)
            if not merged or merged[-1][1] + 1 < x[0]:
                merged.append(x)
            else:
                merged[-1][1] = max(merged[-1][1], x[1])
        self.min_heap = merged
        return self.min_heap
        


# Your SummaryRanges object will be instantiated and called as such:
# obj = SummaryRanges()
# obj.addNum(val)
# param_2 = obj.getIntervals()
```
