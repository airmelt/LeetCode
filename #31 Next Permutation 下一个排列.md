__Description__:
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.

If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).

The replacement must be in-place and use only constant extra memory.

__Example:__
Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

__题目描述__:
实现获取下一个排列的函数，算法需要将给定数字序列重新排列成字典序中下一个更大的排列。

如果不存在下一个更大的排列，则将数字重新排列成最小的排列（即升序排列）。

必须原地修改，只允许使用额外常数空间。

__示例 :__
以下是一些例子，输入位于左侧列，其相应输出位于右侧列。
```
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
```

__思路__:
```
123465 -> 123456 -> 123546
```
总体思路是找到一个尽可能大的小数和它后面的一个尽可能小的大数进行交换(如上面例子中的 4和5), 所以应该从右向左查找, 逆序再交换
从数组的最后往前查找, 找到第一个减少的下标(这个下标对应的应该是一个尽可能大的较小的数, 用来与后面尽可能小的大数进行交换), 则该下标之后的所有数字都应该是递减的, 将这个下标之后的数字全部逆序, 得到一个部分正序的数组, 再在这个下标之后找到第一个比它大的数交换即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```C++
class Solution 
{
public:
    void nextPermutation(vector<int>& nums) 
    {
        next_permutation(nums.begin(), nums.end());
    }
};
```

__Java__:
```Java
class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 1;
        while (i > 0 && nums[i] <= nums[i - 1]) --i; 
        int a = i, b = nums.length - 1;
        while (a < b) {
            nums[a] ^= nums[b];
            nums[b] ^= nums[a];
            nums[a++] ^= nums[b--];
        }
        int j = i - 1;
        if (j < 0) return;
        for (int index = i; index < nums.length; index++) {
            if (nums[index] > nums[j]) {
                nums[index] ^= nums[j];
                nums[j] ^= nums[index];
                nums[index] ^= nums[j];
                break;
            }
        }
    }
}
```

__Python__:
```Python
class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = len(nums) - 1
        while i and nums[i] <= nums[i - 1]:
            i -= 1
        a, b = i, len(nums) - 1
        while a < b:
            nums[a], nums[b] = nums[b], nums[a]
            a += 1
            b -= 1
        j = i - 1
        for index in range(i, len(nums)):
            if nums[index] > nums[j]:
                nums[j], nums[index] = nums[index], nums[j]
                break
```