# 2456 Most Popular Video Creator 最流行的视频创作者

__Description:__

You are given two string arrays `creators` and `ids`, and an integer array `views`, all of length `n`. The `i ^ th` video on a platform was created by `creators[i]`, has an id of `ids[i]`, and has `views[i]` views.

The popularity of a creator is the sum of the number of views on all of the creator's videos. Find the creator with the highest popularity and the id of their most viewed video.

- If multiple creators have the highest popularity, find all of them.
- If multiple videos have the highest view count for a creator, find the lexicographically __smallest__ id.

Note: It is possible for different videos to have the same `id`, meaning that `id`s do not uniquely identify a video. For example, two videos with the same ID are considered as distinct videos with their own view count.

Return a __2D array__ of __strings__ `answer` where `answer[i] = [creators_i, idi]` means that `creators_i` has the __highest__ popularity and `idi` is the __id__ of their most __popular__ video. The answer can be returned in any order.

__Example:__

Example 1:

```text
Input: creators = ["alice","bob","alice","chris"], ids = ["one","two","three","four"], views = [5,10,5,4]
```

Output: [["alice","one"],["bob","two"]]

Explanation:

The popularity of alice is 5 + 5 = 10.
The popularity of bob is 10.
The popularity of chris is 4.
alice and bob are the most popular creators.
For bob, the video with the highest view count is "two".
For alice, the videos with the highest view count are "one" and "three". Since "one" is lexicographically smaller than "three", it is included in the answer.

Example 2:

```text
Input: creators = ["alice","alice","alice"], ids = ["a","b","c"], views = [1,2,2]
```

Output: [["alice","b"]]

Explanation:

The videos with id "b" and "c" have the highest view count.
Since "b" is lexicographically smaller than "c", it is included in the answer.

__Constraints:__

- `n == creators.length == ids.length == views.length`
- `1 <= n <= 10 ^ 5`
- `1 <= creators[i].length, ids[i].length <= 5`
- `creators[i]` and `ids[i]` consist only of lowercase English letters.
- `0 <= views[i] <= 10 ^ 5`

__题目描述:__

给你两个字符串数组 `creators` 和 `ids` ，和一个整数数组 `views` ，所有数组的长度都是 `n` 。平台上第 `i` 个视频者是 `creator[i]` ，视频分配的 id 是 `ids[i]` ，且播放量为 `views[i]` 。

视频创作者的 流行度 是该创作者的 所有 视频的播放量的 总和 。请找出流行度 最高 创作者以及该创作者播放量 最大 的视频的 id 。

- 如果存在多个创作者流行度都最高，则需要找出所有符合条件的创作者。
- 如果某个创作者存在多个播放量最高的视频，则只需要找出字典序最小的 `id` 。

返回一个二维字符串数组 `answer` ，其中 `answer[i] = [creator_i, idi]` 表示 `creator_i` 的流行度 __最高__ 且其最流行的视频 id 是 `idi` ，可以按任何顺序返回该结果。

__示例:__

示例 1：

```text
输入：creators = ["alice","bob","alice","chris"], ids = ["one","two","three","four"], views = [5,10,5,4]
输出：[["alice","one"],["bob","two"]]
解释：
alice 的流行度是 5 + 5 = 10 。
bob 的流行度是 10 。
chris 的流行度是 4 。
alice 和 bob 是流行度最高的创作者。
bob 播放量最高的视频 id 为 "two" 。
alice 播放量最高的视频 id 是 "one" 和 "three" 。由于 "one" 的字典序比 "three" 更小，所以结果中返回的 id 是 "one" 。
```

示例 2：

```text
输入：creators = ["alice","alice","alice"], ids = ["a","b","c"], views = [1,2,2]
输出：[["alice","b"]]
解释：
id 为 "b" 和 "c" 的视频都满足播放量最高的条件。
由于 "b" 的字典序比 "c" 更小，所以结果中返回的 id 是 "b" 。
```

__提示：__

- `n == creators.length == ids.length == views.length`
- `1 <= n <= 10 ^ 5`
- `1 <= creators[i].length, ids[i].length <= 5`
- `creators[i]` 和 `ids[i]` 仅由小写英文字母组成
- `0 <= views[i] <= 10 ^ 5`

__思路:__

```text
哈希表
用一个哈希表 c 记录每个创作者的流行度
用一个哈希表 m 记录每个创作者的播放量最高的视频
用一个哈希表 p 记录每个创作者的播放量最高的视频的 id
遍历 creators, 和 views
将 creators[i] 和 views[i] 加入 c
记录播放量最大的创作者
将 creators[i] 和 views[i] 加入 m
遍历 ids, 如果 creators[i] 的流行度等于最大的流行度且 views[i] 等于 m[creators[i]] 则将字典序最小的 ids[i] 加入 p
最后将 p 中的元素加入结果
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<vector<string>> mostPopularCreator(vector<string>& creators, vector<string>& ids, vector<int>& views) 
    {
        unordered_map<string, long long> c;
        unordered_map<string, int> m;
        unordered_map<string, string> p;
        long long mx = 0LL;
        for (int i = 0, n = ids.size(); i < n; i++) 
        {
            c[creators[i]] += views[i];
            mx = max(mx, c[creators[i]]);
            m[creators[i]] = max(m[creators[i]], views[i]);
        }
        for (int i = 0, n = ids.size(); i < n; i++) if (c[creators[i]] == mx and views[i] == m[creators[i]]) if (!p.count(creators[i]) or ids[i].compare(p[creators[i]]) < 0) p[creators[i]] = ids[i];
        vector<vector<string>> result;
        for (const auto& [k, v] : p) result.emplace_back(vector<string>{k, v});
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<List<String>> mostPopularCreator(String[] creators, String[] ids, int[] views) {
        var c = new HashMap<String, Long>();
        var m = new HashMap<String, Integer>();
        var p = new HashMap<String, String>();
        long mx = 0L;
        for (int i = 0, n = ids.length; i < n; i++) {
            c.merge(creators[i], (long)views[i], Long::sum);
            mx = Math.max(mx, c.get(creators[i]));
            m.put(creators[i], Math.max(m.getOrDefault(creators[i], 0), views[i]));
        }
        for (int i = 0, n = ids.length; i < n; i++) if (c.get(creators[i]) == mx && views[i] == m.get(creators[i])) if (!p.containsKey(creators[i]) || ids[i].compareTo(p.get(creators[i])) < 0) p.put(creators[i], ids[i]);
        return p.entrySet().stream().map(e -> List.of(e.getKey(), e.getValue())).collect(Collectors.toList());
    }
}
```

__Python__:

```Python
class Solution:
    def mostPopularCreator(self, creators: List[str], ids: List[str], views: List[int]) -> List[List[str]]:
        c, m, mx = defaultdict(int), defaultdict(lambda: (1, '')), 0
        for name, tag, v in zip(creators, ids, views):
            c[name] += v
            mx, m[name] = max(mx, c[name]), min(m[name], (-v, tag))
        return [[name, tag] for name, (_, tag) in m.items() if c[name] == mx]
```
