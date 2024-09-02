# 2231 Largest Number After Digit Swaps by Parity 按奇偶性交换后的最大数字

__Description:__

You are given a positive integer `num`. You may swap any two digits of `num` that have the same __parity__ (i.e. both odd digits or both even digits).

Return _the __largest__ possible value of_ `num` _after __any__ number of swaps._

__Example:__

Example 1:

```text
Input: num = 1234
Output: 3412
Explanation: Swap the digit 3 with the digit 1, this results in the number 3214.
Swap the digit 2 with the digit 4, this results in the number 3412.
Note that there may be other sequences of swaps but it can be shown that 3412 is the largest possible number.
Also note that we may not swap the digit 4 with the digit 1 since they are of different parities.
```

Example 2:

```text
Input: num = 65875
Output: 87655
Explanation: Swap the digit 8 with the digit 6, this results in the number 85675.
Swap the first digit 5 with the digit 7, this results in the number 87655.
Note that there may be other sequences of swaps but it can be shown that 87655 is the largest possible number.
```

__Constraints:__

- `1 <= num <= 10 ^ 9`

__题目描述:__

给你一个正整数 `num` 。你可以交换 `num` 中 __奇偶性__ 相同的任意两位数字（即，都是奇数或者偶数）。

返回交换 __任意__ 次之后 `num` 的 __最大__ 可能值。

__示例:__

示例 1：

```text
输入：num = 1234
输出：3412
解释：交换数字 3 和数字 1 ，结果得到 3214 。
交换数字 2 和数字 4 ，结果得到 3412 。
注意，可能存在其他交换序列，但是可以证明 3412 是最大可能值。
注意，不能交换数字 4 和数字 1 ，因为它们奇偶性不同。
```

示例 2：

```text
输入：num = 65875
输出：87655
解释：交换数字 8 和数字 6 ，结果得到 85675 。
交换数字 5 和数字 7 ，结果得到 87655 。
注意，可能存在其他交换序列，但是可以证明 87655 是最大可能值。
```

__提示：__

- `1 <= num <= 10 ^ 9`

__思路:__

```text
贪心
按照奇偶性分别存储数字
排序或者用优先队列存储
遍历字符串, 交换奇数中最大的数字和偶数中最大的数字
最后返回结果
时间复杂度为 O(K ^ 2), 空间复杂度为 O(K), 其中 K = logN, K 为 num 的位数, N 为 num 的大小
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int largestInteger(int num) 
    {
        string s = to_string(num);
        int n = s.size();
        for (int i = 0; i < n - 1; i++) for (int j = i + 1; j < n; j++) if (!((s[i] - s[j]) & 1) and s[i] < s[j]) swap(s[i], s[j]);
        return stoi(s);
    }
};
```

__Java__:

```Java
class Solution {
    public int largestInteger(int num) {
        var arr = String.valueOf(num).toCharArray();
        PriorityQueue<Integer> odd = new PriorityQueue<>((a, b) -> b - a), even = new PriorityQueue<>((a, b) -> b - a);
        for (int i = 0, n = arr.length, x = 0; i < n; i++) {
            if (((x = arr[i] - '0') & 1) == 0) even.offer(x);
            else odd.offer(x);
            arr[i] = (x & 1) == 0 ? '0' : '1';
        }
        for (int i = 0, n = arr.length; i < n; i++) arr[i] = arr[i] == '0' ? (char)(even.poll() + '0') : (char)(odd.poll() + '0');
        return Integer.parseInt(new String(arr));
   }
}
```

__Python__:

```Python
class Solution:
    def largestInteger(self, num: int) -> int:
        return int(''.join(map(str, [nums1.pop() if int(x) & 1 else nums2.pop() for x in str(num)]))) if (nums1 := sorted([int(x) for x in str(num) if x in '13579'])) and (nums2 := sorted([int(x) for x in str(num) if x in '02468'])) else int(''.join(sorted(str(num), reverse=True)))
```
