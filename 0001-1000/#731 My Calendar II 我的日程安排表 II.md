# 731 My Calendar II 我的日程安排表 II

__Description__:
You are implementing a program to use as your calendar. We can add a new event if adding the event will not cause a triple booking.

A triple booking happens when three events have some non-empty intersection (i.e., some moment is common to all the three events.).

The event can be represented as a pair of integers start and end that represents a booking on the half-open interval [start, end), the range of real numbers x such that start <= x < end.

Implement the MyCalendarTwo class:

MyCalendarTwo() Initializes the calendar object.
boolean book(int start, int end) Returns true if the event can be added to the calendar successfully without causing a triple booking. Otherwise, return false and do not add the event to the calendar.

__Example:__

Example 1:

Input
["MyCalendarTwo", "book", "book", "book", "book", "book", "book"]
[[], [10, 20], [50, 60], [10, 40], [5, 15], [5, 10], [25, 55]]
Output
[null, true, true, true, false, true, true]

Explanation

```Java
MyCalendarTwo myCalendarTwo = new MyCalendarTwo();
myCalendarTwo.book(10, 20); // return True, The event can be booked. 
myCalendarTwo.book(50, 60); // return True, The event can be booked. 
myCalendarTwo.book(10, 40); // return True, The event can be double booked. 
myCalendarTwo.book(5, 15);  // return False, The event ca not be booked, because it would result in a triple booking.
myCalendarTwo.book(5, 10); // return True, The event can be booked, as it does not use time 10 which is already double booked.
myCalendarTwo.book(25, 55); // return True, The event can be booked, as the time in [25, 40) will be double booked with the third event, the time [40, 50) will be single booked, and the time [50, 55) will be double booked with the second event.
```

__Constraints:__

0 <= start < end <= 10^9
At most 1000 calls will be made to book.

__题目描述__:
实现一个 MyCalendar 类来存放你的日程安排。如果要添加的时间内不会导致三重预订时，则可以存储这个新的日程安排。

MyCalendar 有一个 book(int start, int end)方法。它意味着在 start 到 end 时间内增加一个日程安排，注意，这里的时间是半开区间，即 [start, end), 实数 x 的范围为，  start <= x < end。

当三个日程安排有一些时间上的交叉时（例如三个日程安排都在同一时间内），就会产生三重预订。

每次调用 MyCalendar.book方法时，如果可以将日程安排成功添加到日历中而不会导致三重预订，返回 true。否则，返回 false 并且不要将该日程安排添加到日历中。

请按照以下步骤调用MyCalendar 类: MyCalendar cal = new MyCalendar(); MyCalendar.book(start, end)

__示例 :__

```Java
MyCalendar();
MyCalendar.book(10, 20); // returns true
MyCalendar.book(50, 60); // returns true
MyCalendar.book(10, 40); // returns true
MyCalendar.book(5, 15); // returns false
MyCalendar.book(5, 10); // returns true
MyCalendar.book(25, 55); // returns true
```

解释：
前两个日程安排可以添加至日历中。 第三个日程安排会导致双重预订，但可以添加至日历中。
第四个日程安排活动（5,15）不能添加至日历中，因为它会导致三重预订。
第五个日程安排（5,10）可以添加至日历中，因为它未使用已经双重预订的时间10。
第六个日程安排（25,55）可以添加至日历中，因为时间 [25,40] 将和第三个日程安排双重预订；
时间 [40,50] 将单独预订，时间 [50,55）将和第二个日程安排双重预订。

__提示:__

每个测试用例，调用 MyCalendar.book 函数最多不超过 1000次。
调用函数 MyCalendar.book(start, end)时， start 和 end 的取值范围为 [0, 10^9]。

__思路__:

1. 有序字典
使用 C++ 中的 map 或者 Java 中的 TreeMap实现
记录开始和结束的时间(边界值)
开始时间 + 1, 结束时间 - 1
遍历整个字典累计 count 值
只要 count 值大于 2, 将开始和结束的时间复原, 返回 false
否则返回 true
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)
2. 集合
用一个集合 first 记录有一次日程的安排
用另一个集合 second 记录有两次的日程安排
加入新的日程安排时先遍历 second 检查是否有日程交叉, 只要有交叉立即返回 false
然后将 second 中的日程与新日程及 first 中的日程进行合并区间即可
时间复杂度为 O(n ^ 2), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class MyCalendarTwo 
{
private:
    map<int, int> m;
public:
    MyCalendarTwo() {}
    
    bool book(int start, int end) 
    {
        ++m[start];
        --m[end];
        int count = 0;
        for (auto& [k, v] : m)
        {
            count += v;
            if (count > 2)
            {
                --m[start];
                ++m[end];
                return false;
            }
        }
        return true;
    }
};

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo* obj = new MyCalendarTwo();
 * bool param_1 = obj->book(start,end);
 */
```

__Java__:

```Java
class MyCalendarTwo {
    private TreeMap<Integer, Integer> data;

    public MyCalendarTwo() {
        data = new TreeMap<>();
    }
    
    public boolean book(int start, int end) {
        data.put(start, data.getOrDefault(start, 0) + 1);
        data.put(end, data.getOrDefault(end, 0) - 1);
        int count = 0;
        for (int day : data.values()) {
            count += day;
            if (count > 2) {
                data.put(start, data.get(start) - 1);
                data.put(end, data.get(end) + 1);
                if (data.get(start) == 0) data.remove(start);
                return false;
            }
        }
        return true;
    }
}

/**
 * Your MyCalendarTwo object will be instantiated and called as such:
 * MyCalendarTwo obj = new MyCalendarTwo();
 * boolean param_1 = obj.book(start,end);
 */
```

__Python__:

```Python
class MyCalendarTwo:

    def __init__(self):
        self.first = set()
        self.second = set()
        

    def book(self, start: int, end: int) -> bool:
        if any(end > day[0] and start < day[1] for day in self.second):
            return False
        self.second |= set((max(start, day[0]), min(end, day[1])) for day in self.first if end > day[0] and start < day[1])
        self.first.add((start, end))
        return True


# Your MyCalendarTwo object will be instantiated and called as such:
# obj = MyCalendarTwo()
# param_1 = obj.book(start,end)
```
