# 692 Top K Frequent Words 前K个高频单词

__Description__:
Given a non-empty list of words, return the k most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

__Example:__

Example 1:
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
Explanation: "i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.

Example 2:
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
Explanation: "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.

__Note:__
You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
Input words contain only lowercase letters.

__Follow up:__
Try to solve it in O(n log k) time and O(n) extra space.

__题目描述__:
给一非空的单词列表，返回前 k 个出现次数最多的单词。

返回的答案应该按单词出现频率由高到低排序。如果不同的单词有相同出现频率，按字母顺序排序。

__示例 :__

示例 1：

输入: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
输出: ["i", "love"]
解析: "i" 和 "love" 为出现次数最多的两个单词，均为2次。
    注意，按字母顺序 "i" 在 "love" 之前。

示例 2：

输入: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
输出: ["the", "is", "sunny", "day"]
解析: "the", "is", "sunny" 和 "day" 是出现次数最多的四个单词，
    出现次数依次为 4, 3, 2 和 1 次。

__注意:__

假定 k 总为有效值， 1 ≤ k ≤ 集合元素数。
输入的单词均由小写字母组成。

__扩展练习：__

尝试以 O(n log k) 时间复杂度和 O(n) 空间复杂度解决。

__思路__:

堆排序
用哈希表记录每个单词出现的次数
使用堆排序找出频率前 k 的元素
时间复杂度为 O(nlgk), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> topKFrequent(vector<string>& words, int k) 
    {
        unordered_map<string, int> count;
        for (auto& word : words) ++count[word];
        auto cmp = [](const auto& a, const auto& b) { return a.second == b.second ? a.first < b.first : a.second > b.second; };
        priority_queue<pair<string, int>, vector<pair<string, int>>, decltype(cmp)> pq(cmp);
        for (auto& it : count) 
        {
            pq.emplace(it);
            if (pq.size() > k) pq.pop();
        }
        vector<string> result(k);
        for (int i = k - 1; i >= 0; i--) 
        {
            result[i] = pq.top().first;
            pq.pop();
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        return Arrays.stream(words).collect(Collectors.groupingBy(Function.identity(), Collectors.counting())).entrySet().stream().sorted((o1, o2) -> { if (o1.getValue().equals(o2.getValue())) { return o1.getKey().compareTo(o2.getKey()); } else { return o2.getValue().compareTo(o1.getValue()); } }).map(Map.Entry::getKey).limit(k).collect(Collectors.toList());
    }
}
```

__Python__:

```Python
class Solution:
    def topKFrequent(self, words: List[str], k: int) -> List[str]:
        return [x[1] for x in heapq.nsmallest(k, [(v, k) for k, v in Counter(words).items()], key=lambda a: (-a[0], a[1]))]
```
