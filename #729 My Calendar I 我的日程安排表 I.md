# 729 My Calendar I 我的日程安排表 I

__Description__:
You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a double booking.

A double booking happens when two events have some non-empty intersection (i.e., some moment is common to both events.).

The event can be represented as a pair of integers start and end that represents a booking on the half-open interval [start, end), the range of real numbers x such that start <= x < end.

Implement the MyCalendar class:

MyCalendar() Initializes the calendar object.
boolean book(int start, int end) Returns true if the event can be added to the calendar successfully without causing a double booking. Otherwise, return false and do not add the event to the calendar.

__Example:__

Example 1:

Input
["MyCalendar", "book", "book", "book"]
[[], [10, 20], [15, 25], [20, 30]]
Output
[null, true, false, true]

Explanation

```Java
MyCalendar myCalendar = new MyCalendar();
myCalendar.book(10, 20); // return True
myCalendar.book(15, 25); // return False, It can not be booked because time 15 is already booked by another event.
myCalendar.book(20, 30); // return True, The event can be booked, as the first event takes every time less than 20, but not including 20.
```

__Constraints:__

0 <= start < end <= 10^9
At most 1000 calls will be made to book.

__题目描述__:
实现一个 MyCalendar 类来存放你的日程安排。如果要添加的时间内没有其他安排，则可以存储这个新的日程安排。

MyCalendar 有一个 book(int start, int end)方法。它意味着在 start 到 end 时间内增加一个日程安排，注意，这里的时间是半开区间，即 [start, end), 实数 x 的范围为，  start <= x < end。

当两个日程安排有一些时间上的交叉时（例如两个日程安排都在同一时间内），就会产生重复预订。

每次调用 MyCalendar.book方法时，如果可以将日程安排成功添加到日历中而不会导致重复预订，返回 true。否则，返回 false 并且不要将该日程安排添加到日历中。

请按照以下步骤调用 MyCalendar 类: MyCalendar cal = new MyCalendar(); MyCalendar.book(start, end)

__示例 :__

示例 1:

```Java
MyCalendar();
MyCalendar.book(10, 20); // returns true
MyCalendar.book(15, 25); // returns false
MyCalendar.book(20, 30); // returns true
```

解释:
第一个日程安排可以添加到日历中.  第二个日程安排不能添加到日历中，因为时间 15 已经被第一个日程安排预定了。
第三个日程安排可以添加到日历中，因为第一个日程安排并不包含时间 20 。

__说明:__

每个测试用例，调用 MyCalendar.book 函数最多不超过 1000次。
调用函数 MyCalendar.book(start, end)时， start 和 end 的取值范围为 [0, 10^9]。

__思路__:

1. 有序列表
因为 start 和 end 的取值范围为 [0, 10^9]
可以设置两个哨兵 (-2, -1), (10 \** 9 + 1, 10 ** 9 + 2) 保证 start 和 end 在这个区间内
每次使用二分查找查找插入的位置
判断区间的位置及进行合并操作
时间复杂度为 O(nlgn), 空间复杂度为 O(n)
2. 平衡二叉树
用 C++ 中的 map 保证插入有序
每次使用二分查找查找插入的位置并判断区间的位置
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class MyCalendar 
{
private:
    map<int, int> data;
public:
    MyCalendar() {}
    
    bool book(int start, int end)
    {
        auto it = data.lower_bound(end);
        if (it != data.begin() and (--it) -> second > start) return false;
        data[start] = end;
        return true;
    }
};

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar* obj = new MyCalendar();
 * bool param_1 = obj->book(start,end);
 */
```

__Java__:

```Java
class MyCalendar {

    private List<int[]> data;
    public MyCalendar() {
        data = new ArrayList<int[]>();
        data.add(new int[]{ -2, -1 });
        data.add(new int[]{ 1000000001, 1000000002 });
    }
    
    public boolean book(int start, int end) {
        int l = 0, r = data.size() - 1;
        while (l < r) {
            int mid = l + ((r - l) >> 1);
            if (start > data.get(mid)[0]) l = mid + 1;
            else r = mid;
        }
        if (start >= data.get(l - 1)[1] && end <= data.get(l)[0]) {
            data.add(l, new int[]{ start, end });
            return true;
        }
        return false;
    }
}

/**
 * Your MyCalendar object will be instantiated and called as such:
 * MyCalendar obj = new MyCalendar();
 * boolean param_1 = obj.book(start,end);
 */
```

__Python__:

```Python
class MyCalendar:

    def __init__(self):
        self.data = [(-2, -1), (10 ** 9 + 1, 10 ** 9 + 2)]

        
    def book(self, start: int, end: int) -> bool:
        l, r = 0, len(self.data) - 1
        while l < r:
            mid = l + ((r - l) >> 1)
            if start > self.data[mid][0]:
                l = mid + 1
            else:
                r = mid
        if start >= self.data[l - 1][1] and end <= self.data[l][0]:
            self.data.insert(l, (start, end))
            return True
        return False


# Your MyCalendar object will be instantiated and called as such:
# obj = MyCalendar()
# param_1 = obj.book(start,end)
```
