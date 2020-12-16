__Description__:
Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.

__Note:__
The array size can be very large. Solution that uses too much extra space will not pass the judge.

__Example:__

int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(3);

// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);

__题目描述__:
给定一个可能含有重复元素的整数数组，要求随机输出给定的数字的索引。 您可以假设给定的数字一定存在于数组中。

__注意:__
数组大小可能非常大。 使用太多额外空间的解决方案将不会通过测试。

__示例 :__

int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) 应该返回索引 2,3 或者 4。每个索引的返回概率应该相等。
solution.pick(3);

// pick(1) 应该返回 0。因为只有nums[0]等于1。
solution.pick(1);

__思路__:
蓄水池取样
遍历数组, 每次查找到 target, 记录出现的次数
选择一个 0-出现次数的随机数, 如果选到 0, 就交换结果和数组下标
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
private:
    vector<int> arr;
public:
    Solution(vector<int>& nums) 
    {
        arr = nums;
    }
    
    int pick(int target) 
    {
        int count = 0, result = 0, n = arr.size();
        for (int i = 0; i < n; i++)
        {
            if (arr[i] == target)
            {
                ++count;
                if (!(rand() % count)) result = i;
            }
        }
        return result;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(nums);
 * int param_1 = obj->pick(target);
 */
```

__Java__:
```Java
class Solution {

    private int[] nums;
    private Random random;
    
    public Solution(int[] nums) {
        this.nums = nums;
        this.random = new Random();
    }
    
    public int pick(int target) {
        int point = 0, result = 0, n = nums.length;
        for (int i = 0; i < n; i++) {
            if (nums[i] == target) {
                ++point;
                if (random.nextInt(point) % point == 0) result = i;
            }
        }
        return result;
    }
}

/**
 * Your Solution object will be instantiated and called as such:
 * Solution obj = new Solution(nums);
 * int param_1 = obj.pick(target);
 */
```

__Python__:
```Python
class Solution:

    def __init__(self, nums: List[int]):
        self.nums = nums
        

    def pick(self, target: int) -> int:
        return random.choice([i for i in range(len(self.nums)) if self.nums[i] == target])
        


# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.pick(target)
```