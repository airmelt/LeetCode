# 2251 Number of Flowers in Full Bloom 花期内花的数目

__Description:__

You are given a __0-indexed__ 2D integer array `flowers`, where `flowers[i] = [start_i, end_i]` means the `i ^ th` flower will be in __full bloom__ from `start_i` to `end_i` (__inclusive__). You are also given a __0-indexed__ integer array `people` of size `n`, where `people[i]` is the time that the `i ^ th` person will arrive to see the flowers.

Return _an integer array_ `answer` _of size_ `n`_, where_ `answer[i]` _is the __number__ of flowers that are in full bloom when the_ `i ^ th` _person arrives._

__Example:__

Example 1:

![2251-1](https://assets.leetcode.com/uploads/2022/03/02/ex1new.jpg)

```text
Input: flowers = [[1,6],[3,7],[9,12],[4,13]], people = [2,3,7,11]
Output: [1,2,2,2]
Explanation: The figure above shows the times when the flowers are in full bloom and when the people arrive.
For each person, we return the number of flowers in full bloom during their arrival.
```

Example 2:

![2251-2](https://assets.leetcode.com/uploads/2022/03/02/ex2new.jpg)

```text
Input: flowers = [[1,10],[3,3]], people = [3,3,2]
Output: [2,2,1]
Explanation: The figure above shows the times when the flowers are in full bloom and when the people arrive.
For each person, we return the number of flowers in full bloom during their arrival.
```

__Constraints:__

- `1 <= flowers.length <= 5 * 10 ^ 4`
- `flowers[i].length == 2`
- `1 <= start_i <= end_i <= 10 ^ 9`
- `1 <= people.length <= 5 * 10 ^ 4`
- `1 <= people[i] <= 10 ^ 9`

__题目描述:__

给你一个下标从 __0__ 开始的二维整数数组 `flowers` ，其中 `flowers[i] = [start_i, end_i]` 表示第 `i` 朵花的 __花期__ 从 `start_i` 到 `end_i` （都 __包含__）。同时给你一个下标从 __0__ 开始大小为 `n` 的整数数组 `people` ， `people[i]` 是第 `i` 个人来看花的时间。

请你返回一个大小为 `n` 的整数数组 `answer` ，其中 `answer[i]`是第 `i` 个人到达时在花期内花的 __数目__ 。

__示例:__

示例 1：

![2251-3](https://assets.leetcode.com/uploads/2022/03/02/ex1new.jpg)

```text
输入：flowers = [[1,6],[3,7],[9,12],[4,13]], people = [2,3,7,11]
输出：[1,2,2,2]
解释：上图展示了每朵花的花期时间，和每个人的到达时间。
对每个人，我们返回他们到达时在花期内花的数目。
```

示例 2：

![2251-4](https://assets.leetcode.com/uploads/2022/03/02/ex2new.jpg)

```text
输入：flowers = [[1,10],[3,3]], people = [3,3,2]
输出：[2,2,1]
解释：上图展示了每朵花的花期时间，和每个人的到达时间。
对每个人，我们返回他们到达时在花期内花的数目。
```

__提示：__

- `1 <= flowers.length <= 5 * 10 ^ 4`
- `flowers[i].length == 2`
- `1 <= start_i <= end_i <= 10 ^ 9`
- `1 <= people.length <= 5 * 10 ^ 4`
- `1 <= people[i] <= 10 ^ 9`

__思路:__

```text
1. 二分
将开花开始的时间和结束的时间分别排序
对于每个人的到达时间 p, 二分查找开始时间的下标 start 和结束时间的下标 end
则还在开放的花为 start - end
starts 中查找 p 的右边界, ends 中查找 p 的左边界
时间复杂度为 O((N + M)logN), 空间复杂度为 O(N), 其中 N 为花的数量, M 为人的数量
2. 差分 ➕ 二分
对于每朵花, 差分数组的开始位置 +1, 结束位置 -1
为了快速计算每个人到达的时间对应的花的数量, 将人的到达时间排序
这样在遍历人的同时, 累计到达之前的花的数量, 即前缀和
就可以得到每个人到达时的花的数量
时间复杂度为 O(NlogN + MlogM), 空间复杂度为 O(N + M), 其中 N 为花的数量, M 为人的数量
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> fullBloomFlowers(vector<vector<int>>& flowers, vector<int>& people) 
    {
        map<int, int> diff;
        for (const auto& flower : flowers)
        {
            ++diff[flower.front()];
            --diff[flower.back() + 1];
        }
        int n = people.size(), s = 0;
        vector<int> ids(n);
        iota(ids.begin(), ids.end(), 0);
        sort(ids.begin(), ids.end(), [&](int i, int j) { return people[i] < people[j]; });
        auto it = diff.begin();
        for (const auto& i : ids)
        {
            while (it != diff.end() and it -> first <= people[i]) s += it++ -> second;
            people[i] = s;
        }
        return people;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] fullBloomFlowers(int[][] flowers, int[] people) {
        int[] starts = Arrays.stream(flowers).mapToInt(f -> f[0]).sorted().toArray(), ends = Arrays.stream(flowers).mapToInt(f -> f[1]).sorted().toArray();
        return Arrays.stream(people).map(p -> helper(starts, p + 1) - helper(ends, p)).toArray();
    }

    private int helper(int[] nums, int target) {
        int left = 0, right = nums.length, mid = 0;
        while (left < right) {
            if (nums[mid = left + ((right - left) >>> 1)] < target) left = mid + 1;
            else right = mid;
        }
        return right;
    }
}
```

__Python__:

```Python
class Solution:
    def fullBloomFlowers(self, flowers: List[List[int]], people: List[int]) -> List[int]:
        return [bisect_right(starts, p) - bisect_left(ends, p) for p in people] if (starts := sorted([s for s, _ in flowers])) and (ends := sorted([e for _, e in flowers])) and (n := len(people)) else []
```
