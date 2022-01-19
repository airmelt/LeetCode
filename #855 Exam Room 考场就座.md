# 855 Exam Room 考场就座

__Description__:
There is an exam room with n seats in a single row labeled from 0 to n - 1.

When a student enters the room, they must sit in the seat that maximizes the distance to the closest person. If there are multiple such seats, they sit in the seat with the lowest number. If no one is in the room, then the student sits at seat number 0.

Design a class that simulates the mentioned exam room.

Implement the ExamRoom class:

ExamRoom(int n) Initializes the object of the exam room with the number of the seats n.
int seat() Returns the label of the seat at which the next student will set.
void leave(int p) Indicates that the student sitting at seat p will leave the room. It is guaranteed that there will be a student sitting at seat p.

__Example:__

Example 1:

Input
["ExamRoom", "seat", "seat", "seat", "seat", "leave", "seat"]
[[10], [], [], [], [], [4], []]
Output
[null, 0, 9, 4, 2, null, 5]

Explanation

```Java
ExamRoom examRoom = new ExamRoom(10);
examRoom.seat(); // return 0, no one is in the room, then the student sits at seat number 0.
examRoom.seat(); // return 9, the student sits at the last seat number 9.
examRoom.seat(); // return 4, the student sits at the last seat number 4.
examRoom.seat(); // return 2, the student sits at the last seat number 2.
examRoom.leave(4);
examRoom.seat(); // return 5, the student sits at the last seat number 5.
```

__Constraints:__

1 <= n <= 10^9
It is guaranteed that there is a student sitting at seat p.
At most 10^4 calls will be made to seat and leave.

__题目描述__:
在考场里，一排有 N 个座位，分别编号为 0, 1, 2, ..., N-1 。

当学生进入考场后，他必须坐在能够使他与离他最近的人之间的距离达到最大化的座位上。如果有多个这样的座位，他会坐在编号最小的座位上。(另外，如果考场里没有人，那么学生就坐在 0 号座位上。)

返回 ExamRoom(int N) 类，它有两个公开的函数：其中，函数 ExamRoom.seat() 会返回一个 int （整型数据），代表学生坐的位置；函数 ExamRoom.leave(int p) 代表坐在座位 p 上的学生现在离开了考场。每次调用 ExamRoom.leave(p) 时都保证有学生坐在座位 p 上。

__示例 :__

输入：["ExamRoom","seat","seat","seat","seat","leave","seat"], [[10],[],[],[],[],[4],[]]
输出：[null,0,9,4,2,null,5]
解释：
ExamRoom(10) -> null
seat() -> 0，没有人在考场里，那么学生坐在 0 号座位上。
seat() -> 9，学生最后坐在 9 号座位上。
seat() -> 4，学生最后坐在 4 号座位上。
seat() -> 2，学生最后坐在 2 号座位上。
leave(4) -> null
seat() -> 5，学生最后坐在 5 号座位上。

__提示:__

1 <= N <= 10^9
在所有的测试样例中 ExamRoom.seat() 和 ExamRoom.leave() 最多被调用 10^4 次。
保证在调用 ExamRoom.leave(p) 时有学生正坐在座位 p 上。

__思路__:

有序列表
使用 set(C++), TreeSet(Java), sortedcontainers.SortedList(Python) 记录已经坐上去的学生
当记录为空时, 插入 0 并返回
否则, 遍历有序列表中的元素, 查找中点与区间端点距离的最大值并插入有序列表
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class ExamRoom 
{
private:
    set<int> seats;
    int size;
public:
    ExamRoom(int n)
    {
        size = n;
    }
    
    int seat() 
    {
        if (seats.empty())
        {
            seats.insert(0);
            return 0;
        }
        else
        {
            int prev = -1, dist = *seats.begin(), seat = 0;
            for (const auto& s : seats)
            {
                if (prev != -1)
                {
                    int d = ((s - prev) >> 1);
                    if (d > dist)
                    {
                        dist = d;
                        seat = prev + d;
                    }
                }
                prev = s;
            }
            if (size - 1 - *seats.rbegin() > dist) seat = size - 1;
            seats.insert(seat);
            return seat;
        }
    }
    
    void leave(int p) 
    {
        seats.erase(p);
    }
};

/**
 * Your ExamRoom object will be instantiated and called as such:
 * ExamRoom* obj = new ExamRoom(n);
 * int param_1 = obj->seat();
 * obj->leave(p);
 */
```

__Java__:

```Java
class ExamRoom {
    private TreeSet<Integer> seats;
    private int size;

    public ExamRoom(int n) {
        seats = new TreeSet<>();
        size = n;
    }
    
    public int seat() {
        if (seats.isEmpty()) {
            seats.add(0);
            return 0;
        } else {
            int dist = seats.first(), seat = 0, prev = -1;
            for (int s : seats) {
                if (prev != -1) {
                    int d = ((s - prev) >>> 1);
                    if (d > dist) {
                        dist = d;
                        seat = prev + d;
                    }
                }
                prev = s;
            }
            if (size - 1 - seats.last() > dist) seat = size - 1;
            seats.add(seat);
            return seat;
        }
    }
    
    public void leave(int p) {
        seats.remove(p);
    }
}

/**
 * Your ExamRoom object will be instantiated and called as such:
 * ExamRoom obj = new ExamRoom(n);
 * int param_1 = obj.seat();
 * obj.leave(p);
 */
```

__Python__:

```Python
from sortedcontainers import SortedList
class ExamRoom:

    def __init__(self, n: int):
        self.seats = SortedList()
        self.n = n


    def seat(self) -> int:
        if not self.seats:
            self.seats.add(0)
            return 0
        else:
            dist, seat = self.seats[0], 0
            for i, s in enumerate(self.seats):
                if i and (d := ((s - (prev := self.seats[i - 1])) >> 1)) > dist:
                    dist, seat = d, prev + d
            self.seats.add((seat := self.n - 1 if (self.n - 1 - self.seats[-1]) > dist else seat))
            return seat        


    def leave(self, p: int) -> None:
        self.seats.remove(p)
        


# Your ExamRoom object will be instantiated and called as such:
# obj = ExamRoom(n)
# param_1 = obj.seat()
# obj.leave(p)
```
