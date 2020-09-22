__Description__:
Convert a non-negative integer to its english words representation. Given input is guaranteed to be less than 2^31 - 1.

__Example:__
Example 1:

Input: 123
Output: "One Hundred Twenty Three"

Example 2:

Input: 12345
Output: "Twelve Thousand Three Hundred Forty Five"

Example 3:

Input: 1234567
Output: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"

Example 4:

Input: 1234567891
Output: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"

__题目描述__:
将非负整数转换为其对应的英文表示。可以保证给定输入小于 2^31 - 1 。

__示例 :__
示例 1:

输入: 123
输出: "One Hundred Twenty Three"

示例 2:

输入: 12345
输出: "Twelve Thousand Three Hundred Forty Five"

示例 3:

输入: 1234567
输出: "One Million Two Hundred Thirty Four Thousand Five Hundred Sixty Seven"

示例 4:

输入: 1234567891
输出: "One Billion Two Hundred Thirty Four Million Five Hundred Sixty Seven Thousand Eight Hundred Ninety One"

__思路__:
按照每 1000分开数字, 转换成英文即可
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
class Solution 
{
public:
    string numberToWords(int num) 
    {
        if (!num) return low.back();
        string result = "";
        for (int i = 1000000000, j = 0; j < 4; i /= 1000, j++)
        {
            int temp = num / i;
            if (temp)
            {
                result += helper(temp) + " " + high[j] + " ";
                num %= i;
            }
        }
        trim(result);
        return result;
    }
private:
    vector<string> low{" ", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen", "Zero"}, mid{" ", " ", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"}, high{"Billion", "Million", "Thousand", " ", "Hundred"};
    
    string helper(int num)
    {
        string result;
        if (num /100)
        {
            result += low[num / 100] + high[3] + high.back() + high[3];
            num %= 100;
        }
        if (num >= 20)
        {
            result += mid[num / 10] + " ";
            num %= 10;
        }
        if (num < 20) result += low[num] + " ";
        trim(result);
        return result;
    }
    
    void trim(string &result)
    {
        while (result.back() == ' ') result.pop_back();
    }
};
```

__Java__:
```Java
class Solution {
    
    private final String low[] = {"", "One", "Two", "Three", "Four", "Five", "Six", "Seven", "Eight", "Nine", "Ten", "Eleven", "Twelve", "Thirteen", "Fourteen", "Fifteen", "Sixteen", "Seventeen", "Eighteen", "Nineteen", "Zero"}, mid[] = {"", "", "Twenty", "Thirty", "Forty", "Fifty", "Sixty", "Seventy", "Eighty", "Ninety"}, high[] = {" ", "Thousand", "Million", "Billion", "Hundred"};
    
    public String numberToWords(int num) {
        if (num == 0) return low[20];
        StringBuilder sb = new StringBuilder();
        int index = 0;
        while (num > 0) {
            if (num % 1000 != 0) {
                StringBuilder temp = new StringBuilder();
                helper(num % 1000, temp);
                sb.insert(0, temp.append(high[index]).append(high[0]));
            }
            index++;
            num /= 1000;
        }
        return sb.toString().trim();
    }

    private void helper(int num, StringBuilder temp) {
        if (num == 0) return;
        if (num < 20) temp.append(low[num]).append(high[0]);
        else if (num < 100) {
            temp.append(mid[num / 10]).append(high[0]);
            helper(num % 10, temp);
        } else {
            temp.append(low[num / 100]).append(high[0]).append(high[4]).append(high[0]);
            helper(num % 100, temp);
        }
    }
}
```

__Python__:
```Python
class Solution:
    def numberToWords(self, num: int) -> str:
        low, high, result, i = ['', 'One', 'Two', 'Three', 'Four', 'Five', 'Six', 'Seven', 'Eight', 'Nine', 'Ten','Eleven', 'Twelve', 'Thirteen', 'Fourteen', 'Fifteen', 'Sixteen', 'Seventeen','Eighteen', 'Nineteen', 'Twenty', 'Twenty One', 'Twenty Two', 'Twenty Three','Twenty Four', 'Twenty Five', 'Twenty Six', 'Twenty Seven', 'Twenty Eight','Twenty Nine', 'Thirty', 'Thirty One', 'Thirty Two', 'Thirty Three','Thirty Four', 'Thirty Five', 'Thirty Six', 'Thirty Seven', 'Thirty Eight','Thirty Nine', 'Forty', 'Forty One', 'Forty Two', 'Forty Three', 'Forty Four','Forty Five', 'Forty Six', 'Forty Seven', 'Forty Eight', 'Forty Nine', 'Fifty','Fifty One', 'Fifty Two', 'Fifty Three', 'Fifty Four', 'Fifty Five','Fifty Six', 'Fifty Seven', 'Fifty Eight', 'Fifty Nine', 'Sixty','Sixty One', 'Sixty Two', 'Sixty Three', 'Sixty Four', 'Sixty Five','Sixty Six', 'Sixty Seven', 'Sixty Eight', 'Sixty Nine', 'Seventy','Seventy One', 'Seventy Two', 'Seventy Three', 'Seventy Four','Seventy Five', 'Seventy Six', 'Seventy Seven', 'Seventy Eight','Seventy Nine', 'Eighty', 'Eighty One', 'Eighty Two', 'Eighty Three','Eighty Four', 'Eighty Five', 'Eighty Six', 'Eighty Seven', 'Eighty Eight','Eighty Nine', 'Ninety', 'Ninety One', 'Ninety Two', 'Ninety Three', 'Ninety Four','Ninety Five', 'Ninety Six', 'Ninety Seven', 'Ninety Eight', 'Ninety Nine', ], ['', ' Thousand ', ' Million ', ' Billion ', ' Hundred ', 'Zero'], '', 0
        while num:
            temp = (lambda n: low[n // 100] + high[-2] + low[n % 100] if n > 99 else low[n])(num % 1000).strip()
            if temp:
                result = temp + high[i] + result
            i += 1
            num //= 1000
        return result.strip() if result else high[-1]
```