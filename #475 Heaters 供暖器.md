# 475 Heaters 供暖器

__Description__:
Winter is coming! Your first job during the contest is to design a standard heater with fixed warm radius to warm all the houses.

Now, you are given positions of houses and heaters on a horizontal line, find out minimum radius of heaters so that all houses could be covered by those heaters.

So, your input will be the positions of houses and heaters seperately, and your expected output will be the minimum radius standard of heaters.

__Note:__

Numbers of houses and heaters you are given are non-negative and will not exceed 25000.
Positions of houses and heaters you are given are non-negative and will not exceed 10^9.
As long as a house is in the heaters' warm radius range, it can be warmed.
All the heaters follow your radius standard and the warm radius will the same.

__Example:__

Example 1:

Input: [1,2,3],[2]
Output: 1
Explanation: The only heater was placed in the position 2, and if we use the radius 1 standard, then all the houses can be warmed.

Example 2:

Input: [1,2,3,4],[1,4]
Output: 1
Explanation: The two heater was placed in the position 1 and 4. We need to use radius 1 standard, then all the houses can be warmed.

__题目描述__:
冬季已经来临。 你的任务是设计一个有固定加热半径的供暖器向所有房屋供暖。

现在，给出位于一条水平线上的房屋和供暖器的位置，找到可以覆盖所有房屋的最小加热半径。

所以，你的输入将会是房屋和供暖器的位置。你将输出供暖器的最小加热半径。

__说明:__

给出的房屋和供暖器的数目是非负数且不会超过 25000。
给出的房屋和供暖器的位置均是非负数且不会超过10^9。
只要房屋位于供暖器的半径内(包括在边缘上)，它就可以得到供暖。
所有供暖器都遵循你的半径标准，加热的半径也一样。

__示例 :__

示例 1:

输入: [1,2,3],[2]
输出: 1
解释: 仅在位置2上有一个供暖器。如果我们将加热半径设为1，那么所有房屋就都能得到供暖。

示例 2:

输入: [1,2,3,4],[1,4]
输出: 1
解释: 在位置1, 4上有两个供暖器。我们需要将加热半径设为1，这样所有房屋就都能得到供暖。

__思路__:

题意是找到离房子最近的加热器的距离的最大值

1. 对加热器数组进行排序, 遍历房子数组, 用二分查找找到离房子最近的加热器, 更新房子和加热器的距离
时间复杂度O(mlgm), 空间复杂度O(1)
2. 对两个数组都进行排序
时间复杂度O(mn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    int findRadius(vector<int>& houses, vector<int>& heaters) 
    {
        sort(houses.begin(), houses.end());
        sort(heaters.begin(), heaters.end());
        // vector<int>::iterator -> auto
        vector<int>::iterator i = heaters.begin(), j = houses.begin();
        int result = 0;
        while (j != houses.end()) 
        {
            while (i != heaters.end() - 1 and abs(*(i + 1) - *j) <= abs(*i - *j)) i++;
            result = max(result, abs(*i - *j));
            j++;
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int findRadius(int[] houses, int[] heaters) {
        int result = 0;
        Arrays.sort(heaters);
        for (int house : houses) result = Math.abs(binarySearch(heaters, house) - house) > result ? Math.abs(binarySearch(heaters, house) - house) : result;
        return result;
    }

    private int binarySearch(int[] heaters, int target) {
        int left = 0, right = heaters.length - 1;
        while (left < right - 1) {
            int mid = left + ((right - left) >> 1);
            if (target == heaters[mid]) return heaters[mid];
            else if (target < heaters[mid]) right = mid;
            else left = mid;
        }
        if (Math.abs(target - heaters[left]) < Math.abs(target - heaters[right])) return heaters[left];
        return heaters[right];
    }
}
```

__Python__:

```Python
class Solution:
    def findRadius(self, houses: List[int], heaters: List[int]) -> int:
        result = 0
        heaters.sort()
        for house in houses:
            result = max(abs(self.binary_search(heaters, house) - house), result)
        return result

    def binary_search(self, heaters: List[int], target: int) -> int:
        left, right = 0, len(heaters) - 1
        while left < right - 1:
            mid = ((right - left) >> 1) + left
            if target == heaters[mid]:
                return heaters[mid]
            elif target < heaters[mid]:
                right = mid
            else:
                left = mid
        if abs(target - heaters[left]) < abs(target - heaters[right]):
            return heaters[left]
        return heaters[right]
```
