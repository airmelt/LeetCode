# 954 Array of Doubled Pairs 二倍数对数组

__Description__:
Given an integer array of even length arr, return true if it is possible to reorder arr such that arr[2 * i + 1] = 2 * arr[2 * i] for every 0 <= i < len(arr) / 2, or false otherwise.

__Example:__

Example 1:

Input: arr = [3,1,3,6]
Output: false

Example 2:

Input: arr = [2,1,2,6]
Output: false

Example 3:

Input: arr = [4,-2,2,-4]
Output: true
Explanation: We can take two groups, [-2,-4] and [2,4] to form [-2,-4,2,4] or [2,4,-2,-4].

Example 4:

Input: arr = [1,2,4,16,8,4]
Output: false

__Constraints:__

2 <= arr.length <= 3 * 10^4
arr.length is even.
-10^5 <= arr[i] <= 10^5

__题目描述__:
给定一个长度为偶数的整数数组 arr，只有对 arr 进行重组后可以满足 “对于每个 0 <= i < len(arr) / 2，都有 arr[2 * i + 1] = 2 * arr[2 * i]” 时，返回 true；否则，返回 false。

__示例 :__

示例 1：

输入：arr = [3,1,3,6]
输出：false

示例 2：

输入：arr = [2,1,2,6]
输出：false

示例 3：

输入：arr = [4,-2,2,-4]
输出：true
解释：可以用 [-2,-4] 和 [2,4] 这两组组成 [-2,-4,2,4] 或是 [2,4,-2,-4]

示例 4：

输入：arr = [1,2,4,16,8,4]
输出：false

__提示:__

0 <= arr.length <= 3 * 10^4
arr.length 是偶数
-10^5 <= arr[i] <= 10^5

__思路__:

排序
按照绝对值排序
用哈希表统计 arr 中元素的个数
每次查找是否有 2 \* num 在哈希表中
然后将 num 及 2 \* num 的计数都减少 1
时间复杂度为 O(nlgn), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    bool canReorderDoubled(vector<int>& arr) 
    {
        unordered_map<int, int> m;
        sort(arr.begin(), arr.end(), [](int a, int b){ return abs(a) < abs(b); });
        for (const auto& num : arr) ++m[num];
        for (const auto& num : arr)
        {
            if (m[num])
            {
                if (m[num * 2] <= 0) return false;
                --m[num];
                --m[num * 2];
            }
        }
        return true;
    }
};
```

__Java__:

```Java
class Solution {
    public boolean canReorderDoubled(int[] arr) {
        Map<Integer, Integer> count = new HashMap<>();
        for (int num : arr) count.put(num, count.getOrDefault(num, 0) + 1);
        Integer[] nums = new Integer[arr.length];
        for (int i = 0, n = arr.length; i < n; i++) nums[i] = arr[i];
        Arrays.sort(nums, Comparator.comparingInt(Math::abs));
        for (int num : nums) {
            if (count.get(num) != 0) {
                if (count.getOrDefault(num << 1, 0) <= 0) return false;
                count.put(num, count.get(num) - 1);
                count.put(num << 1, count.get(num << 1) - 1);
            }
        }
        return true;
    }
}
```

__Python__:

```Python
class Solution:
    def canReorderDoubled(self, arr: List[int]) -> bool:
        arr.sort(key=abs)
        d = Counter(arr)
        for i in d:
            if d[i]:
                if (i << 1) in d:
                    d[i << 1] -= d[i]
                    if d[i << 1] < 0:
                        return False
                else:
                    return False
        return True
```
