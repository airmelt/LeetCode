__Description__:
We define a harmounious array as an array where the difference between its maximum value and its minimum value is exactly 1.

Now, given an integer array, you need to find the length of its longest harmonious subsequence among all its possible subsequences.

__Example:__
Example 1:

Input: [1,3,2,2,5,2,3,7]
Output: 5
Explanation: The longest harmonious subsequence is [3,2,2,2,3].
 
__Note:__
The length of the input array will not exceed 20,000.

__题目描述__:
和谐数组是指一个数组里元素的最大值和最小值之间的差别正好是1。

现在，给定一个整数数组，你需要在所有可能的子序列中找到最长的和谐子序列的长度。

__示例 :__
示例 1:

输入: [1,3,2,2,5,2,3,7]
输出: 5
原因: 最长的和谐数组是：[3,2,2,2,3].

说明:
输入的数组长度最大不超过20,000.

__思路__:
用一个 map记录下数组元素出现的次数
遍历 map找到有刚好相差 1的就更新 result
时间复杂度O(n), 空间复杂度O(n)

__代码__:
__C++__:
```
class Solution {
public:
    int findLHS(vector<int>& nums) {
        int result = 0;
        map<int, int> m;
        for (auto num : nums) m[num]++;
        for (auto iter = m.begin(); iter != m.end(); iter++) if (m.count(iter -> first + 1)) result = max(result, m[iter -> first] + m[iter -> first + 1]);
        return result;
    }
};
```

__Java__:
```
class Solution {
    public int findLHS(int[] nums) {
        int result = 0;
        Map<Integer, Integer> map = new HashMap<>();
        for (int num : nums) map.put(num, map.getOrDefault(num, 0) + 1);
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            Integer count = map.get(entry.getKey() + 1);
            if (count != null) result = Math.max(result, count + entry.getValue());
        }
        return result;
    }
}
```

__Python__:
```
from collections import Counter
class Solution:
    def findLHS(self, nums: List[int]) -> int:
        c = Counter(nums)
        result = 0
        for i in c:
            if i + 1 in c:
                result = max(result, c[i] + c[i + 1])
        return result
```
