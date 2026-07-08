# 3092 Most Frequent IDs 最高频率的 ID

__Description:__

The problem involves tracking the frequency of IDs in a collection that changes over time. You have two integer arrays, `nums` and `freq`, of equal length `n`. Each element in `nums` represents an ID, and the corresponding element in `freq` indicates how many times that ID should be added to or removed from the collection at each step.

- __Addition of IDs:__ If `freq[i]` is positive, it means `freq[i]` IDs with the value `nums[i]` are added to the collection at step `i`.
- __Removal of IDs:__ If `freq[i]` is negative, it means `-freq[i]` IDs with the value `nums[i]` are removed from the collection at step `i`.

Return an array `ans` of length `n`, where `ans[i]` represents the __count__ of the _most frequent ID_ in the collection after the `i ^ th` step. If the collection is empty at any step, `ans[i]` should be 0 for that step.

__Example:__

Example 1:

```text
Input: nums = [2,3,2,1], freq = [3,2,-3,1]

Output: [3,3,2,2]

Explanation:
```

After step 0, we have 3 IDs with the value of 2. So `ans[0] = 3`.

After step 1, we have 3 IDs with the value of 2 and 2 IDs with the value of 3. So `ans[1] = 3`.

After step 2, we have 2 IDs with the value of 3. So `ans[2] = 2`.

After step 3, we have 2 IDs with the value of 3 and 1 ID with the value of 1. So `ans[3] = 2`.

Example 2:

```text
Input: nums = [5,5,3], freq = [2,-2,1]

Output: [2,0,1]

Explanation:
```

After step 0, we have 2 IDs with the value of 5. So `ans[0] = 2`.

After step 1, there are no IDs. So `ans[1] = 0`.

After step 2, we have 1 ID with the value of 3. So `ans[2] = 1`.

__Constraints:__

- `1 <= nums.length == freq.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `-10 ^ 5 <= freq[i] <= 10 ^ 5`
- `freq[i] != 0`
- The input is generated such that the occurrences of an ID will not be negative in any step.

__题目描述:__

你需要在一个集合里动态记录 ID 的出现频率。给你两个长度都为 `n` 的整数数组 `nums` 和 `freq` ， `nums` 中每一个元素表示一个 ID ，对应的 `freq` 中的元素表示这个 ID 在集合中此次操作后需要增加或者减少的数目。

- __增加 ID 的数目:__ 如果 `freq[i]` 是正数，那么 `freq[i]` 个 ID 为 `nums[i]` 的元素在第 `i` 步操作后会添加到集合中。
- __减少 ID 的数目:__ 如果 `freq[i]` 是负数，那么 `-freq[i]` 个 ID 为 `nums[i]` 的元素在第 `i` 步操作后会从集合中删除。

请你返回一个长度为 `n` 的数组 `ans` ，其中 `ans[i]` 表示第 `i` 步操作后出现频率最高的 ID __数目__ ，如果在某次操作后集合为空，那么 `ans[i]` 为 0 。

__示例:__

示例 1：

```text
输入：nums = [2,3,2,1], freq = [3,2,-3,1]

输出：[3,3,2,2]

解释：
```

第 0 步操作后，有 3 个 ID 为 2 的元素，所以 `ans[0] = 3` 。

第 1 步操作后，有 3 个 ID 为 2 的元素和 2 个 ID 为 3 的元素，所以 `ans[1] = 3` 。

第 2 步操作后，有 2 个 ID 为 3 的元素，所以 `ans[2] = 2` 。

第 3 步操作后，有 2 个 ID 为 3 的元素和 1 个 ID 为 1 的元素，所以 `ans[3] = 2` 。

示例 2：

```text
输入：nums = [5,5,3], freq = [2,-2,1]

输出：[2,0,1]

解释：
```

第 0 步操作后，有 2 个 ID 为 5 的元素，所以 `ans[0] = 2` 。

第 1 步操作后，集合中没有任何元素，所以 `ans[1] = 0` 。

第 2 步操作后，有 1 个 ID 为 3 的元素，所以 `ans[2] = 1` 。

__提示：__

- `1 <= nums.length == freq.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 5`
- `-10 ^ 5 <= freq[i] <= 10 ^ 5`
- `freq[i] != 0`
- 输入保证任何操作后，集合中的元素出现次数不会为负数。

__思路:__

```text
1. 有序列表
记录每个数出现的频率
加入到有序列表中
返回有序列表最大值即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
2. 哈希表 ➕ 懒删除堆
用哈希表记录频率
如果堆里的数据对不上, 那就删除
否则直接更新
记录堆顶的数据即可
时间复杂度为 O(NlogN), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<long long> mostFrequentIDs(vector<int>& nums, vector<int>& freq) 
    {
        int n = nums.size();
        vector<long long> result(n);
        unordered_map<int, long long> m;
        priority_queue<pair<long long, int>> pq;
        for (int i = 0, x = 0; i < n; i++) 
        {
            pq.emplace(m[x = nums[i]] += freq[i], x);
            while (pq.top().first != m[pq.top().second]) pq.pop();
            result[i] = pq.top().first;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long[] mostFrequentIDs(int[] nums, int[] freq) {
        var map = new HashMap<Integer, Long>();
        var tree = new TreeMap<Long, Integer>();
        int n = nums.length;
        long result[] = new long[n], cur = 0L, v = 0L;
        for (int i = 0, x = 0; i < n; i++) {
            if (map.containsKey(x = nums[i]) && tree.containsKey(v = map.get(x)) && tree.merge(v, -1, Integer::sum) == 0) tree.remove(v);
            tree.merge(cur = map.merge(x, (long)freq[i], Long::sum), 1, Integer::sum);
            result[i] = tree.lastKey();
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def mostFrequentIDs(self, nums: List[int], freq: List[int]) -> List[int]:
        c, sl, result = defaultdict(int), SortedList(), []
        for num, f in zip(nums, freq):
            sl.discard(c[num])
            c[num] += f
            sl.add(c[num])
            result.append(sl[-1])
        return result
```
