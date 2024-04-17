# 2075 Decode the Slanted Ciphertext 解码斜向换位密码

__Description:__

A string `originalText` is encoded using a __slanted transposition cipher__ to a string `encodedText` with the help of a matrix having a __fixed number of rows__ `rows`.

`originalText` is placed first in a top-left to bottom-right manner.

![2075-1](https://assets.leetcode.com/uploads/2021/11/07/exa11.png)

The blue cells are filled first, followed by the red cells, then the yellow cells, and so on, until we reach the end of `originalText`. The arrow indicates the order in which the cells are filled. All empty cells are filled with `' '`. The number of columns is chosen such that the rightmost column will __not be empty__ after filling in `originalText`.

`encodedText` is then formed by appending all characters of the matrix in a row-wise fashion.

![2075-2](https://assets.leetcode.com/uploads/2021/11/07/exa12.png)

The characters in the blue cells are appended first to `encodedText`, then the red cells, and so on, and finally the yellow cells. The arrow indicates the order in which the cells are accessed.

For example, if `originalText = "cipher"` and `rows = 3`, then we encode it in the following manner:

![2075-3](https://assets.leetcode.com/uploads/2021/10/25/desc2.png)

The blue arrows depict how `originalText` is placed in the matrix, and the red arrows denote the order in which `encodedText` is formed. In the above example, `encodedText = "ch ie pr"`.

Given the encoded string `encodedText` and number of rows `rows`, return _the original string_ `originalText`.

__Note:__ `originalText` __does not__ have any trailing spaces `' '`. The test cases are generated such that there is only one possible `originalText`.

__Example:__

Example 1:

```text
Input: encodedText = "ch   ie   pr", rows = 3
Output: "cipher"
Explanation: This is the same example described in the problem description.
```

Example 2:

![2075-4](https://assets.leetcode.com/uploads/2021/10/26/exam1.png)

```text
Input: encodedText = "iveo    eed   l te   olc", rows = 4
Output: "i love leetcode"
Explanation: The figure above denotes the matrix that was used to encode originalText. 
The blue arrows show how we can find originalText from encodedText.
```

Example 3:

![2075-5](https://assets.leetcode.com/uploads/2021/10/26/eg2.png)

```text
Input: encodedText = "coding", rows = 1
Output: "coding"
Explanation: Since there is only 1 row, both originalText and encodedText are the same.
```

__Constraints:__

- `0 <= encodedText.length <= 10 ^ 6`
- `encodedText` consists of lowercase English letters and `' '` only.
- `encodedText` is a valid encoding of some `originalText` that __does not__ have trailing spaces.
- `1 <= rows <= 1000`
- The testcases are generated such that there is __only one__ possible `originalText`.

__题目描述:__

字符串 `originalText` 使用 __斜向换位密码__ ，经由 __行数固定__ 为 `rows` 的矩阵辅助，加密得到一个字符串 `encodedText` 。

`originalText` 先按从左上到右下的方式放置到矩阵中。

![2075-6](https://assets.leetcode.com/uploads/2021/11/07/exa11.png)

先填充蓝色单元格，接着是红色单元格，然后是黄色单元格，以此类推，直到到达 `originalText` 末尾。箭头指示顺序即为单元格填充顺序。所有空单元格用 `' '` 进行填充。矩阵的列数需满足:用 `originalText` 填充之后，最右侧列 __不为空__ 。

接着按行将字符附加到矩阵中，构造 `encodedText` 。

![2075-7](https://assets.leetcode.com/uploads/2021/11/07/exa12.png)

先把蓝色单元格中的字符附加到 `encodedText` 中，接着是红色单元格，最后是黄色单元格。箭头指示单元格访问顺序。

例如，如果 `originalText = "cipher"` 且 `rows = 3` ，那么我们可以按下述方法将其编码:

![2075-8](https://assets.leetcode.com/uploads/2021/10/25/desc2.png)

蓝色箭头标识 `originalText` 是如何放入矩阵中的，红色箭头标识形成 `encodedText` 的顺序。在上述例子中， `encodedText = "ch  ie  pr"` 。

给你编码后的字符串 `encodedText` 和矩阵的行数 `rows` ，返回源字符串 `originalText` 。

__注意:__`originalText` __不__ 含任何尾随空格 `' '` 。生成的测试用例满足 __仅存在一个__ 可能的 `originalText` 。

__示例:__

示例 1：

输入：encodedText = "ch   ie   pr", rows = 3
输出："cipher"
解释：此示例与问题描述中的例子相同。

示例 2：

![2075-9](https://assets.leetcode.com/uploads/2021/10/26/exam1.png)

```text
输入：encodedText = "iveo    eed   l te   olc", rows = 4
输出："i love leetcode"
解释：上图标识用于编码 originalText 的矩阵。 
蓝色箭头展示如何从 encodedText 找到 originalText 。
```

示例 3：

![2075-10](https://assets.leetcode.com/uploads/2021/10/26/eg2.png)

```text
输入：encodedText = "coding", rows = 1
输出："coding"
解释：由于只有 1 行，所以 originalText 和 encodedText 是相同的。
```

示例 4：

![2075-11](https://assets.leetcode.com/uploads/2021/10/26/exam3.png)

```text
输入：encodedText = " b  ac", rows = 2
输出：" abc"
解释：originalText 不能含尾随空格，但它可能会有一个或者多个前置空格。
```

__提示：__

- `0 <= encodedText.length <= 10 ^ 6`
- `encodedText` 仅由小写英文字母和 `' '` 组成
- `encodedText` 是对某个 __不含__ 尾随空格的 `originalText` 的一个有效编码
- `1 <= rows <= 1000`
- 生成的测试用例满足 __仅存在一个__ 可能的 `originalText`

__思路:__

```text
模拟
将 encodedText 看作二维矩阵
将每一列还原为原字符串
最后将结果的末尾的空格都删掉
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    string decodeCiphertext(string encodedText, int rows) 
    {
        int n = encodedText.size(), cols = n / rows, r = 0, c = 0;
        string result;
        for (int i = 0; i < cols; i++, r = 0, c = i) while (r < rows and c < cols) result += encodedText[r++ * cols + c++];
        result.erase(find_if(result.rbegin(), result.rend(), not1(ptr_fun<int, int>(isspace))).base(), result.end());
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public String decodeCiphertext(String encodedText, int rows) {
        int n = encodedText.length(), cols = n / rows, r = 0, c = 0;
        StringBuilder result = new StringBuilder();
        for (int i = 0; i < cols; i++, r = 0, c = i) while (r < rows && c < cols) result.append(encodedText.charAt(r++ * cols + c++));
        return result.toString().replaceAll("\\s+$", "");
    }
}
```

__Python__:

```Python
class Solution:
    def decodeCiphertext(self, encodedText: str, rows: int) -> str:
        cols, result = len(encodedText) // rows, []
        for i in range(cols):
            r, c = 0, i
            while r < rows and c < cols:
                result.append(encodedText[r * cols + c])
                r += 1
                c += 1
        return "".join(result).rstrip()
```
