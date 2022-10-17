# 1348 Tweet Counts Per Frequency 推文计数

__Description:__

A social media company is trying to monitor activity on their site by analyzing the number of tweets that occur in select periods of time. These periods can be partitioned into smaller time chunks based on a certain frequency (every minute, hour, or day).

For example, the period [10, 10000] (in seconds) would be partitioned into the following time chunks with these frequencies:

Every minute (60-second chunks): [10,69], [70,129], [130,189], ..., [9970,10000]
Every hour (3600-second chunks): [10,3609], [3610,7209], [7210,10000]
Every day (86400-second chunks): [10,10000]
Notice that the last chunk may be shorter than the specified frequency's chunk size and will always end with the end time of the period (10000 in the above example).

Design and implement an API to help the company with their analysis.

Implement the TweetCounts class:

TweetCounts() Initializes the TweetCounts object.
void recordTweet(String tweetName, int time) Stores the tweetName at the recorded time (in seconds).
List\<Integer> getTweetCountsPerFrequency(String freq, String tweetName, int startTime, int endTime) Returns a list of integers representing the number of tweets with tweetName in each time chunk for the given period of time [startTime, endTime] (in seconds) and frequency freq.
freq is one of "minute", "hour", or "day" representing a frequency of every minute, hour, or day respectively.

__Example:__

Input
["TweetCounts","recordTweet","recordTweet","recordTweet","getTweetCountsPerFrequency","getTweetCountsPerFrequency","recordTweet","getTweetCountsPerFrequency"]
[[],["tweet3",0],["tweet3",60],["tweet3",10],["minute","tweet3",0,59],["minute","tweet3",0,60],["tweet3",120],["hour","tweet3",0,210]]

Output
[null,null,null,null,[2],[2,1],null,[4]]

Explanation

```Java
TweetCounts tweetCounts = new TweetCounts();
tweetCounts.recordTweet("tweet3", 0);                              // New tweet "tweet3" at time 0
tweetCounts.recordTweet("tweet3", 60);                             // New tweet "tweet3" at time 60
tweetCounts.recordTweet("tweet3", 10);                             // New tweet "tweet3" at time 10
tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 59); // return [2]; chunk [0,59] had 2 tweets
tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 60); // return [2,1]; chunk [0,59] had 2 tweets, chunk [60,60] had 1 tweet
tweetCounts.recordTweet("tweet3", 120);                            // New tweet "tweet3" at time 120
tweetCounts.getTweetCountsPerFrequency("hour", "tweet3", 0, 210);  // return [4]; chunk [0,210] had 4 tweets
```

__Constraints:__

0 <= time, startTime, endTime <= 10^9
0 <= endTime - startTime <= 10^4
There will be at most 10^4 calls in total to recordTweet and getTweetCountsPerFrequency.

__题目描述：__

一家社交媒体公司正试图通过分析特定时间段内出现的推文数量来监控其网站上的活动。这些时间段可以根据特定的频率（ 每分钟 、每小时 或 每一天 ）划分为更小的 时间段 。

例如，周期 [10,10000] （以 秒 为单位）将被划分为以下频率的 时间块 :

每 分钟 (60秒 块)： [10,69], [70,129], [130,189], ..., [9970,10000]
每 小时 (3600秒 块)：[10,3609], [3610,7209], [7210,10000]
每 天 (86400秒 块)： [10,10000]
注意，最后一个块可能比指定频率的块大小更短，并且总是以时间段的结束时间结束(在上面的示例中为 10000 )。

设计和实现一个API来帮助公司进行分析。

实现 TweetCounts 类:

TweetCounts() 初始化 TweetCounts 对象。
存储记录时间的 tweetName (以秒为单位)。
List\<integer> getTweetCountsPerFrequency(String freq, String tweetName, int startTime, int endTime) 返回一个整数列表，表示给定时间 [startTime, endTime] （单位秒）和频率频率中，每个 时间块 中带有 tweetName 的 tweet 的数量。
freq 是 “minute” 、 “hour” 或 “day” 中的一个，分别表示 每分钟 、 每小时 或 每一天 的频率。

__示例：__

输入：
["TweetCounts","recordTweet","recordTweet","recordTweet","getTweetCountsPerFrequency","getTweetCountsPerFrequency","recordTweet","getTweetCountsPerFrequency"]
[[],["tweet3",0],["tweet3",60],["tweet3",10],["minute","tweet3",0,59],["minute","tweet3",0,60],["tweet3",120],["hour","tweet3",0,210]]

输出：
[null,null,null,null,[2],[2,1],null,[4]]

解释：

```Java
TweetCounts tweetCounts = new TweetCounts();
tweetCounts.recordTweet("tweet3", 0);
tweetCounts.recordTweet("tweet3", 60);
tweetCounts.recordTweet("tweet3", 10);                             // "tweet3" 发布推文的时间分别是 0, 10 和 60 。
tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 59); // 返回 [2]。统计频率是每分钟（60 秒），因此只有一个有效时间间隔 [0,60> - > 2 条推文。
tweetCounts.getTweetCountsPerFrequency("minute", "tweet3", 0, 60); // 返回 [2,1]。统计频率是每分钟（60 秒），因此有两个有效时间间隔 1) [0,60> - > 2 条推文，和 2) [60,61> - > 1 条推文。 
tweetCounts.recordTweet("tweet3", 120);                            // "tweet3" 发布推文的时间分别是 0, 10, 60 和 120 。
tweetCounts.getTweetCountsPerFrequency("hour", "tweet3", 0, 210);  // 返回 [4]。统计频率是每小时（3600 秒），因此只有一个有效时间间隔 [0,211> - > 4 条推文。
```

__提示：__

0 <= time, startTime, endTime <= 10^9
0 <= endTime - startTime <= 10^4
recordTweet 和 getTweetCountsPerFrequency，最多有 10^4 次操作。

__思路：__

1. 模拟
将推文按照用户名放在哈希表中
查询时遍历用户名
时间复杂度为 O(q(t + n)), 空间复杂度为 O(n + t), 其中 n 表示推文数量, t 表示查询的时间范围, q 表示查询的次数
2. 使用红黑树优化
将推文按照用户名放入哈希表, 哈希表的值为红黑树
查询时使用二分查找加快查找速度
时间复杂度为 O(q(t + lgn) + nlgn), 空间复杂度为 O(n + t), 其中 n 表示推文数量, t 表示查询的时间范围, q 表示查询的次数

__代码__:

__C++__:

```C++
class TweetCounts 
{
private:
    unordered_map<string, multiset<int>> records;
    unordered_map<string, int> m;
public:
    TweetCounts() 
    {
        m["minute"] = 60;
        m["hour"] = 3600;
        m["day"] = 86400;
    }
    
    void recordTweet(string tweetName, int time) 
    {
        records[tweetName].emplace(time);
    }
    
    vector<int> getTweetCountsPerFrequency(string freq, string tweetName, int startTime, int endTime) 
    {
        vector<int> result; 
        int step = m[freq];
        while (startTime <= endTime)
        {
            auto it1 = records[tweetName].lower_bound(startTime), it2 = records[tweetName].upper_bound(min(startTime + step - 1, endTime));
            result.emplace_back(distance(it1, it2));
            startTime += step;
        }
        return result;
    }
};

/**
 * Your TweetCounts object will be instantiated and called as such:
 * TweetCounts* obj = new TweetCounts();
 * obj->recordTweet(tweetName,time);
 * vector<int> param_2 = obj->getTweetCountsPerFrequency(freq,tweetName,startTime,endTime);
 */
```

__Java__:

```Java
class TweetCounts {
    private Map<String, TreeMap<Integer, Integer>> records;
    private Map<String, Integer> map;

    public TweetCounts() {
        records = new HashMap<>();
        map = new HashMap<>() {{ put("minute", 60); put("hour", 3600); put("day", 86400); }};
    }
    
    public void recordTweet(String tweetName, int time) {
        TreeMap<Integer, Integer> treeMap = records.computeIfAbsent(tweetName, v -> new TreeMap<>());
        treeMap.put(time, treeMap.getOrDefault(time, 0) + 1);
    }
    
    public List<Integer> getTweetCountsPerFrequency(String freq, String tweetName, int startTime, int endTime) {
        List<Integer> result = new ArrayList<>();
        int step = map.get(freq);
        TreeMap<Integer, Integer> treeMap = records.get(tweetName);
        for (int time = startTime; time <= endTime; time += step) {
            int end = Math.min(time + step, endTime + 1), count = 0;
            Map.Entry<Integer, Integer> entry = treeMap.ceilingEntry(time);
            while (entry != null && entry.getKey() < end) {
                count += entry.getValue();
                entry = treeMap.higherEntry(entry.getKey());
            }
            result.add(count);
        }
        return result;
    }
}

/**
 * Your TweetCounts object will be instantiated and called as such:
 * TweetCounts obj = new TweetCounts();
 * obj.recordTweet(tweetName,time);
 * List<Integer> param_2 = obj.getTweetCountsPerFrequency(freq,tweetName,startTime,endTime);
 */
```

__Python__:

```Python
class TweetCounts:

    def __init__(self):
        self.records = defaultdict(list)


    def recordTweet(self, tweetName: str, time: int) -> None:
        self.records[tweetName].append(time)


    def getTweetCountsPerFrequency(self, freq: str, tweetName: str, startTime: int, endTime: int) -> List[int]:
        result = [0] * ((endTime - startTime) // (step := 60 if freq == 'minute' else 3600 if freq == 'hour' else 86400) + 1)
        for x in self.records[tweetName]:
            if startTime <= x <= endTime:
                result[(x - startTime) // step] += 1
        return result



# Your TweetCounts object will be instantiated and called as such:
# obj = TweetCounts()
# obj.recordTweet(tweetName,time)
# param_2 = obj.getTweetCountsPerFrequency(freq,tweetName,startTime,endTime)
```
