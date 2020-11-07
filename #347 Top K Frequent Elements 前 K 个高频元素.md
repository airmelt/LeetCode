__Description__:
Given a non-empty array of integers, return the k most frequent elements.

__Example:__
Example 1:

Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

Example 2:

Input: nums = [1], k = 1
Output: [1]

__Note:__

You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
Your algorithm's time complexity must be better than O(n log n), where n is the array's size.
It's guaranteed that the answer is unique, in other words the set of the top k frequent elements is unique.
You can return the answer in any order.

__题目描述__:
给定一个非空的整数数组，返回其中出现频率前 k 高的元素。

__示例 :__
示例 1:

输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]

示例 2:

输入: nums = [1], k = 1
输出: [1]

__提示：__

你可以假设给定的 k 总是合理的，且 1 ≤ k ≤ 数组中不相同的元素的个数。
你的算法的时间复杂度必须优于 O(n log n) , n 是数组的大小。
题目数据保证答案唯一，换句话说，数组中前 k 个高频元素的集合是唯一的。
你可以按任意顺序返回答案。

__思路__:
由于要求时间复杂度优于 nlogn, 所以不能使用直接排序的方式
先将数字对应的出现频率存在 map中
然后将 map中的元素加入优先队列(小根堆)中, 保证优先队列的大小小于等于 k
时间复杂度O(nlogk), 空间复杂度O(n)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    vector<int> topKFrequent(vector<int>& nums, int k) 
    {
        vector<int> result(k);
        unordered_map<int, int> m;
        priority_queue<pair<int, int>> pq;
        for (auto num : nums) ++m[num];
        for (auto &item : m)
        {
            pq.push(pair<int, int>(-item.second, item.first));
            if (pq.size() > k) pq.pop();
        }
        while (k--)
        {
            result[k] = pq.top().second;
            pq.pop();
        }
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        return Arrays.stream(nums).boxed().collect(Collectors.toMap(e -> e, e -> 1, Integer::sum)).entrySet().stream().sorted((m1, m2) -> m2.getValue() - m1.getValue()).limit(k).mapToInt(Map.Entry::getKey).toArray();
    }
}
```

__Python__:
```Python
class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        return [i[0] for i in Counter(nums).most_common(k)]
```