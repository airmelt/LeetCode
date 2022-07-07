# 1186 Maximum Subarray Sum with One Deletion 删除一次得到子数组最大和

__Description__:
Given an array of integers, return the maximum sum for a non-empty subarray (contiguous elements) with at most one element deletion. In other words, you want to choose a subarray and optionally delete one element from it so that there is still at least one element left and the sum of the remaining elements is maximum possible.

Note that the subarray needs to be non-empty after deleting one element.

__Example:__

Example 1:

Input: arr = [1,-2,0,3]
Output: 4
Explanation: Because we can choose [1, -2, 0, 3] and drop -2, thus the subarray [1, 0, 3] becomes the maximum value.

Example 2:

Input: arr = [1,-2,-2,3]
Output: 3
Explanation: We just choose [3] and it's the maximum sum.

Example 3:

Input: arr = [-1,-1,-1,-1]
Output: -1
Explanation: The final subarray needs to be non-empty. You can't choose [-1] and delete -1 from it, then get an empty subarray to make the sum equals to 0.

__Constraints:__

1 <= arr.length <= 10^5
-104 <= arr[i] <= 10^4

__题目描述__:
给你一个整数数组，返回它的某个 非空 子数组（连续元素）在执行一次可选的删除操作后，所能得到的最大元素总和。换句话说，你可以从原数组中选出一个子数组，并可以决定要不要从中删除一个元素（只能删一次哦），（删除后）子数组中至少应当有一个元素，然后该子数组（剩下）的元素总和是所有子数组之中最大的。

注意，删除一个元素后，子数组 不能为空。

__示例 :__

示例 1：

输入：arr = [1,-2,0,3]
输出：4
解释：我们可以选出 [1, -2, 0, 3]，然后删掉 -2，这样得到 [1, 0, 3]，和最大。

示例 2：

输入：arr = [1,-2,-2,3]
输出：3
解释：我们直接选出 [3]，这就是最大和。

示例 3：

输入：arr = [-1,-1,-1,-1]
输出：-1
解释：最后得到的子数组不能为空，所以我们不能选择 [-1] 并从中删去 -1 来得到 0。
     我们应该直接选择 [-1]，或者选择 [-1, -1] 再从中删去一个 -1。

__提示:__

1 <= arr.length <= 105
-10^4 <= arr[i] <= 10^4

__思路__:

动态规划
zero 表示不删除数字得到的最大子数组和, one 表示删除 1 个数字得到的最大子数组和
初始化将 zero[0], one[0] 均赋值为 arr[0], 因为至少需要选择一个数组中的元素
zero[i] = max(zero[i - 1] + arr[i], arr[i])
one[i] = max(zero[i - 1], one[i - 1] + arr[i]), 要么删除当前元素, 要么选择前一个已经删除过的子数组
返回两个的数组最大值
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int maximumSum(vector<int>& arr) 
    {
        int n = arr.size(), result = arr.front();
        vector<int> zero(n), one(n);
        zero.front() = one.front() = arr.front();
        for (int i = 1; i < n; i++) 
        {
            zero[i] = max(arr[i], zero[i - 1] + arr[i]);
            one[i] = max(zero[i - 1], one[i - 1] + arr[i]);
            result = max({result, zero[i], one[i]});
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int maximumSum(int[] arr) {
        int n = arr.length, zero[] = new int[n], one[] = new int[n], result = arr[0];
        zero[0] = one[0] = arr[0];
        for (int i = 1; i < n; i++) {
            zero[i] = Math.max(arr[i], zero[i - 1] + arr[i]);
            one[i] = Math.max(zero[i - 1], one[i - 1] + arr[i]);
            result = Math.max(result, Math.max(zero[i], one[i]));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def maximumSum(self, arr: List[int]) -> int:
        zero, one = [arr[0]] * (n := len(arr)), [arr[0]] * n
        for i in range(1, n):
            zero[i], one[i] = max(arr[i], zero[i - 1] + arr[i]), max(zero[i - 1], one[i - 1] + arr[i])
        return max(max(zero), max(one))
```
