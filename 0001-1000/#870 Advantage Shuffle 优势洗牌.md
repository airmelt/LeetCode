# 870 Advantage Shuffle 优势洗牌

__Description__:
You are given two integer arrays nums1 and nums2 both of the same length. The advantage of nums1 with respect to nums2 is the number of indices i for which nums1[i] > nums2[i].

Return any permutation of nums1 that maximizes its advantage with respect to nums2.

__Example:__

Example 1:

Input: nums1 = [2,7,11,15], nums2 = [1,10,4,11]
Output: [2,11,7,15]

Example 2:

Input: nums1 = [12,24,8,32], nums2 = [13,25,32,11]
Output: [24,32,8,12]

__Constraints:__

1 <= nums1.length <= 10^5
nums2.length == nums1.length
0 <= nums1[i], nums2[i] <= 10^9

__题目描述__:
给定两个大小相等的数组 A 和 B，A 相对于 B 的优势可以用满足 A[i] > B[i] 的索引 i 的数目来描述。

返回 A 的任意排列，使其相对于 B 的优势最大化。

__示例 :__

示例 1：

输入：A = [2,7,11,15], B = [1,10,4,11]
输出：[2,11,7,15]

示例 2：

输入：A = [12,24,8,32], B = [13,25,32,11]
输出：[24,32,8,12]

__提示:__

1 <= A.length = B.length <= 10000
0 <= A[i] <= 10^9
0 <= B[i] <= 10^9

__思路__:

贪心
对 A 排序
将 B 中元素和对应下标插入优先队列或者排序
每次选择一个 A 中最小大于当前数字的数
如果 A 中最大的数都比当前数小, 则选最小的
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<int> advantageCount(vector<int>& nums1, vector<int>& nums2) 
    {
        int n = nums1.size(), left = 0, right = n - 1, right2 = n;
        vector<int> result(n, 0);
        vector<pair<int, int>> v;
        for (int i = 0; i < n; i++) v.emplace_back(make_pair(nums2[i], i));
        sort(nums1.begin(), nums1.end());
        sort(v.begin(), v.end());
        while (--right2 > -1)
        {
            if (nums1[right] > v[right2].first) result[v[right2].second] = nums1[right--];
            else result[v[right2].second] = nums1[left++];
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] advantageCount(int[] nums1, int[] nums2) {
        int n = nums1.length, left = 0, right = n - 1, result[] = new int[n];
        Arrays.sort(nums1);
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> { return b[1] - a[1]; });
        for (int i = 0; i < n; i++) pq.offer(new int[] { i, nums2[i] });
        while (!pq.isEmpty()) {
            int[] cur = pq.poll();
            if (nums1[right] > cur[1]) result[cur[0]] = nums1[right--];
            else result[cur[0]] = nums1[left++];
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def advantageCount(self, nums1: List[int], nums2: List[int]) -> List[int]:
        sorted1, sorted2, assigned, remaining, i = sorted(nums1), sorted(nums2), { num: [] for num in nums2 }, [], 0
        for num in sorted1:
            if num > sorted2[i]:
                assigned[sorted2[i]].append(num)
                i += 1
            else:
                remaining.append(num)
        return [assigned[num].pop() if assigned[num] else remaining.pop() for num in nums2]
```
