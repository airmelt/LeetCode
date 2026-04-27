# 1726 Tuple with Same Product 同积元组

__Description:__

Given an array `nums` of __distinct__ positive integers, return _the number of tuples_ `(a, b, c, d)` _such that_ `a * b = c * d` _where_ `a`_,_ `b`_,_ `c`_, and_ `d` _are elements of_ `nums`_, and_ `a != b != c != d`_._

__Example:__

Example 1:

```text
Input: nums = [2,3,4,6]
Output: 8
Explanation: There are 8 valid tuples:
(2,6,3,4) , (2,6,4,3) , (6,2,3,4) , (6,2,4,3)
(3,4,2,6) , (4,3,2,6) , (3,4,6,2) , (4,3,6,2)
```

Example 2:

```text
Input: nums = [1,2,4,5,10]
Output: 16
Explanation: There are 16 valid tuples:
(1,10,2,5) , (1,10,5,2) , (10,1,2,5) , (10,1,5,2)
(2,5,1,10) , (2,5,10,1) , (5,2,1,10) , (5,2,10,1)
(2,10,4,5) , (2,10,5,4) , (10,2,4,5) , (10,2,5,4)
(4,5,2,10) , (4,5,10,2) , (5,4,2,10) , (5,4,10,2)
```

__Constraints:__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 4`
- All elements in `nums` are __distinct__.

__题目描述:__

给你一个由 __不同__ 正整数组成的数组 `nums` ，请你返回满足 `a * b = c * d` 的元组 `(a, b, c, d)` 的数量。其中 `a`、 `b`、 `c` 和 `d` 都是 `nums` 中的元素，且 `a != b != c != d` 。

__示例:__

示例 1：

```text
输入：nums = [2,3,4,6]
输出：8
解释：存在 8 个满足题意的元组：
(2,6,3,4) , (2,6,4,3) , (6,2,3,4) , (6,2,4,3)
(3,4,2,6) , (4,3,2,6) , (3,4,6,2) , (4,3,6,2)
```

示例 2：

```text
输入：nums = [1,2,4,5,10]
输出：16
解释：存在 16 个满足题意的元组：
(1,10,2,5) , (1,10,5,2) , (10,1,2,5) , (10,1,5,2)
(2,5,1,10) , (2,5,10,1) , (5,2,1,10) , (5,2,10,1)
(2,10,4,5) , (2,10,5,4) , (10,2,4,5) , (10,2,4,5)
(4,5,2,10) , (4,5,10,2) , (5,4,2,10) , (5,4,10,2)
```

__提示：__

- `1 <= nums.length <= 1000`
- `1 <= nums[i] <= 10 ^ 4`
- `nums` 中的所有元素 __互不相同__

__思路:__

```text
哈希表
只需要记录 nums[i] * nums[j] 的个数即可
对所有的个数进行累加, 最后乘以 8 即可, 每个乘积对可以排列组合成 8 组元组
累加可以利用等差数列求和
时间复杂度为 O(N ^ 2), 空间复杂度为 O(N ^ 2)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int tupleSameProduct(vector<int>& nums) 
    {
        unordered_map<int, int> m;
        int n = nums.size(), result = 0;
        for (int i = 0; i < n; i++) for (int j = 0; j < i; j++) result += m[nums[i] * nums[j]]++;
        return result << 3;
    }
};
```

__Java__:

```Java
class Solution {
    public int tupleSameProduct(int[] nums) {
        Map<Integer, Integer> map = new HashMap<>();
        int n = nums.length, result = 0;
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                int value = nums[i] * nums[j], cur = map.getOrDefault(nums[i] * nums[j], 0);
                map.put(value, cur + 1);
                result += cur;
            }
        }
        return result << 3;
    }
}
```

__Python__:

```Python
class Solution:
    def tupleSameProduct(self, nums: List[int]) -> int:
        return sum(v * (v - 1) >> 1 for v in Counter(nums[i] * nums[j] for i in range(len(nums)) for j in range(i)).values()) << 3
```
