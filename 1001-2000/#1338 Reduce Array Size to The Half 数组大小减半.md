# 1338 Reduce Array Size to The Half 数组大小减半

__Description:__

You are given an integer array arr. You can choose a set of integers and remove all the occurrences of these integers in the array.

Return the minimum size of the set so that at least half of the integers of the array are removed.

__Example:__

Example 1:

Input: arr = [3,3,3,3,5,5,5,2,2,7]
Output: 2
Explanation: Choosing {3,7} will make the new array [5,5,5,2,2] which has size 5 (i.e equal to half of the size of the old array).
Possible sets of size 2 are {3,5},{3,2},{5,2}.
Choosing set {2,7} is not possible as it will make the new array [3,3,3,3,5,5,5] which has a size greater than half of the size of the old array.

Example 2:

Input: arr = [7,7,7,7,7,7]
Output: 1
Explanation: The only possible set you can choose is {7}. This will make the new array empty.

__Constraints:__

2 <= arr.length <= 10^5
arr.length is even.
1 <= arr[i] <= 10^5

__题目描述：__

给你一个整数数组 arr。你可以从中选出一个整数集合，并删除这些整数在数组中的每次出现。

返回 至少 能删除数组中的一半整数的整数集合的最小大小。

__示例：__

示例 1：

输入：arr = [3,3,3,3,5,5,5,2,2,7]
输出：2
解释：选择 {3,7} 使得结果数组为 [5,5,5,2,2]、长度为 5（原数组长度的一半）。
大小为 2 的可行集合有 {3,5},{3,2},{5,2}。
选择 {2,7} 是不可行的，它的结果数组为 [3,3,3,3,5,5,5]，新数组长度大于原数组的二分之一。

示例 2：

输入：arr = [7,7,7,7,7,7]
输出：1
解释：我们只能选择集合 {7}，结果数组为空。

__提示：__

1 <= arr.length <= 10^5
arr.length 为偶数
1 <= arr[i] <= 10^5

__思路：__

贪心
哈希表 ➕ 优先队列
用哈希表记录每一个元素出现的次数
将出现的次数加入优先队列中
从大到小加入优先队列中的值直到超过数组长度的一半
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:

__C++__:

```C++
class Solution 
{
public:
    int minSetSize(vector<int>& arr) 
    {
        unordered_map<int, int> m;
        for (const auto& num : arr) ++m[num];
        int result = 0, cur = 0, target = arr.size() >> 1;
        priority_queue<int> pq;
        for (const auto& [k, v] : m) pq.push(v);
        while (cur < target) 
        {
            cur += pq.top();
            pq.pop();
            ++result;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int minSetSize(int[] arr) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : arr) map.put(num, map.getOrDefault(num, 0) + 1);
        int result = 0, cur = 0, target = arr.length >> 1;
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        for (int count : map.values()) pq.offer(-count);
        while (cur < target) {
            cur += -pq.poll();
            ++result;
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def minSetSize(self, arr: List[int]) -> int:
        return bisect_left(list(accumulate(sorted(Counter(arr).values(), reverse=True))), len(arr) >> 1) + 1
```
