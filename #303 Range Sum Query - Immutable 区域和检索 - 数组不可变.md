# 303 Range Sum Query - Immutable 区域和检索 - 数组不可变

__Description__:
Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

**Example :**

```text
Given nums = [-2, 0, 3, -5, 2, -1]

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

__Note:__
You may assume that the array does not change.
There are many calls to sumRange function.

__题目描述__:
给定一个整数数组  nums，求出数组从索引 i 到 j  (i ≤ j) 范围内元素的总和，包含 i,  j 两点。

**示例 :**

```text
给定 nums = [-2, 0, 3, -5, 2, -1]，求和函数为 sumRange()

sumRange(0, 2) -> 1
sumRange(2, 5) -> -1
sumRange(0, 5) -> -3
```

__说明:__

你可以假设数组不可变。
会多次调用 sumRange 方法。

__思路__:

动态规划(前缀和), 记录下从 0开始到 i的数组之和
调用时返回 sum_[j] - sum_[i] + nums[i]即可
初始化类时间复杂度O(n), 空间复杂度O(n)
查询时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class NumArray 
{
private:
    vector<int> nums;
    vector<int> sum_;
public:
    NumArray(vector<int>& nums) 
    {
        this -> nums = nums;
        int temp = 0;
        for (int i = 0; i < nums.size(); i++) 
        {
            temp += nums[i];
            sum_.push_back(temp);
        }
    }

    int sumRange(int i, int j) 
    {
        return sum_[j] - sum_[i] + nums[i];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(i,j);
 */
```

__Java__:

```Java
class NumArray {

    private int[] sum_;
    private int[] nums;
    public NumArray(int[] nums) {
        sum_ = new int[nums.length];
        int temp = 0;
        for (int i = 0; i < nums.length; i++) {
            temp += nums[i];
            sum_[i] = temp;
        }
        this.nums = nums;
    }

    public int sumRange(int i, int j) {
        return sum_[j] - sum_[i] + nums[i];
    }
}

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray obj = new NumArray(nums);
 * int param_1 = obj.sumRange(i,j);
 */
```

__Python__:

```Python
class NumArray:

    def __init__(self, nums: List[int]):
        self.nums = nums
        temp = 0
        sum_ = []
        for i in nums:
            temp += i
            sum_.append(temp)
        self.sum_ = sum_

    def sumRange(self, i: int, j: int) -> int:
        return self.sum_[j] - self.sum_[i] + self.nums[i]


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(i,j)
```
