__Description__:
Given a list of sorted characters letters containing only lowercase letters, and given a target letter target, find the smallest element in the list that is larger than the given target.

Letters also wrap around. For example, if the target is target = 'z' and letters = ['a', 'b'], the answer is 'a'.

__Examples:__

Input:
letters = ["c", "f", "j"]
target = "a"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "c"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "d"
Output: "f"

Input:
letters = ["c", "f", "j"]
target = "g"
Output: "j"

Input:
letters = ["c", "f", "j"]
target = "j"
Output: "c"

Input:
letters = ["c", "f", "j"]
target = "k"
Output: "c"

__Note:__

letters has a length in range [2, 10000].
letters consists of lowercase letters, and contains at least 2 unique letters.
target is a lowercase letter.

__题目描述__:
给定一个只包含小写字母的有序数组letters 和一个目标字母 target，寻找有序数组里面比目标字母大的最小字母。

数组里字母的顺序是循环的。举个例子，如果目标字母target = 'z' 并且有序数组为 letters = ['a', 'b']，则答案返回 'a'。

__示例 :__

输入:
letters = ["c", "f", "j"]
target = "a"
输出: "c"

输入:
letters = ["c", "f", "j"]
target = "c"
输出: "f"

输入:
letters = ["c", "f", "j"]
target = "d"
输出: "f"

输入:
letters = ["c", "f", "j"]
target = "g"
输出: "j"

输入:
letters = ["c", "f", "j"]
target = "j"
输出: "c"

输入:
letters = ["c", "f", "j"]
target = "k"
输出: "c"

__注:__

letters长度范围在[2, 10000]区间内。
letters 仅由小写字母组成，最少包含两个不同的字母。
目标字母target 是一个小写字母。


__思路__:
由于数组有序, 用二分查找, 注意顺序是循环的, 所以比较一下数组的最后一个元素和 target的大小来判定返回值
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:
```
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        int i = 0, j = letters.size() - 1;
        while (i <= j) {
            int mid = i + ((j - i) >> 1);
            if (letters[mid] > target) j = mid - 1;
            else i = mid + 1;
        }
        return letters[letters.size() - 1] <= target ? letters[0] : letters[i];
    }
};
```

__Java__:
```
class Solution {
    public char nextGreatestLetter(char[] letters, char target) {
        int i = 0, j = letters.length - 1;
        while (i <= j) {
            int mid = i + ((j - i) >> 1);
            if (letters[mid] > target) j = mid - 1;
            else i = mid + 1;
        }
        return letters[letters.length - 1] <= target ? letters[0] : letters[i];
    }
}
```

__Python__:
```
class Solution:
    def nextGreatestLetter(self, letters: List[str], target: str) -> str:
        return letters[[x > target for x in letters].index(max([x > target for x in letters]))]
```