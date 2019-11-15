__Description__:
Given two positive integers x and y, an integer is powerful if it is equal to x^i + y^j for some integers i >= 0 and j >= 0.

Return a list of all powerful integers that have value less than or equal to bound.

You may return the answer in any order.  In your answer, each value should occur at most once.

__Example:__
Example 1:

Input: x = 2, y = 3, bound = 10
Output: [2,3,4,5,7,9,10]
Explanation: 
2 = 2^0 + 3^0
3 = 2^1 + 3^0
4 = 2^0 + 3^1
5 = 2^1 + 3^1
7 = 2^2 + 3^1
9 = 2^3 + 3^0
10 = 2^0 + 3^2

Example 2:

Input: x = 3, y = 5, bound = 15
Output: [2,4,6,8,10,14]
 
__Note:__

1 <= x <= 100
1 <= y <= 100
0 <= bound <= 10^6

__题目描述__:
给定两个正整数 x 和 y，如果某一整数等于 x^i + y^j，其中整数 i >= 0 且 j >= 0，那么我们认为该整数是一个强整数。

返回值小于或等于 bound 的所有强整数组成的列表。

你可以按任何顺序返回答案。在你的回答中，每个值最多出现一次。

__示例 :__
示例 1：

输入：x = 2, y = 3, bound = 10
输出：[2,3,4,5,7,9,10]
解释： 
2 = 2^0 + 3^0
3 = 2^1 + 3^0
4 = 2^0 + 3^1
5 = 2^1 + 3^1
7 = 2^2 + 3^1
9 = 2^3 + 3^0
10 = 2^0 + 3^2

示例 2：

输入：x = 3, y = 5, bound = 15
输出：[2,4,6,8,10,14]
 
__提示：__

1 <= x <= 100
1 <= y <= 100
0 <= bound <= 10^6

__思路__:
注意有特殊情况 1, 可以单独拿出来讨论
由于 2 ^ 20 = 1048576 > bound <= 100000, 最大边界到 20
时间复杂度O(1), 空间复杂度O(logn), 其中 n就是 bound的值

__代码__:
__C++__:
```C++
class Solution {
public:
    vector<int> powerfulIntegers(int x, int y, int bound) {
        set<int> s;
        for (int i = 0; i < 20; i++) for (int j = 0; j < 20; j++) if (pow(x, i) + pow(y, j) <= bound) s.insert(pow(x, i) + pow(y, j));
        vector<int> result;
        result.assign(s.begin(), s.end());
        return result;
    }
};
```

__Java__:
```Java
class Solution {
    public List<Integer> powerfulIntegers(int x, int y, int bound) {
        Set<Integer> set = new HashSet<>();
        for (int i = 0; i < 20; i++) for (int j = 0; j < 20; j++) if (Math.pow(x, i) + Math.pow(y, j) <= bound) set.add((int)(Math.pow(x, i) + Math.pow(y, j)));
        List<Integer> result = new ArrayList<>();
        result.addAll(set);
        return result;
    }
}
```

__Python__:
```Python
class Solution:
    def powerfulIntegers(self, x: int, y: int, bound: int) -> List[int]:
        return list(set(x ** i + y ** j for i in range(20) for j in range(20) if x ** i + y ** j <= bound))
```