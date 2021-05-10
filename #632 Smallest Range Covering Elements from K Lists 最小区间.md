# 632 Smallest Range Covering Elements from K Lists 最小区间

__Description__:
You have k lists of sorted integers in non-decreasing order. Find the smallest range that includes at least one number from each of the k lists.

We define the range [a, b] is smaller than range [c, d] if b - a < d - c or a < c if b - a == d - c.

__Example:__

Example 1:

Input: nums = [[4,10,15,24,26],[0,9,12,20],[5,18,22,30]]
Output: [20,24]
Explanation:
List 1: [4, 10, 15, 24,26], 24 is in range [20,24].
List 2: [0, 9, 12, 20], 20 is in range [20,24].
List 3: [5, 18, 22, 30], 22 is in range [20,24].

Example 2:

Input: nums = [[1,2,3],[1,2,3],[1,2,3]]
Output: [1,1]

Example 3:

Input: nums = [[10,10],[11,11]]
Output: [10,11]

Example 4:

Input: nums = [[10],[11]]
Output: [10,11]

Example 5:

Input: nums = [[1],[2],[3],[4],[5],[6],[7]]
Output: [1,7]

__Constraints:__

nums.length == k
1 <= k <= 3500
1 <= nums[i].length <= 50
-10^5 <= nums[i][j] <= 10^5
nums[i] is sorted in non-decreasing order.

__题目描述__:
你有 k 个 非递减排列 的整数列表。找到一个 最小 区间，使得 k 个列表中的每个列表至少有一个数包含在其中。

我们定义如果 b-a < d-c 或者在 b-a == d-c 时 a < c，则区间 [a,b] 比 [c,d] 小。

__示例 :__

示例 1：

输入：nums = [[4,10,15,24,26], [0,9,12,20], [5,18,22,30]]
输出：[20,24]
解释：
列表 1：[4, 10, 15, 24, 26]，24 在区间 [20,24] 中。
列表 2：[0, 9, 12, 20]，20 在区间 [20,24] 中。
列表 3：[5, 18, 22, 30]，22 在区间 [20,24] 中。

示例 2：

输入：nums = [[1,2,3],[1,2,3],[1,2,3]]
输出：[1,1]

示例 3：

输入：nums = [[10,10],[11,11]]
输出：[10,11]

示例 4：

输入：nums = [[10],[11]]
输出：[10,11]

示例 5：

输入：nums = [[1],[2],[3],[4],[5],[6],[7]]
输出：[1,7]

__提示:__

nums.length == k
1 <= k <= 3500
1 <= nums[i].length <= 50
-10^5 <= nums[i][j] <= 10^5
nums[i] 按非递减顺序排列

__思路__:

1. 贪心 ➕ 最小堆
首先看每个数组的第一个数字, 初始化区间为 [min_value, max_value], 这个区间一定包含所有数组中的第一个元素, 并将第一个元素和它在 nums 的下标及在子数组的下标加入最小堆中
那么每次从最小堆弹出一定是当前各数组最小的元素, 每次将对应子数组的下一个元素加入堆, 并对 min_value, max_value 进行更新, 直到找到最小区间
时间复杂度 O(nklgk), 空间复杂度 O(k)
2. 哈希表 ➕ 滑动窗口
考虑将子数组的下标作为哈希表的值, 子数组对应元素的值作为哈希表的键
那么可以用一个滑动窗口包含所有的数组 [0, nums.size() - 1] 来搜索最小区间
时间复杂度 O(nk + |V|), 空间复杂度 O(nk), |V| 表示 nums 中各元素的值的范围, 本题中为 2 * 10 ^ 5

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> smallestRange(vector<vector<int>>& nums) 
    {
        map<int, vector<int>> m;
        int size = nums.size(), min_value = 1000000, max_value = -1000000;
        for (int i = 0; i < size; i++)
        {
            for (const auto &x : nums[i])
            {
                m[x].emplace_back(i);
                min_value = min(x, min_value);
                max_value = max(x, max_value);
            }
        }
        int cur_left = min_value, cur_right = min_value - 1, left = min_value, right = max_value, count = 0;
        vector<int> freq(size, 0);
        while (cur_right++ < max_value)
        {
            if (m.count(cur_right))
            {
                for (const auto &x : m[cur_right])
                {
                    ++freq[x];
                    if (freq[x] == 1) ++count;
                }
            }
            while (count == size)
            {
                if (cur_right - cur_left < right - left)
                {
                    right = cur_right;
                    left = cur_left;
                }
                if (m.count(cur_left))
                {
                    for (const auto &x : m[cur_left])
                    {
                        --freq[x];
                        if (!freq[x]) --count;
                    }
                }
                ++cur_left;
            }
        }
        return {left, right};
    }
};
```

__Java__:

```Java
class Solution {
    public int[] smallestRange(List<List<Integer>> nums) {
        Map<Integer, List<Integer>> map = new HashMap<>();
        int size = nums.size(), min = 1000000, max = -1000000;
        for (int i = 0; i < size; i++) {
            for (int x : nums.get(i)) {
                List<Integer> cur = map.getOrDefault(x, new ArrayList<Integer>());
                cur.add(i);
                map.put(x, cur);
                min = Math.min(x, min);
                max = Math.max(x, max);
            }
        }
        int freq[] = new int[size], cur_left = min, cur_right = min - 1, left = min, right = max, count = 0;
        while (cur_right < max) {
            ++cur_right;
            if (map.containsKey(cur_right)) {
                for (int x : map.get(cur_right)) {
                    ++freq[x];
                    if (freq[x] == 1) ++count;
                }
            }
            while (count == size) {
                if (right - left > cur_right - cur_left) {
                    left = cur_left;
                    right = cur_right;
                }
                if (map.containsKey(cur_left)) {
                    for (int x : map.get(cur_left)) {
                        --freq[x];
                        if (freq[x] == 0) --count;
                    }
                }
                ++cur_left;
            }
        } 
        return new int[]{left, right};
    }
}
```

__Python__:

```Python
class Solution:
    def smallestRange(self, nums: List[List[int]]) -> List[int]:
        left, right, max_value, heap = -float('inf'), float('inf'), max(v[0] for v in nums), [(v[0], i, 0) for i, v in enumerate(nums)]
        heapq.heapify(heap)
        while True:
            min_value, row, i = heapq.heappop(heap)
            if max_value - min_value < right - left:
                left, right = min_value, max_value
            if (i := i + 1) == len(nums[row]):
                break
            max_value = max(max_value, nums[row][i])
            heapq.heappush(heap, (nums[row][i], row, i))
        return [left, right]
```
