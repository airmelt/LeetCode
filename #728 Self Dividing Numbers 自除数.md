__Description__:
A self-dividing number is a number that is divisible by every digit it contains.

For example, 128 is a self-dividing number because 128 % 1 == 0, 128 % 2 == 0, and 128 % 8 == 0.

Also, a self-dividing number is not allowed to contain the digit zero.

Given a lower and upper number bound, output a list of every possible self dividing number, including the bounds if possible.

__Example:__
Example 1:

Input: 
left = 1, right = 22
Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]

__Note:__

The boundaries of each input argument are 1 <= left <= right <= 10000.

__题目描述__:
自除数 是指可以被它包含的每一位数除尽的数。

例如，128 是一个自除数，因为 128 % 1 == 0，128 % 2 == 0，128 % 8 == 0。

还有，自除数不允许包含 0 。

给定上边界和下边界数字，输出一个列表，列表的元素是边界（含边界）内所有的自除数。

__示例 :__
示例 1：

输入： 
上边界left = 1, 下边界right = 22
输出： [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]

__注意：__

每个输入参数的边界满足 1 <= left <= right <= 10000。

__思路__:
1. 对每一个范围内的元素逐位计算
时间复杂度O(n), 空间复杂度O(1)
2. 打表法, left和 right的范围已经确定, 可以算出所有在 10000以内的自除数
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    vector<int> selfDividingNumbers(int left, int right) {
        vector<int> result;
        for (int i = left; i < right + 1; i++) if (isSelfDividing(i)) result.push_back(i);
        return result;
    }
private:
    bool isSelfDividing(int i) {
        for (char c : to_string(i)) if (!(c - '0') || i % (c - '0')) return false;
        return true;
    }
};
```

__Java__:
```
class Solution {
    public List<Integer> selfDividingNumbers(int left, int right) {
        List<Integer> result = new ArrayList<>();
        for (int i = left; i < right + 1; i++) if (isSelfDividing(i)) result.add(i);
        return result;
    }
    
    private boolean isSelfDividing(int i) {
        int n = i;
        while (n > 0) {
            if (n % 10 == 0 || i % (n % 10) != 0) return false;
            n /= 10;
        }
        return true;
    }
}
```

__Python__:
```
class Solution:
    def selfDividingNumbers(self, left: int, right: int) -> List[int]:
        return [n for n in range(left, right + 1) if '0' not in str(n) and all([n % int(i) == 0 for i in str(n)])]
```