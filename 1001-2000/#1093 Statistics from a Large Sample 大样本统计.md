# 1093 Statistics from a Large Sample 大样本统计

__Description__:
You are given a large sample of integers in the range [0, 255]. Since the sample is so large, it is represented by an array count where count[k] is the number of times that k appears in the sample.

Calculate the following statistics:

minimum: The minimum element in the sample.
maximum: The maximum element in the sample.
mean: The average of the sample, calculated as the total sum of all elements divided by the total number of elements.
median:
If the sample has an odd number of elements, then the median is the middle element once the sample is sorted.
If the sample has an even number of elements, then the median is the average of the two middle elements once the sample is sorted.
mode: The number that appears the most in the sample. It is guaranteed to be unique.
Return the statistics of the sample as an array of floating-point numbers [minimum, maximum, mean, median, mode]. Answers within 10^-5 of the actual answer will be accepted.

__Example:__

Example 1:

Input: count = [0,1,3,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
Output: [1.00000,3.00000,2.37500,2.50000,3.00000]
Explanation: The sample represented by count is [1,2,2,2,3,3,3,3].
The minimum and maximum are 1 and 3 respectively.
The mean is (1+2+2+2+3+3+3+3) / 8 = 19 / 8 = 2.375.
Since the size of the sample is even, the median is the average of the two middle elements 2 and 3, which is 2.5.
The mode is 3 as it appears the most in the sample.

Example 2:

Input: count = [0,4,3,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
Output: [1.00000,4.00000,2.18182,2.00000,1.00000]
Explanation: The sample represented by count is [1,1,1,1,2,2,2,3,3,4,4].
The minimum and maximum are 1 and 4 respectively.
The mean is (1+1+1+1+2+2+2+3+3+4+4) / 11 = 24 / 11 = 2.18181818... (for display purposes, the output shows the rounded number 2.18182).
Since the size of the sample is odd, the median is the middle element 2.
The mode is 1 as it appears the most in the sample.

__Constraints:__

count.length == 256
0 <= count[i] <= 10^9
1 <= sum(count) <= 10^9
The mode of the sample that count represents is unique.

__题目描述__:
我们对 0 到 255 之间的整数进行采样，并将结果存储在数组 count 中：count[k] 就是整数 k 在样本中出现的次数。

计算以下统计数据:

minimum ：样本中的最小元素。
maximum ：样品中的最大元素。
mean ：样本的平均值，计算为所有元素的总和除以元素总数。
median ：
如果样本的元素个数是奇数，那么一旦样本排序后，中位数 median 就是中间的元素。
如果样本中有偶数个元素，那么中位数median 就是样本排序后中间两个元素的平均值。
mode ：样本中出现次数最多的数字。保众数是 唯一 的。
以浮点数数组的形式返回样本的统计信息 [minimum, maximum, mean, median, mode] 。与真实答案误差在 10^-5 内的答案都可以通过。

__示例 :__

示例 1：

输入：count = [0,1,3,4,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
输出：[1.00000,3.00000,2.37500,2.50000,3.00000]
解释：用count表示的样本为[1,2,2,2,3,3,3,3,3]。
最小值和最大值分别为1和3。
均值是(1+2+2+2+3+3+3+3) / 8 = 19 / 8 = 2.375。
因为样本的大小是偶数，所以中位数是中间两个元素2和3的平均值，也就是2.5。
众数为3，因为它在样本中出现的次数最多。

示例 2：

输入：count = [0,4,3,2,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
输出：[1.00000,4.00000,2.18182,2.00000,1.00000]
解释：用count表示的样本为[1,1,1,1,2,2,3,3,3,4,4]。
最小值为1，最大值为4。
平均数是(1+1+1+1+2+2+2+3+3+4+4)/ 11 = 24 / 11 = 2.18181818…(为了显示，输出显示了整数2.18182)。
因为样本的大小是奇数，所以中值是中间元素2。
众数为1，因为它在样本中出现的次数最多。

__提示:__

count.length == 256
0 <= count[i] <= 10^9
1 <= sum(count) <= 10^9
 count 的众数是 唯一 的

__思路__:

模拟
count 数组表示对应的下标 i 的数量, 如 count[1] = 4, 表示样本中有 4 个值为 1 的整数
最大最小值和众数只需要一次遍历即可
遍历的同时记录下所有样本的个数以及样本总和
样本总和除以个数即为平均值
求中位数可以求所有左边的最后一个数和右边的第一个数的平均数, 避免讨论样本个数的奇偶性
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:

```C++
class Solution 
{
public:
    vector<double> sampleStats(vector<int>& count) 
    {
        int min_val = 1001, max_val = -1, cur = 0, max_times = 0, mode = 0, j = 0, median1 = 0, median2 = 0, mid1 = 0, mid2 = 0, t = 0;
        double sum = 0;
        for (int i = 0; i < 256; i++) 
        {
            if (count[i] > 0) 
            {
                min_val = min(min_val, i);
                max_val = max(max_val, i);
                cur += count[i];
                if (count[i] > max_times) 
                {
                    max_times = count[i];
                    mode = i;
                }
            }
            sum += (double)count[i] * i;
        }
        median1 = (cur + 1) >> 1;
        median2 = (cur >> 1) + 1;
        while (t < median1) t += count[j++];
        mid1 = j - 1;
        while (t < median2) t += count[j++];
        mid2 = j - 1;
        return vector<double>{(double)min_val, (double)max_val, sum / cur, (mid1 + mid2) / 2.0, (double)mode};
    }
};
```

__Java__:

```Java
class Solution {
    public double[] sampleStats(int[] count) {
        int minVal = 1001, maxVal = -1, cur = 0, maxTimes = 0, mode = 0, j = 0, median1 = 0, median2 = 0, mid1 = 0, mid2 = 0, t = 0;
        double sum = 0;
        for (int i = 0; i < 256; i++) {
            if (count[i] > 0) {
                minVal = Math.min(minVal, i);
                maxVal = Math.max(maxVal, i);
                cur += count[i];
                if (count[i] > maxTimes) {
                    maxTimes = count[i];
                    mode = i;
                }
            }
            sum += (double)count[i] * i;
        }
        median1 = (cur + 1) >> 1;
        median2 = (cur >> 1) + 1;
        while (t < median1) t += count[j++];
        mid1 = j - 1;
        while (t < median2) t += count[j++];
        mid2 = j - 1;
        return new double[]{(double)minVal, (double)maxVal, sum / cur, (mid1 + mid2) / 2.0, (double)mode};
    }
}
```

__Python__:

```Python
class Solution:
    def sampleStats(self, count: List[int]) -> List[float]:
        it = filter(count.__getitem__, range(256))
        minimum = maximum = mean = mode = median1 = median2 = next(it)
        n, (mid, odd) = count[minimum], divmod(sum(count), 2)
        for maximum in it:
            median1, median2 = maximum if n < mid + odd <= n + count[maximum] else median1, maximum if n < mid + 1 else median2
            mean += count[maximum] * (maximum - mean) / (n := n + count[maximum])
            mode = max(mode, maximum, key=count.__getitem__)
        return minimum, maximum, mean, (median1 + median2) / 2, mode
```
