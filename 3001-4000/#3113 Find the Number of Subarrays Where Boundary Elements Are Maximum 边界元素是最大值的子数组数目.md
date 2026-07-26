# 3113 Find the Number of Subarrays Where Boundary Elements Are Maximum 边界元素是最大值的子数组数目

__Description:__

You are given an array of __positive__ integers `nums`.

Return the number of subarrays of `nums`, where the __first__ and the __last__ elements of the subarray are _equal_ to the __largest__ element in the subarray.

__Example:__

Example 1:

```text
Input: nums = [1,4,3,3,2]

Output: 6

Explanation:

There are 6 subarrays which have the first and the last elements equal to the largest element of the subarray:
```

- subarray [__1__,4,3,3,2], with its largest element 1. The first element is 1 and the last element is also 1.
- subarray [1,__4__,3,3,2], with its largest element 4. The first element is 4 and the last element is also 4.
- subarray [1,4,__3__,3,2], with its largest element 3. The first element is 3 and the last element is also 3.
- subarray [1,4,3,__3__,2], with its largest element 3. The first element is 3 and the last element is also 3.
- subarray [1,4,3,3,__2__], with its largest element 2. The first element is 2 and the last element is also 2.
- subarray [1,4,__3,3__,2], with its largest element 3. The first element is 3 and the last element is also 3.

Hence, we return 6.

Example 2:

```text
Input: nums = [3,3,3]
```

Output: 6

Explanation:

There are 6 subarrays which have the first and the last elements equal to the largest element of the subarray:

- subarray `[__3__,3,3]`, with its largest element 3. The first element is 3 and the last element is also 3.
- subarray `[3,__3__,3]`, with its largest element 3. The first element is 3 and the last element is also 3.
- subarray `[3,3,__3__]`, with its largest element 3. The first element is 3 and the last element is also 3.
- subarray `[__3,3__,3]`, with its largest element 3. The first element is 3 and the last element is also 3.
- subarray `[3,__3,3__]`, with its largest element 3. The first element is 3 and the last element is also 3.
- subarray `[__3,3,3__]`, with its largest element 3. The first element is 3 and the last element is also 3.

Hence, we return 6.

Example 3:

```text
Input: nums = [1]

Output: 1

Explanation:
```

There is a single subarray of `nums` which is [__1__], with its largest element 1. The first element is 1 and the last element is also 1.

Hence, we return 1.

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__题目描述:__

给你一个 __正__ 整数数组 `nums` 。

请你求出 `nums` 中有多少个子数组，满足子数组中 __第一个__ 和 __最后一个__ 元素都是这个子数组中的 __最大__ 值。

__示例:__

示例 1：

```text
输入：nums = [1,4,3,3,2]

输出：6

解释：

总共有 6 个子数组满足第一个元素和最后一个元素都是子数组中的最大值：
```

- 子数组 [___1___,4,3,3,2] ，最大元素为 1 ，第一个和最后一个元素都是 1 。
- 子数组 [1,___4___,3,3,2] ，最大元素为 4 ，第一个和最后一个元素都是 4 。
- 子数组 [1,4,___3___,3,2] ，最大元素为 3 ，第一个和最后一个元素都是 3 。
- 子数组 [1,4,3,___3___,2] ，最大元素为 3 ，第一个和最后一个元素都是 3 。
- 子数组 [1,4,3,3,___2___] ，最大元素为 2 ，第一个和最后一个元素都是 2 。
- 子数组 [1,4,___3,3___,2] ，最大元素为 3 ，第一个和最后一个元素都是 3 。

所以我们返回 6 。

示例 2：

```text
输入：nums = [3,3,3]

输出：6

解释：

总共有 6 个子数组满足第一个元素和最后一个元素都是子数组中的最大值：
```

- 子数组 [___3___,3,3] ，最大元素为 3 ，第一个和最后一个元素都是 3 。
- 子数组 [3,___3___,3] ，最大元素为 3 ，第一个和最后一个元素都是 3 。
- 子数组 [3,3,___3___] ，最大元素为 3 ，第一个和最后一个元素都是 3 。
- 子数组 [___3,3___,3] ，最大元素为 3 ，第一个和最后一个元素都是 3 。
- 子数组 [3,___3,3___] ，最大元素为 3 ，第一个和最后一个元素都是 3 。
- 子数组 [___3,3,3___] ，最大元素为 3 ，第一个和最后一个元素都是 3 。

所以我们返回 6 。

示例 3：

```text
输入：nums = [1]

输出：1

解释：
```

`nums` 中只有一个子数组 [___1___] ，最大元素为 1 ，第一个和最后一个元素都是 1 。

所以我们返回 1 。

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 10 ^ 9`

__思路:__

```text
单调栈
单调栈中记录当前元素的值以及当前连续长度
如果遍历的元素大于栈顶, 就弹出
否则
当栈顶元素就是当前元素时, 将连续长度累加进结果, 当前连续长度自增
否则将当前元素以及长度 1 加入单调栈
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    long long numberOfSubarrays(vector<int>& nums) 
    {
        long long result = nums.size();
        stack<pair<int, int>> st;
        for (const auto& num : nums) 
        {
            while (!st.empty() and num > st.top().first) st.pop();
            if (!st.empty() and num == st.top().first) result += st.top().second++;
            else st.emplace(num, 1);
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public long numberOfSubarrays(int[] nums) {
        long result = nums.length;
        var stack = new ArrayDeque<int[]>();
        for (int num : nums) {
            while (!stack.isEmpty() && num > stack.peek()[0]) stack.pop();
            if (!stack.isEmpty() && num == stack.peek()[0]) result += stack.peek()[1]++;
            else stack.push(new int[]{ num, 1 });
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def numberOfSubarrays(self, nums: List[int]) -> int:
        result, stack = len(nums), []
        for num in nums:
            while stack and num > stack[-1][0]:
                stack.pop()
            if stack and stack[-1][0] == num:
                result += stack[-1][1]
                stack[-1][1] += 1
            else:
                stack.append([num, 1])
        return result
```
