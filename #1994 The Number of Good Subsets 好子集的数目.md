# 1994 The Number of Good Subsets 好子集的数目

__Description:__

You are given an integer array `nums`. We call a subset of `nums` __good__ if its product can be represented as a product of one or more __distinct prime__ numbers.

- For example, if `nums = [1, 2, 3, 4]`:

	
		- `[2, 3]`, `[1, 2, 3]`, and `[1, 3]` are __good__ subsets with products `6 = 2*3`, `6 = 2*3`, and `3 = 3` respectively.
		- `[1, 4]` and `[4]` are not __good__ subsets with products `4 = 2*2` and `4 = 2*2` respectively.
- `[2, 3]`, `[1, 2, 3]`, and `[1, 3]` are __good__ subsets with products `6 = 2*3`, `6 = 2*3`, and `3 = 3` respectively.
- `[1, 4]` and `[4]` are not __good__ subsets with products `4 = 2*2` and `4 = 2*2` respectively.

- `[2, 3]`, `[1, 2, 3]`, and `[1, 3]` are __good__ subsets with products `6 = 2*3`, `6 = 2*3`, and `3 = 3` respectively.
- `[1, 4]` and `[4]` are not __good__ subsets with products `4 = 2*2` and `4 = 2*2` respectively.

Return _the number of different __good__ subsets in_ `nums` ___modulo___ `10 ^ 9 + 7`.

A __subset__ of `nums` is any array that can be obtained by deleting some (possibly none or all) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

__Example:__

Example 1:

```text
Input: nums = [1,2,3,4]
Output: 6
Explanation: The good subsets are:
- [1,2]: product is 2, which is the product of distinct prime 2.
- [1,2,3]: product is 6, which is the product of distinct primes 2 and 3.
- [1,3]: product is 3, which is the product of distinct prime 3.
- [2]: product is 2, which is the product of distinct prime 2.
- [2,3]: product is 6, which is the product of distinct primes 2 and 3.
- [3]: product is 3, which is the product of distinct prime 3.
```

Example 2:

```text
Input: nums = [4,2,3,15]
Output: 5
Explanation: The good subsets are:
- [2]: product is 2, which is the product of distinct prime 2.
- [2,3]: product is 6, which is the product of distinct primes 2 and 3.
- [2,15]: product is 30, which is the product of distinct primes 2, 3, and 5.
- [3]: product is 3, which is the product of distinct prime 3.
- [15]: product is 15, which is the product of distinct primes 3 and 5.
```

__Constraints:__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 30`

__题目描述:__

给你一个整数数组 `nums` 。如果 `nums` 的一个子集中，所有元素的乘积可以表示为一个或多个 __互不相同的质数__ 的乘积，那么我们称它为 __好子集__ 。

- 比方说，如果 `nums = [1, 2, 3, 4]` :

	
		- `[2, 3]` ， `[1, 2, 3]` 和 `[1, 3]` 是 __好__ 子集，乘积分别为 `6 = 2*3` ， `6 = 2*3` 和 `3 = 3` 。
		- `[1, 4]` 和 `[4]` 不是 __好__ 子集，因为乘积分别为 `4 = 2*2` 和 `4 = 2*2` 。
- `[2, 3]` ， `[1, 2, 3]` 和 `[1, 3]` 是 __好__ 子集，乘积分别为 `6 = 2*3` ， `6 = 2*3` 和 `3 = 3` 。
- `[1, 4]` 和 `[4]` 不是 __好__ 子集，因为乘积分别为 `4 = 2*2` 和 `4 = 2*2` 。

- `[2, 3]` ， `[1, 2, 3]` 和 `[1, 3]` 是 __好__ 子集，乘积分别为 `6 = 2*3` ， `6 = 2*3` 和 `3 = 3` 。
- `[1, 4]` 和 `[4]` 不是 __好__ 子集，因为乘积分别为 `4 = 2*2` 和 `4 = 2*2` 。

请你返回 `nums` 中不同的 __好__ 子集的数目对 `10 ^ 9 + 7` __取余__ 的结果。

`nums` 中的 __子集__ 是通过删除 `nums` 中一些（可能一个都不删除，也可能全部都删除）元素后剩余元素组成的数组。如果两个子集删除的下标不同，那么它们被视为不同的子集。

__示例:__

示例 1：

```text
输入：nums = [1,2,3,4]
输出：6
解释：好子集为：
- [1,2]：乘积为 2 ，可以表示为质数 2 的乘积。
- [1,2,3]：乘积为 6 ，可以表示为互不相同的质数 2 和 3 的乘积。
- [1,3]：乘积为 3 ，可以表示为质数 3 的乘积。
- [2]：乘积为 2 ，可以表示为质数 2 的乘积。
- [2,3]：乘积为 6 ，可以表示为互不相同的质数 2 和 3 的乘积。
- [3]：乘积为 3 ，可以表示为质数 3 的乘积。
```

示例 2：

```text
输入：nums = [4,2,3,15]
输出：5
解释：好子集为：
- [2]：乘积为 2 ，可以表示为质数 2 的乘积。
- [2,3]：乘积为 6 ，可以表示为互不相同质数 2 和 3 的乘积。
- [2,15]：乘积为 30 ，可以表示为互不相同质数 2，3 和 5 的乘积。
- [3]：乘积为 3 ，可以表示为质数 3 的乘积。
- [15]：乘积为 15 ，可以表示为互不相同质数 3 和 5 的乘积。
```

__提示：__

- `1 <= nums.length <= 10 ^ 5`
- `1 <= nums[i] <= 30`

__思路:__

```text

时间复杂度为 O(N), 空间复杂度为 O(N)
```

__代码:__

__C++__:

```C++
class Solution {
public:
    int numberOfGoodSubsets(vector<int>& nums) {

    }
};
```

__Java__:

```Java
class Solution {
    public int numberOfGoodSubsets(int[] nums) {

    }
}
```

__Python__:

```Python
class Solution:
    def numberOfGoodSubsets(self, nums: List[int]) -> int:
```
