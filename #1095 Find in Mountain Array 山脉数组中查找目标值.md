# 1095 Find in Mountain Array 山脉数组中查找目标值

__Description__:
(This problem is an interactive problem.)

You may recall that an array arr is a mountain array if and only if:

arr.length >= 3
There exists some i with 0 < i < arr.length - 1 such that:
arr[0] < arr[1] < ... < arr[i - 1] < arr[i]
arr[i] > arr[i + 1] > ... > arr[arr.length - 1]
Given a mountain array mountainArr, return the minimum index such that mountainArr.get(index) == target. If such an index does not exist, return -1.

You cannot access the mountain array directly. You may only access the array using a MountainArray interface:

MountainArray.get(k) returns the element of the array at index k (0-indexed).
MountainArray.length() returns the length of the array.
Submissions making more than 100 calls to MountainArray.get will be judged Wrong Answer. Also, any solutions that attempt to circumvent the judge will result in disqualification.

__Example:__

Example 1:

Input: array = [1,2,3,4,5,3,1], target = 3
Output: 2
Explanation: 3 exists in the array, at index=2 and index=5. Return the minimum index, which is 2.

Example 2:

Input: array = [0,1,2,4,2,1], target = 3
Output: -1
Explanation: 3 does not exist in the array, so we return -1.

__Constraints:__

3 <= mountain_arr.length() <= 10^4
0 <= target <= 10^9
0 <= mountain_arr.get(index) <= 10^9

__题目描述__:
（这是一个 交互式问题 ）

给你一个 山脉数组 mountainArr，请你返回能够使得 mountainArr.get(index) 等于 target 最小 的下标 index 值。

如果不存在这样的下标 index，就请返回 -1。

何为山脉数组？如果数组 A 是一个山脉数组的话，那它满足如下条件：

首先，A.length >= 3

其次，在 0 < i < A.length - 1 条件下，存在 i 使得：

A[0] < A[1] < ... A[i-1] < A[i]
A[i] > A[i+1] > ... > A[A.length - 1]

你将 不能直接访问该山脉数组，必须通过 MountainArray 接口来获取数据：

MountainArray.get(k) - 会返回数组中索引为k 的元素（下标从 0 开始）
MountainArray.length() - 会返回该数组的长度

__注意：__

对 MountainArray.get 发起超过 100 次调用的提交将被视为错误答案。此外，任何试图规避判题系统的解决方案都将会导致比赛资格被取消。

为了帮助大家更好地理解交互式问题，我们准备了一个样例 [“答案”：](https://leetcode-cn.com/playground/RKhe3ave)，请注意这 不是一个正确答案。

__示例 :__

示例 1：

输入：array = [1,2,3,4,5,3,1], target = 3
输出：2
解释：3 在数组中出现了两次，下标分别为 2 和 5，我们返回最小的下标 2。

示例 2：

输入：array = [0,1,2,4,2,1], target = 3
输出：-1
解释：3 在数组中没有出现，返回 -1。

__提示:__

3 <= mountain_arr.length() <= 10000
0 <= target <= 10^9
0 <= mountain_arr.get(index) <= 10^9

__思路__:

二分查找
首先找到山脉数组的峰值, 峰值一定比其前一个数大
然后将山脉数组分为两个半区, 左半区单调递增, 右半区单调递减
分别在两个半区中二分搜索 target, 返回对应下标, 搜索失败返回 -1
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
/**
 * // This is the MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * class MountainArray {
 *   public:
 *     int get(int index);
 *     int length();
 * };
 */

class Solution 
{
private:
    MountainArray* arr;
    unordered_map<int, int> cache;
    
    int bisearch(int l, int r, int target, bool flag)
    {
        while (l < r) 
        {
            int mid = ((l + r) >> 1);
            if (get(mid) * (flag ? 1 : -1) >= target * (flag ? 1 : -1)) r = mid;
            else l = mid + 1;
        }
        return l;
    }
    
    int get(int mid) 
    {
        return cache.count(mid) ? cache[mid] : cache[mid] = arr -> get(mid);
    }
public:
    int findInMountainArray(int target, MountainArray &mountainArr) 
    {
        int n = mountainArr.length(), l = 0, r = n - 1, peek = 0, mid = 0;
        arr = &mountainArr;
        while (l < r) 
        {
            mid = ((l + r + 1) >> 1);
            if (get(mid) > get(mid - 1)) l = mid;
            else r = mid - 1;
        }
        peek = l;
        if (get(l = bisearch(0, peek, target, 1)) == target) return l;
        return get(l = bisearch(peek, n - 1, target, 0)) == target ? l : -1;
    }
};
```

__Java__:

```Java
/**
 * // This is MountainArray's API interface.
 * // You should not implement it, or speculate about its implementation
 * interface MountainArray {
 *     public int get(int index) {}
 *     public int length() {}
 * }
 */
 
class Solution {
    private Map<Integer, Integer> cache;
    private MountainArray arr;
    
    public int findInMountainArray(int target, MountainArray mountainArr) {
        int n = mountainArr.length(), l = 0, r = n - 1, peek = 0, mid = 0;
        cache = new HashMap<>();
        arr = mountainArr;
        while (l < r) {
            mid = ((l + r + 1) >>> 1);
            if (get(mid) > get(mid - 1)) l = mid;
            else r = mid - 1;
        }
        peek = r = l;
        l = 0;
        while (l < r) {
            mid = ((l + r) >>> 1);
            if (get(mid) >= target) r = mid;
            else l = mid + 1;
        }
        if (get(l) == target) return l;
        l = peek;
        r = n - 1;
        while (l < r) {
            mid = ((l + r) >>> 1);
            if (get(mid) <= target) r = mid;
            else l = mid + 1;
        }
        return get(l) == target ? l : -1;
    }
    
    private int get(int mid) {
        if (!cache.containsKey(mid)) cache.put(mid, arr.get(mid));
        return cache.get(mid);
    }
}
```

__Python__:

```Python
# """
# This is MountainArray's API interface.
# You should not implement it, or speculate about its implementation
# """
#class MountainArray:
#    def get(self, index: int) -> int:
#    def length(self) -> int:

class Solution:
    def findInMountainArray(self, target: int, mountain_arr: 'MountainArray') -> int:
        l, r, cache = 0, (n := mountain_arr.length()) - 1, {}
        
        def get(mid: int) -> int:
            if not mid in cache:
                cache[mid] = mountain_arr.get(mid)
            return cache[mid]
        
        while l < r:
            mid = (l + r + 1) >> 1
            if get(mid) > get(mid - 1):
                l = mid
            else:
                r = mid - 1
        peek, l, r = l, 0, l
        while l < r:
            mid = (l + r) >> 1
            if get(mid) >= target:
                r = mid
            else:
                l = mid + 1
        if get(l) == target:
            return l
        l, r = l, n - 1
        while l < r:
            mid = (l + r) >> 1
            if get(mid) <= target:
                r = mid
            else:
                l = mid + 1 
        return l if get(l) == target else -1
```
