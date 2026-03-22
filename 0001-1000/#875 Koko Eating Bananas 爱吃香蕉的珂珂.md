# 875 Koko Eating Bananas 爱吃香蕉的珂珂

__Description__:
Koko loves to eat bananas. There are n piles of bananas, the ith pile has piles[i] bananas. The guards have gone and will come back in h hours.

Koko can decide her bananas-per-hour eating speed of k. Each hour, she chooses some pile of bananas and eats k bananas from that pile. If the pile has less than k bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return the minimum integer k such that she can eat all the bananas within h hours.

__Example:__

Example 1:

Input: piles = [3,6,7,11], h = 8
Output: 4

Example 2:

Input: piles = [30,11,23,4,20], h = 5
Output: 30

Example 3:

Input: piles = [30,11,23,4,20], h = 6
Output: 23

__Constraints:__

1 <= piles.length <= 10^4
piles.length <= h <= 10^9
1 <= piles[i] <= 10^9

__题目描述__:
珂珂喜欢吃香蕉。这里有 N 堆香蕉，第 i 堆中有 piles[i] 根香蕉。警卫已经离开了，将在 H 小时后回来。

珂珂可以决定她吃香蕉的速度 K （单位：根/小时）。每个小时，她将会选择一堆香蕉，从中吃掉 K 根。如果这堆香蕉少于 K 根，她将吃掉这堆的所有香蕉，然后这一小时内不会再吃更多的香蕉。  

珂珂喜欢慢慢吃，但仍然想在警卫回来前吃掉所有的香蕉。

返回她可以在 H 小时内吃掉所有香蕉的最小速度 K（K 为整数）。

__示例 :__

示例 1：

输入: piles = [3,6,7,11], H = 8
输出: 4

示例 2：

输入: piles = [30,11,23,4,20], H = 5
输出: 30

示例 3：

输入: piles = [30,11,23,4,20], H = 6
输出: 23

__提示:__

1 <= piles.length <= 10^4
piles.length <= H <= 10^9
1 <= piles[i] <= 10^9

__思路__:

二分法
左边界为 1, 右边界为 piles 数组最大值
检查如果超过 h 时间说明左边界太小令 left = mid + 1
否则说明右边界太大令 right = mid
最后返回 left
时间复杂度为 O(nlg(m)), 空间复杂度为 O(1), 其中 m 为数组中的最大值

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int minEatingSpeed(vector<int>& piles, int h) 
    {
        int left = 1, right = *max_element(piles.begin(), piles.end());
        while (left < right) 
        {
            int mid = left + ((right - left) >> 1);
            if (check(mid, piles, h)) left = mid + 1;
            else right = mid;
        }
        return left;
    }
private:
    bool check(int mid, vector<int>& piles, int h)
    {
        int s = 0;
        for (int pile : piles) s += pile / mid + (pile % mid != 0);
        return s > h;
    }
};
```

__Java__:

```Java
class Solution {
    public int minEatingSpeed(int[] piles, int h) {
        int left = 1, right = 1;
        for (int pile : piles) right = Math.max(pile, right);
        while (left < right) {
            int mid = left + ((right - left) >>> 1);
            if (check(mid, piles, h)) left = mid + 1;
            else right = mid;
        }
        return left;
    }
    
    private boolean check(int mid, int[] piles, int h) {
        int s = 0;
        for (int pile : piles) s += pile / mid + (pile % mid == 0 ? 0 : 1);
        return s > h;
    }
}
```

__Python__:

```Python
class Solution:
    def minEatingSpeed(self, piles: List[int], h: int) -> int:
        left, right = 1, max(piles)
        while left < right:
            mid = left + ((right - left) >> 1)
            if sum(pile // mid + (not not pile % mid) for pile in piles) > h:
                left = mid + 1
            else:
                right = mid
        return left
```
