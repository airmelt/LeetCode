# 1481 Least Number of Unique Integers after K Removals 不同整数的最少数目

__Description:__

Given an array of integers  `arr` and an integer  `k`. Find the _least number of unique integers_ after removing __exactly__  `k` elements _._

__Example:__

Example 1:

```text
Input: arr = [5,5,4], k = 1
Output: 1
Explanation: Remove the single 4, only 5 is left.
```

Input: arr = [4,3,1,1,3,3,2], k = 3
Output: 2
Explanation: Remove 4, 2 and either one of the two 1s or three 3s. 1 and 3 will be left.

__Constraints:__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 9`
- `0 <= k <= arr.length`

__题目描述:__

给你一个整数数组  `arr` 和一个整数  `k` 。现需要从数组中恰好移除  `k` 个元素，请找出移除后数组中不同整数的最少数目。

__示例:__

示例 1：

```text
输入：arr = [5,5,4], k = 1
输出：1
解释：移除 1 个 4 ，数组中只剩下 5 一种整数。
```

示例 2：

```text
输入：arr = [4,3,1,1,3,3,2], k = 3
输出：2
解释：先移除 4、2 ，然后再移除两个 1 中的任意 1 个或者三个 3 中的任意 1 个，最后剩下 1 和 3 两种整数。
```

__提示：__

- `1 <= arr.length <= 10 ^ 5`
- `1 <= arr[i] <= 10 ^ 9`
- `0 <= k <= arr.length`

__思路:__

```text
优先队列
先用哈希表统计每个元素的个数
将每个元素的个数插入优先队列
每次移除一种元素直到 k 小于 0
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int findLeastNumOfUniqueInts(vector<int>& arr, int k) 
    {
        unordered_map<int, int> m;
        for (const auto& num : arr) ++m[num];
        priority_queue<int, vector<int>, greater<int>> pq;
        for (const auto& [k, v] : m) pq.push(v);
        while (k and !pq.empty()) 
        {
            int t = pq.top(), a = min(t, k); 
            pq.pop();
            t -= a;
            k -= a;
            if (t > 0) pq.push(t);
        }
        return pq.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int findLeastNumOfUniqueInts(int[] arr, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : arr) map.put(num, map.getOrDefault(num, 0) + 1);
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (Integer value: map.values()) pq.offer(value);
        while (k > 0 && !pq.isEmpty()) {
            int t = pq.poll(), a = Math.min(t, k); 
            t -= a;
            k -= a;
            if (t > 0) pq.offer(t);
        }
        return pq.size();
    }
}
```

__Python__:

```Python
class Solution:
    def findLeastNumOfUniqueInts(self, arr: List[int], k: int) -> int:
        c = list(collections.Counter(arr).values())
        c.sort(reverse=True)
        while k:
            c[-1] -= 1
            if not c[-1]:
                del c[-1]
            k -= 1
        return len(c)
```
