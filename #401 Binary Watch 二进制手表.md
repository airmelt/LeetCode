# 401 Binary Watch 二进制手表

__Description__:
A binary watch has 4 LEDs on the top which represent the hours (0-11), and the 6 LEDs on the bottom represent the minutes (0-59).

Each LED represents a zero or one, with the least significant bit on the right.

![Binary_clock](https://upload.wikimedia.org/wikipedia/commons/8/8b/Binary_clock_samui_moon.jpg)
For example, the above binary watch reads "3:25".

Given a non-negative integer n which represents the number of LEDs that are currently on, return all possible times the watch could represent.

__Example:__

Input: n = 1
Return: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]

__Note:__
The order of output does not matter.
The hour must not contain a leading zero, for example "01:00" is not valid, it should be "1:00".
The minute must be consist of two digits and may contain a leading zero, for example "10:2" is not valid, it should be "10:02".

__题目描述__:
二进制手表顶部有 4 个 LED 代表小时（0-11），底部的 6 个 LED 代表分钟（0-59）。

每个 LED 代表一个 0 或 1，最低位在右侧。

![二进制手表(淘宝无货)](https://upload.wikimedia.org/wikipedia/commons/8/8b/Binary_clock_samui_moon.jpg)
例如，上面的二进制手表读取 “3:25”。

给定一个非负整数 n 代表当前 LED 亮着的数量，返回所有可能的时间。

__案例:__

输入: n = 1
返回: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]

__注意事项:__

输出的顺序没有要求。
小时不会以零开头，比如 “01:00” 是不允许的，应为 “1:00”。
分钟必须由两位数组成，可能会以零开头，比如 “10:2” 是无效的，应为 “10:02”。

__思路__:

参考[Java源码Integer.bitCount算法解析，分析原理（统计二进制bit位）
](https://segmentfault.com/a/1190000015763941)
设置两个指针, 一个为时针(0~12), 一个为分针(0~60), 统计二进制中 1的数量满足要求的即可
时间复杂度O(1), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<string> readBinaryWatch(int num) 
    {
        vector<string> result;
        // 左移 6位是为了j(j < 64)与 i一定不相交, 取 6-28都可以
        for (int i = 0; i < 12; i++) for (int j = 0; j < 60; j++) if (bitCount((i << 6) | j) == num) result.push_back(to_string(i) + ":" + (j > 9 ? "" : "0") + to_string(j));
        return result;
    }
private:
    int bitCount(int i) 
    {
        i = (i & 0x55555555) + ((i >> 1) & 0x55555555);
        i = (i & 0x33333333) + ((i >> 2) & 0x33333333);
        i = (i & 0x0f0f0f0f) + ((i >> 4) & 0x0f0f0f0f);
        i = (i & 0x00ff00ff) + ((i >> 8) & 0x00ff00ff);
        i = (i & 0x0000ffff) + ((i >> 16) & 0x0000ffff);
        return i;
    }
};
```

__Java__:

```Java
class Solution {
    public List<String> readBinaryWatch(int num) {
        List<String> result = new ArrayList<>();
        for (int i = 0; i < 12; i++) for (int j = 0; j < 60; j++) if (Integer.bitCount((i << 6) | j) == num) result.add(i + ":" + (j > 9 ? "" : "0") + j);
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def readBinaryWatch(self, num: int) -> List[str]:
        result = []
        for i in range(12):
            for j in range(60):
                if bin(i).count('1') + bin(j).count('1') == num:
                    if j < 10:
                        j = "0" + str(j)
                    result.append(str(i) + ":" + str(j))
        return result
```
