__Description__:
Given an integer array with even length, where different numbers in this array represent different kinds of candies. Each number means one candy of the corresponding kind. You need to distribute these candies equally in number to brother and sister. Return the maximum number of kinds of candies the sister could gain.

__Example:__
Example 1:
Input: candies = [1,1,2,2,3,3]
Output: 3
Explanation:
There are three different kinds of candies (1, 2 and 3), and two candies for each kind.
Optimal distribution: The sister has candies [1,2,3] and the brother has candies [1,2,3], too.
The sister has three different kinds of candies.

Example 2:
Input: candies = [1,1,2,3]
Output: 2
Explanation: For example, the sister has candies [2,3] and the brother has candies [1,1].
The sister has two different kinds of candies, the brother has only one kind of candies.

__Note:__

The length of the given array is in range [2, 10,000], and will be even.
The number in given array is in range [-100,000, 100,000].

__题目描述__:
给定一个偶数长度的数组，其中不同的数字代表着不同种类的糖果，每一个数字代表一个糖果。你需要把这些糖果平均分给一个弟弟和一个妹妹。返回妹妹可以获得的最大糖果的种类数。

__示例 :__
示例 1:

输入: candies = [1,1,2,2,3,3]
输出: 3
解析: 一共有三种种类的糖果，每一种都有两个。
     最优分配方案：妹妹获得[1,2,3],弟弟也获得[1,2,3]。这样使妹妹获得糖果的种类数最多。

示例 2 :

输入: candies = [1,1,2,3]
输出: 2
解析: 妹妹获得糖果[2,3],弟弟获得糖果[1,1]，妹妹有两种不同的糖果，弟弟只有一种。这样使得妹妹可以获得的糖果种类数最多。

__注意:__

数组的长度为[2, 10,000]，并且确定为偶数。
数组中数字的大小在范围[-100,000, 100,000]内。

__思路__:
题意可以转化为求数组中的不同元素的个数, 转换为 set, 如果不同的元素个数大于 size / 2就直接返回 size / 2, 否则返回不同元素的个数
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    int distributeCandies(vector<int>& candies) {
        bitset<200001> b;
        for (int candy : candies) b.set(candy + 100000);
        return min(b.count(), candies.size() / 2);
    }
};
```

__Java__:
```
class Solution {
    public int distributeCandies(int[] candies) {
        BitSet b = new BitSet(200001);
        for (int candy : candies) b.set(candy + 100000);
        return Math.min(b.cardinality(), candies.length / 2);
    }
}
```

__Python__:
```
class Solution:
    def distributeCandies(self, candies: List[int]) -> int:
        return len(set(candies)) if len(set(candies)) < len(candies) // 2 else len(candies) // 2
```
