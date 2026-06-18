# 3069 Distribute Elements Into Two Arrays I 将元素分配到两个数组中 I

__Description:__

You are given a __1-indexed__ array of __distinct__ integers `nums` of length `n`.

You need to distribute all the elements of `nums` between two arrays `arr1` and `arr2` using `n` operations. In the first operation, append `nums[1]` to `arr1`. In the second operation, append `nums[2]` to `arr2`. Afterwards, in the `i ^ th` operation:

- If the last element of `arr1` is __greater__ than the last element of `arr2`, append `nums[i]` to `arr1`. Otherwise, append `nums[i]` to `arr2`.

The array `result` is formed by concatenating the arrays `arr1` and `arr2`. For example, if `arr1 == [1,2,3]` and `arr2 == [4,5,6]`, then `result = [1,2,3,4,5,6]`.

Return _the array_ `result`.

__Example:__

Example 1:

```text
Input: nums = [2,1,3]
Output: [2,3,1]
Explanation: After the first 2 operations, arr1 = [2] and arr2 = [1].
In the 3rd operation, as the last element of arr1 is greater than the last element of arr2 (2 > 1), append nums[3] to arr1.
After 3 operations, arr1 = [2,3] and arr2 = [1].
Hence, the array result formed by concatenation is [2,3,1].
```

Example 2:

```text
Input: nums = [5,4,3,8]
Output: [5,3,4,8]
Explanation: After the first 2 operations, arr1 = [5] and arr2 = [4].
In the 3rd operation, as the last element of arr1 is greater than the last element of arr2 (5 > 4), append nums[3] to arr1, hence arr1 becomes [5,3].
In the 4th operation, as the last element of arr2 is greater than the last element of arr1 (4 > 3), append nums[4] to arr2, hence arr2 becomes [4,8].
After 4 operations, arr1 = [5,3] and arr2 = [4,8].
Hence, the array result formed by concatenation is [5,3,4,8].
```

__Constraints:__

- `3 <= n <= 50`
- `1 <= nums[i] <= 100`
- All elements in `nums` are distinct.

__题目描述:__

给你一个下标从 __1__ 开始、包含 __不同__ 整数的数组 `nums` ，数组长度为 `n` 。

你需要通过 `n` 次操作，将 `nums` 中的所有元素分配到两个数组 `arr1` 和 `arr2` 中。在第一次操作中，将 `nums[1]` 追加到 `arr1` 。在第二次操作中，将 `nums[2]` 追加到 `arr2` 。之后，在第 `i` 次操作中:

- 如果 `arr1` 的最后一个元素 __大于__ `arr2` 的最后一个元素，就将 `nums[i]` 追加到 `arr1` 。否则，将 `nums[i]` 追加到 `arr2` 。

通过连接数组 `arr1` 和 `arr2` 形成数组 `result` 。例如，如果 `arr1 == [1,2,3]` 且 `arr2 == [4,5,6]` ，那么 `result = [1,2,3,4,5,6]` 。

返回数组 `result` 。

__示例:__

示例 1：

```text
输入：nums = [2,1,3]
输出：[2,3,1]
解释：在前两次操作后，arr1 = [2] ，arr2 = [1] 。
在第 3 次操作中，由于 arr1 的最后一个元素大于 arr2 的最后一个元素（2 > 1），将 nums[3] 追加到 arr1 。
3 次操作后，arr1 = [2,3] ，arr2 = [1] 。
因此，连接形成的数组 result 是 [2,3,1] 。
```

示例 2：

```text
输入：nums = [5,4,3,8]
输出：[5,3,4,8]
解释：在前两次操作后，arr1 = [5] ，arr2 = [4] 。
在第 3 次操作中，由于 arr1 的最后一个元素大于 arr2 的最后一个元素（5 > 4），将 nums[3] 追加到 arr1 ，因此 arr1 变为 [5,3] 。
在第 4 次操作中，由于 arr2 的最后一个元素大于 arr1 的最后一个元素（4 > 3），将 nums[4] 追加到 arr2 ，因此 arr2 变为 [4,8] 。
4 次操作后，arr1 = [5,3] ，arr2 = [4,8] 。
因此，连接形成的数组 result 是 [5,3,4,8] 。
```

__提示：__

- `3 <= n <= 50`
- `1 <= nums[i] <= 100`
- `nums`中的所有元素都互不相同。

__思路:__

```text
模拟
按照题意将 nums 中的元素加入另外两个数组
然后再合并两个数组
时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    vector<int> resultArray(vector<int>& nums) 
    {
        vector<int> a{ nums[0] }, b{ nums[1] };
        for (int i = 2, n = nums.size(); i < n; i++) (a.back() > b.back() ? a : b).emplace_back(nums[i]);
        a.insert(a.end(), b.begin(), b.end());
        return a;
    }
};
```

__Java__:

```Java
class Solution {
    public int[] resultArray(int[] nums) {
        List<Integer> a = new ArrayList<>(){{ add(nums[0]); }}, b = new ArrayList<>(){{ add(nums[1]); }};
        for (int i = 2, n = nums.length; i < n; i++) {
            if (a.get(a.size() - 1) > b.get(b.size() - 1)) a.add(nums[i]);
            else b.add(nums[i]);
        }
        a.addAll(b);
        return a.stream().mapToInt(i -> i).toArray();
    }
}
```

__Python__:

```Python
class Solution:
    def resultArray(self, nums: List[int]) -> List[int]:
        a, b = nums[:1], nums[1:2]
        for num in nums[2:]:
            if a[-1] > b[-1]:
                a.append(num)
            else:
                b.append(num)
        return a + b
```
