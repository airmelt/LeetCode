# 911 Online Election 在线选举

__Description__:
You are given two integer arrays persons and times. In an election, the ith vote was cast for persons[i] at time times[i].

For each query at a time t, find the person that was leading the election at time t. Votes cast at time t will count towards our query. In the case of a tie, the most recent vote (among tied candidates) wins.

Implement the TopVotedCandidate class:

TopVotedCandidate(int[] persons, int[] times) Initializes the object with the persons and times arrays.
int q(int t) Returns the number of the person that was leading the election at time t according to the mentioned rules.

__Example:__

Example 1:

Input
["TopVotedCandidate", "q", "q", "q", "q", "q", "q"]
[[[0, 1, 1, 0, 0, 1, 0], [0, 5, 10, 15, 20, 25, 30]], [3], [12], [25], [15], [24], [8]]
Output
[null, 0, 1, 1, 0, 0, 1]

Explanation

```Java
TopVotedCandidate topVotedCandidate = new TopVotedCandidate([0, 1, 1, 0, 0, 1, 0], [0, 5, 10, 15, 20, 25, 30]);
topVotedCandidate.q(3); // return 0, At time 3, the votes are [0], and 0 is leading.
topVotedCandidate.q(12); // return 1, At time 12, the votes are [0,1,1], and 1 is leading.
topVotedCandidate.q(25); // return 1, At time 25, the votes are [0,1,1,0,0,1], and 1 is leading (as ties go to the most recent vote.)
topVotedCandidate.q(15); // return 0
topVotedCandidate.q(24); // return 0
topVotedCandidate.q(8); // return 1
```

__Constraints:__

1 <= persons.length <= 5000
times.length == persons.length
0 <= persons[i] < persons.length
0 <= times[i] <= 10^9
times is sorted in a strictly increasing order.
times[0] <= t <= 10^9
At most 10^4 calls will be made to q.

__题目描述__:
在选举中，第 i 张票是在时间为 times[i] 时投给 persons[i] 的。

现在，我们想要实现下面的查询函数： TopVotedCandidate.q(int t) 将返回在 t 时刻主导选举的候选人的编号。

在 t 时刻投出的选票也将被计入我们的查询之中。在平局的情况下，最近获得投票的候选人将会获胜。

__示例 :__

输入：["TopVotedCandidate","q","q","q","q","q","q"], [[[0,1,1,0,0,1,0],[0,5,10,15,20,25,30]],[3],[12],[25],[15],[24],[8]]
输出：[null,0,1,1,0,0,1]
解释：
时间为 3，票数分布情况是 [0]，编号为 0 的候选人领先。
时间为 12，票数分布情况是 [0,1,1]，编号为 1 的候选人领先。
时间为 25，票数分布情况是 [0,1,1,0,0,1]，编号为 1 的候选人领先（因为最近的投票结果是平局）。
在时间 15、24 和 8 处继续执行 3 个查询。

__提示:__

1 <= persons.length = times.length <= 5000
0 <= persons[i] <= persons.length
times 是严格递增的数组，所有元素都在 [0, 10^9] 范围中。
每个测试用例最多调用 10000 次 TopVotedCandidate.q。
TopVotedCandidate.q(int t) 被调用时总是满足 t >= times[0]。

__思路__:

二分查找
预处理所有的时间的获胜者
按照时间使用二分查找找到当前时间的获胜者
C++ 找到 map.upper_bound 的前一个指针
Java 使用 TreeMap.floorEntry()
Python 找到 bisect.bisect_right() 的前一个指针
时间复杂度为 O(n + mlgn), 空间复杂度为 O(n), 预处理需要 O(n) 时间, 每个搜索需要 O(n) 时间, n 为 persons(times) 数组的长度, m 为查询的次数

__代码__:
__C++__:

```C++
class TopVotedCandidate 
{
private:
    map<int, int> m;
public:
    TopVotedCandidate(vector<int>& persons, vector<int>& times) 
    {
        int n = persons.size(), max_value = 0;
        vector<int> tickets(n);
        for (int i = 0; i < n; i++) m[times[i]] = max_value = (++tickets[persons[i]] >= tickets[max_value] ? persons[i] : max_value);
    }
    
    int q(int t) 
    {
        return (--m.upper_bound(t)) -> second;
    }
};

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate* obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj->q(t);
 */
```

__Java__:

```Java
class TopVotedCandidate {
    private TreeMap<Integer, Integer> map;
    
    public TopVotedCandidate(int[] persons, int[] times) {
        map = new TreeMap<>();
        int n = persons.length, maxValue = 0, tickets[] = new int[n];
        for (int i = 0; i < n; i++) map.put(times[i], maxValue = (++tickets[persons[i]] >= tickets[maxValue] ? persons[i] : maxValue));
    }
    
    public int q(int t) {
        return map.floorEntry(t).getValue();
    }
}

/**
 * Your TopVotedCandidate object will be instantiated and called as such:
 * TopVotedCandidate obj = new TopVotedCandidate(persons, times);
 * int param_1 = obj.q(t);
 */
```

__Python__:

```Python
class TopVotedCandidate:

    def __init__(self, persons: List[int], times: List[int]):
        self.times, self.winner, d, max_value, cur_winner = times, [], defaultdict(int), 0, persons[0]
        for person in persons:
            d[person] += 1
            if d[person] >= max_value:
                cur_winner = person
                max_value = d[person]
            self.winner.append(cur_winner)


    def q(self, t: int) -> int:
        return self.winner[bisect_right(self.times, t) - 1]


# Your TopVotedCandidate object will be instantiated and called as such:
# obj = TopVotedCandidate(persons, times)
# param_1 = obj.q(t)
```
