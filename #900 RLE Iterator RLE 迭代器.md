# 900 RLE Iterator RLE 迭代器

__Description__:
We can use run-length encoding (i.e., RLE) to encode a sequence of integers. In a run-length encoded array of even length encoding (0-indexed), for all even i, encoding[i] tells us the number of times that the non-negative integer value encoding[i + 1] is repeated in the sequence.

For example, the sequence arr = [8,8,8,5,5] can be encoded to be encoding = [3,8,2,5]. encoding = [3,8,0,9,2,5] and encoding = [2,8,1,8,2,5] are also valid RLE of arr.
Given a run-length encoded array, design an iterator that iterates through it.

Implement the RLEIterator class:

RLEIterator(int[] encoded) Initializes the object with the encoded array encoded.
int next(int n) Exhausts the next n elements and returns the last element exhausted in this way. If there is no element left to exhaust, return -1 instead.

__Example:__

Example 1:

Input
["RLEIterator", "next", "next", "next", "next"]
[[[3, 8, 0, 9, 2, 5]], [2], [1], [1], [2]]
Output
[null, 8, 8, 5, -1]

Explanation

```Java
RLEIterator rLEIterator = new RLEIterator([3, 8, 0, 9, 2, 5]); // This maps to the sequence [8,8,8,5,5].
rLEIterator.next(2); // exhausts 2 terms of the sequence, returning 8. The remaining sequence is now [8, 5, 5].
rLEIterator.next(1); // exhausts 1 term of the sequence, returning 8. The remaining sequence is now [5, 5].
rLEIterator.next(1); // exhausts 1 term of the sequence, returning 5. The remaining sequence is now [5].
rLEIterator.next(2); // exhausts 2 terms, returning -1. This is because the first term exhausted was 5,
but the second term did not exist. Since the last term exhausted does not exist, we return -1.
```

__Constraints:__

2 <= encoding.length <= 1000
encoding.length is even.
0 <= encoding[i] <= 10^9
1 <= n <= 10^9
At most 1000 calls will be made to next.

__题目描述__:
编写一个遍历游程编码序列的迭代器。

迭代器由 RLEIterator(int[] A) 初始化，其中 A 是某个序列的游程编码。更具体地，对于所有偶数 i，A[i] 告诉我们在序列中重复非负整数值 A[i + 1] 的次数。

迭代器支持一个函数：next(int n)，它耗尽接下来的  n 个元素（n >= 1）并返回以这种方式耗去的最后一个元素。如果没有剩余的元素可供耗尽，则  next 返回 -1 。

例如，我们以 A = [3,8,0,9,2,5] 开始，这是序列 [8,8,8,5,5] 的游程编码。这是因为该序列可以读作 “三个八，零个九，两个五”。

__示例 :__

输入：["RLEIterator","next","next","next","next"], [[[3,8,0,9,2,5]],[2],[1],[1],[2]]
输出：[null,8,8,5,-1]
解释：
RLEIterator 由 RLEIterator([3,8,0,9,2,5]) 初始化。
这映射到序列 [8,8,8,5,5]。
然后调用 RLEIterator.next 4次。

RLEIterator.next(2) 耗去序列的 2 个项，返回 8。现在剩下的序列是 [8, 5, 5]。

RLEIterator.next(1) 耗去序列的 1 个项，返回 8。现在剩下的序列是 [5, 5]。

RLEIterator.next(1) 耗去序列的 1 个项，返回 5。现在剩下的序列是 [5]。

RLEIterator.next(2) 耗去序列的 2 个项，返回 -1。 这是由于第一个被耗去的项是 5，
但第二个项并不存在。由于最后一个要耗去的项不存在，我们返回 -1。

__提示:__

0 <= A.length <= 1000
A.length 是偶数。
0 <= A[i] <= 10^9
每个测试用例最多调用 1000 次 RLEIterator.next(int n)。
每次调用 RLEIterator.next(int n) 都有 1 <= n <= 10^9 。

__思路__:

模拟
记录下标的位置
如果下标指向的元素个数小于 n, 则移动下标, 并更新 n
否则减少下标指向的元素个数, 并将 n 置零
返回时比较下标和 encoding 的长度返回 -1 或对应位置的元素
时间复杂度为 O(n), 空间复杂度为 O(n)

__代码__:
__C++__:

```C++
class RLEIterator 
{
private:
    vector<int> nums;
    int index, length;
public:
    RLEIterator(vector<int>& encoding) 
    {
        nums = encoding;
        index = 0;
        length = encoding.size();
    }
    
    int next(int n) 
    {
        while (index < length and n)
        {
            if (nums[index] >= n)
            {
                nums[index] -= n;
                n = 0;
            }
            else
            {
                n -= nums[index];
                index += 2;
            }
        }
        return index < length ? nums[index + 1] : -1;
    }
};

/**
 * Your RLEIterator object will be instantiated and called as such:
 * RLEIterator* obj = new RLEIterator(encoding);
 * int param_1 = obj->next(n);
 */
```

__Java__:

```Java
class RLEIterator {
    private int[] nums;
    private int index, size;
    
    public RLEIterator(int[] encoding) {
        index = 0;
        nums = encoding;
        size = encoding.length;
    }
    
    public int next(int n) {
        while (index < size && n > 0) {
            if (nums[index] >= n) {
                nums[index] -= n;
                n = 0;
            } else {
                n -= nums[index];
                index += 2;
            }
        }
        return index >= size ? -1 : nums[index + 1];
    }
}

/**
 * Your RLEIterator object will be instantiated and called as such:
 * RLEIterator obj = new RLEIterator(encoding);
 * int param_1 = obj.next(n);
 */
```

__Python__:

```Python
class RLEIterator:

    def __init__(self, encoding: List[int]):
        self.encoding = encoding[::-1]
        self.index = 0
        

    def next(self, n: int) -> int:
        while self.encoding and n > 0:
            if n > self.encoding[-1]:
                n -= self.encoding.pop()
                self.encoding.pop()
            else:
                self.encoding[-1] -= n
                n = 0
        return -1 if not self.encoding else self.encoding[-2]
            


# Your RLEIterator object will be instantiated and called as such:
# obj = RLEIterator(encoding)
# param_1 = obj.next(n)
```
