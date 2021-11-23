# 923 3Sum With Multiplicity 三数之和的多种可能

__Description__:
Given an integer array arr, and an integer target, return the number of tuples i, j, k such that i < j < k and arr[i] + arr[j] + arr[k] == target.

As the answer can be very large, return it modulo 10^9 + 7.

__Example:__

Example 1:

Input: arr = [1,1,2,2,3,3,4,4,5,5], target = 8
Output: 20
Explanation:
Enumerating by the values (arr[i], arr[j], arr[k]):
(1, 2, 5) occurs 8 times;
(1, 3, 4) occurs 8 times;
(2, 2, 4) occurs 2 times;
(2, 3, 3) occurs 2 times.

Example 2:

Input: arr = [1,1,2,2,2,2], target = 5
Output: 12
Explanation:
arr[i] = 1, arr[j] = arr[k] = 2 occurs 12 times:
We choose one 1 from [1,1] in 2 ways,
and two 2s from [2,2,2,2] in 6 ways.

__Constraints:__

3 <= arr.length <= 3000
0 <= arr[i] <= 100
0 <= target <= 300

__题目描述__:
给定一个整数数组 A，以及一个整数 target 作为目标值，返回满足 i < j < k 且 A[i] + A[j] + A[k] == target 的元组 i, j, k 的数量。

由于结果会非常大，请返回 结果除以 10^9 + 7 的余数。

__示例 :__

示例 1：

输入：A = [1,1,2,2,3,3,4,4,5,5], target = 8
输出：20
解释：
按值枚举（A[i]，A[j]，A[k]）：
(1, 2, 5) 出现 8 次；
(1, 3, 4) 出现 8 次；
(2, 2, 4) 出现 2 次；
(2, 3, 3) 出现 2 次。

示例 2：

输入：A = [1,1,2,2,2,2], target = 5
输出：12
解释：
A[i] = 1，A[j] = A[k] = 2 出现 12 次：
我们从 [1,1] 中选择一个 1，有 2 种情况，
从 [2,2,2,2] 中选出两个 2，有 6 种情况。

__提示:__

3 <= A.length <= 3000
0 <= A[i] <= 100
0 <= target <= 300

__思路__:

双指针
用两个指针分别查询两数之和等于 target - element[i] 的元素个数
如果 l == r == i, 三个数字相同结果数为 C(m, 3), 其中 m 为数字的个数
如果 l == r, 或者 l == i, 两个数字相同结果数为 C(p, 2) * q, p 为相同的数字的个数, q 为剩下的那个数字的个数
否则结果数为 abc, a, b, c 分别为三个数字的个数
时间复杂度为 O(n + m ^ 2), 空间复杂度为 O(m), arr 中最大的数字为 m, 此题为 100, 时间复杂度可看作 O(n)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int threeSumMulti(vector<int>& arr, int target) 
    {
        long result = 0, mod = 1e9 + 7;
        vector<long> count(101, 0), element;
        for (const auto& num : arr) ++count[num];
        for (int i = 0; i < 101; i++) if (count[i]) element.emplace_back(i);
        for (int i = 0, n = element.size(); i < n; i++)
        {
            if (target < 3 * element[i]) break;
            int remain = target - element[i], l = i, r = n - 1;
            while (l <= r) 
            {
                if (element[l] + element[r] > remain) --r;
                else if (element[l] + element[r] < remain) ++l;
                else
                {
                    if (l == r and l == i) result += (count[element[i]] - 2) * (count[element[i]] - 1) * count[element[i]] / 6;
                    else if (l == i) result += (count[element[r]] * ((count[element[i]] - 1) * count[element[i]])) >> 1;
                    else if (l == r) result += (((count[element[l]] - 1) * count[element[l]]) >> 1) * count[element[i]];
                    else result += count[element[l]] * count[element[r]] * count[element[i]];
                    ++l;
                    --r;
                }
            }
        }
        return result % mod;
    }
};
```

__Java__:

```Java
class Solution {
    public int threeSumMulti(int[] arr, int target) {
        long result = 0, mod = 1_000_000_007, count[] = new long[101];
        for (int num : arr) ++count[num];
        List<Integer> element = new ArrayList<>();
        for (int i = 0; i < 101; i++) if (count[i] != 0) element.add(i);
        for (int i = 0, n = element.size(); i < n; i++) {
            if (target < 3 * element.get(i)) break;
            int remain = target - element.get(i), l = i, r = n - 1;
            while (l <= r) {
                if (element.get(l) + element.get(r) > remain) --r;
                else if (element.get(l) + element.get(r) < remain) ++l;
                else {
                    if (l == r && l == i) result += (count[element.get(i)] - 2) * (count[element.get(i)] - 1) * count[element.get(i)] / 6;
                    else if (l == i) result += (count[element.get(r)] * ((count[element.get(i)] - 1) * count[element.get(i)])) >>> 1;
                    else if (l == r) result += (((count[element.get(r)] - 1) * count[element.get(r)]) >>> 1) * count[element.get(i)];
                    else result += count[element.get(l)] * count[element.get(r)] * count[element.get(i)];
                    ++l;
                    --r;
                }
            }
        }
        return (int)(result % mod);
    }
}
```

__Python__:

```Python
class Solution:
    def threeSumMulti(self, arr: List[int], target: int) -> int:
        result, n, mod = 0, len(element := sorted((c := Counter(arr)).keys())), 10 ** 9 + 7
        for i in range(n):
            if target < 3 * element[i]: 
                break
            remain, l, r = target - element[i], i, n - 1
            while l <= r:
                if element[l] + element[r] > remain: 
                    r -= 1
                elif element[l] + element[r] < remain: 
                    l += 1
                else:
                    if l == r == i:
                        result += (c[element[i]] - 2) * (c[element[i]] - 1) * c[element[i]] // 6
                    elif l == i:
                        result += (c[element[r]] * ((c[element[i]] - 1) * c[element[i]])) >> 1
                    elif l == r:
                        result += (((c[element[l]] - 1) * c[element[l]]) >> 1) * c[element[i]]
                    else:
                        result += c[element[l]] * c[element[r]] * c[element[i]]
                    l, r = l + 1, r - 1
        return result % mod
```
