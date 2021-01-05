# 412 Fizz Buzz Fizz Buzz

__Description__:
Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.

__Example:__

n = 15,

Return:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]

__题目描述__:

1. 如果 n 是3的倍数，输出“Fizz”；

2. 如果 n 是5的倍数，输出“Buzz”；

3. 如果 n 同时是3和5的倍数，输出 “FizzBuzz”。

__示例：__

n = 15,

返回:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]

__思路__:

利用整除, 能整除 15输出 "FizzBuzz", 其他以此类推
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> fizzBuzz(int n) 
    {
        vector<string> result;
        for (int i = 1; i < n + 1; i++) 
        {
            if (!(i % 15)) result.push_back("FizzBuzz");
            else if (!(i % 5)) result.push_back("Buzz");
            else if (!(i % 3)) result.push_back("Fizz");
            else result.push_back(to_string(i));
        }
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> fizzBuzz(int n) {
        List<String> result = new ArrayList<>(n);
        for (int i = 1; i < n + 1; i++) {
            if (i % 15 == 0) result.add("FizzBuzz");
            else if (i % 5 == 0) result.add("Buzz");
            else if (i % 3 == 0) result.add("Fizz");
            else result.add(String.valueOf(i));
        }
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def fizzBuzz(self, n: int) -> List[str]:
        return ["Fizz" * (not i % 3) + "Buzz" * (not i % 5) or str(i) for i in range(1, n + 1)]
```
