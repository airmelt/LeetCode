# 2960 Count Tested Devices After Test Operations 统计已测试设备

__Description:__

You are given a __0-indexed__ integer array `batteryPercentages` having length `n`, denoting the battery percentages of `n` __0-indexed__ devices.

Your task is to test each device `i` __in order__ from `0` to `n - 1`, by performing the following test operations:

- If `batteryPercentages[i]` is __greater__ than `0`:
  - __Increment__ the count of tested devices.
  - __Decrease__ the battery percentage of all devices with indices `j` in the range `[i + 1, n - 1]` by `1`, ensuring their battery percentage __never goes below__ `0`, i.e, `batteryPercentages[j] = max(0, batteryPercentages[j] - 1)`.
- Move to the next device.

Return an integer denoting the number of devices that will be tested after performing the test operations in order.

__Example:__

Example 1:

```text
Input: batteryPercentages = [1,1,2,1,3]
Output: 3
Explanation: Performing the test operations in order starting from device 0:
At device 0, batteryPercentages[0] > 0, so there is now 1 tested device, and batteryPercentages becomes [1,0,1,0,2].
At device 1, batteryPercentages[1] == 0, so we move to the next device without testing.
At device 2, batteryPercentages[2] > 0, so there are now 2 tested devices, and batteryPercentages becomes [1,0,1,0,1].
At device 3, batteryPercentages[3] == 0, so we move to the next device without testing.
At device 4, batteryPercentages[4] > 0, so there are now 3 tested devices, and batteryPercentages stays the same.
So, the answer is 3.
```

Example 2:

```text
Input: batteryPercentages = [0,1,2]
Output: 2
Explanation: Performing the test operations in order starting from device 0:
At device 0, batteryPercentages[0] == 0, so we move to the next device without testing.
At device 1, batteryPercentages[1] > 0, so there is now 1 tested device, and batteryPercentages becomes [0,1,1].
At device 2, batteryPercentages[2] > 0, so there are now 2 tested devices, and batteryPercentages stays the same.
So, the answer is 2.
```

__Constraints:__

- `1 <= n == batteryPercentages.length <= 100`
- `0 <= batteryPercentages[i] <= 100`

__题目描述:__

给你一个长度为 `n` 、下标从 __0__ 开始的整数数组 `batteryPercentages` ，表示 `n` 个设备的电池百分比。

你的任务是按照顺序测试每个设备 `i`，执行以下测试操作:

- 如果 `batteryPercentages[i]` __大于__ `0`:
  - __增加__ 已测试设备的计数。
  - 将下标 `j` 在 `[i + 1, n - 1]` 的所有设备的电池百分比减少 `1`，确保它们的电池百分比 __不会低于__ `0` ，即 `batteryPercentages[j] = max(0, batteryPercentages[j] - 1)`。
- 移动到下一个设备。

返回一个整数，表示按顺序执行测试操作后 已测试设备 的数量。

__示例:__

示例 1：

```text
输入：batteryPercentages = [1,1,2,1,3]
输出：3
解释：按顺序从设备 0 开始执行测试操作：
在设备 0 上，batteryPercentages[0] > 0 ，现在有 1 个已测试设备，batteryPercentages 变为 [1,0,1,0,2] 。
在设备 1 上，batteryPercentages[1] == 0 ，移动到下一个设备而不进行测试。
在设备 2 上，batteryPercentages[2] > 0 ，现在有 2 个已测试设备，batteryPercentages 变为 [1,0,1,0,1] 。
在设备 3 上，batteryPercentages[3] == 0 ，移动到下一个设备而不进行测试。
在设备 4 上，batteryPercentages[4] > 0 ，现在有 3 个已测试设备，batteryPercentages 保持不变。
因此，答案是 3 。
```

示例 2：

```text
输入：batteryPercentages = [0,1,2]
输出：2
解释：按顺序从设备 0 开始执行测试操作：
在设备 0 上，batteryPercentages[0] == 0 ，移动到下一个设备而不进行测试。
在设备 1 上，batteryPercentages[1] > 0 ，现在有 1 个已测试设备，batteryPercentages 变为 [0,1,1] 。
在设备 2 上，batteryPercentages[2] > 0 ，现在有 2 个已测试设备，batteryPercentages 保持不变。
因此，答案是 2 。
```

__提示：__

- `1 <= n == batteryPercentages.length <= 100`
- `0 <= batteryPercentages[i] <= 100`

__思路:__

```text
模拟
刚开始的时候电池最少需要为 0
每次测试之后电池阈值需要加 1
检测当前电池电量是否超过阈值
最后返回这个阈值即可
时间复杂度为 O(N), 空间复杂度为 O(1)
```

__代码:__

__C++__:

```C++
class Solution 
{
public:
    int countTestedDevices(vector<int>& batteryPercentages) 
    {
        int result = 0;
        for (const auto& b : batteryPercentages) if (b - result > 0) ++result;
        return result;
    }
};
```

__Java__:

```Java
class Solution {
    public int countTestedDevices(int[] batteryPercentages) {
        int result = 0;
        for (int b : batteryPercentages) if (b - result > 0) ++result;
        return result;
    }
}
```

__Python__:

```Python
class Solution:
    def countTestedDevices(self, batteryPercentages: List[int]) -> int:
        result = 0
        for b in batteryPercentages:
            if b > result:
                result += 1
        return result
```
