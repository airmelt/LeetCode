# 898 Bitwise ORs of Subarrays 子数组按位或操作

__Description__:
We have an array arr of non-negative integers.

For every (contiguous) subarray sub = [arr[i], arr[i + 1], ..., arr[j]] (with i <= j), we take the bitwise OR of all the elements in sub, obtaining a result arr[i] | arr[i + 1] | ... | arr[j].

Return the number of possible results. Results that occur more than once are only counted once in the final answer

__Example:__

Example 1:

Input: arr = [0]
Output: 1
Explanation: There is only one possible result: 0.

Example 2:

Input: arr = [1,1,2]
Output: 3
Explanation: The possible subarrays are [1], [1], [2], [1, 1], [1, 2], [1, 1, 2].
These yield the results 1, 1, 2, 1, 3, 3.
There are 3 unique values, so the answer is 3.

Example 3:

Input: arr = [1,2,4]
Output: 6
Explanation: The possible results are 1, 2, 3, 4, 6, and 7.

__Constraints:__

1 <= nums.length <= 5 * 10^4
0 <= nums[i] <= 10^9

__题目描述__:
我们有一个非负整数数组 A。

对于每个（连续的）子数组 B = [A[i], A[i+1], ..., A[j]] （ i <= j），我们对 B 中的每个元素进行按位或操作，获得结果 A[i] | A[i+1] | ... | A[j]。

返回可能结果的数量。 （多次出现的结果在最终答案中仅计算一次。）

__示例 :__

示例 1：

输入：[0]
输出：1
解释：
只有一个可能的结果 0 。

示例 2：

输入：[1,1,2]
输出：3
解释：
可能的子数组为 [1]，[1]，[2]，[1, 1]，[1, 2]，[1, 1, 2]。
产生的结果为 1，1，2，1，3，3 。
有三个唯一值，所以答案是 3 。

示例 3：

输入：[1,2,4]
输出：6
解释：
可能的结果是 1，2，3，4，6，以及 7 。

__提示:__

1 <= A.length <= 50000
0 <= A[i] <= 10^9

__思路__:

哈希表 ➕ 剪枝
将所有子数组的或的结果加入哈希表
最后返回哈希表的长度即可
剪枝的策略有两种
第一种是找到数组的最大值, mask = 2 ^ max_value - 1, 当或运算的值达到 mask 的时候说明后面的值不需要再参与运算了
第二种是修改原数组的方式, 从后往前遍历 arr[i] | arr[j] = arr[j] 说明 arr[j] 之前的数组或运算结果和自身相同可以剪枝
时间复杂度为 O(nlgm), 空间复杂度为 O(nlgm), n 为数组 arr 的长度, m 为 arr 的最大值

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int subarrayBitwiseORs(vector<int>& arr)
    {
        unordered_set<int> result;
        int mask = 1, max_value = *max_element(arr.begin(), arr.end()), n = arr.size();
        while (mask <= max_value) mask <<= 1;
        --mask;
        for (int i = 0; i < n; i++) 
        {
            int val = arr[i];
            result.insert(val);
            for (int j = i - 1; j > -1; j--) 
            {
                if (val == mask) break;
                val |= arr[j];
                result.insert(val);
            }
        }
        return result.size();
    }
};
```

__Java__:

```Java
class Solution {
    public int subarrayBitwiseORs(int[] arr) {
        Set<Integer> set = new HashSet<>(65536);
        for (int i = 0, n = arr.length; i < n; i++) {
            set.add(arr[i]);
            for (int j = i - 1; j > -1; j--) {
                if ((arr[j] | arr[i]) == arr[j]) break;
                arr[j] |= arr[i];
                set.add(arr[j]);
            }
        }
        return set.size();
    }
}
```

__Python__:

```Python
class Solution:
    def subarrayBitwiseORs(self, arr: List[int]) -> int:
        mask, max_value, nums, n = 1, max(arr), set(arr), len(arr)
        while max_value >= mask:
            mask <<= 1
        mask -= 1;
        for i in range(n):
            val = arr[i]
            for j in range(i - 1, -1, -1):
                if val == mask:
                    break
                val |= arr[j]
                nums.add(val)
        return len(nums)
```
