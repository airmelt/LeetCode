# 278 First Bad Version 第一个错误的版本

__Description__:
You are a product manager and currently leading a team to develop a new product. Unfortunately, the latest version of your product fails the quality check. Since each version is developed based on the previous version, all the versions after a bad version are also bad.

Suppose you have n versions [1, 2, ..., n] and you want to find out the first bad one, which causes all the following ones to be bad.

You are given an API bool isBadVersion(version) which will return whether version is bad. Implement a function to find the first bad version. You should minimize the number of calls to the API.

**Example :**

Given n = 5, and version = 4 is the first bad version.

call isBadVersion(3) -> false
call isBadVersion(5) -> true
call isBadVersion(4) -> true

Then 4 is the first bad version.

__题目描述__:
你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

**示例 :**

给定 n = 5，并且 version = 4 是第一个错误的版本。

调用 isBadVersion(3) -> false
调用 isBadVersion(5) -> true
调用 isBadVersion(4) -> true

所以，4 是第一个错误的版本。

__思路__:

二分查找, 注意边界条件和溢出
时间复杂度O(lgn), 空间复杂度O(1)

__代码__:
__C++__:

```C++
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution 
{
public:
    int firstBadVersion(int n) 
    {
        int low = 1;
        while (low < n) 
        {
            int mid = low + ((n - low) >> 1);
            if (isBadVersion(mid)) n = mid;
            else low = ++mid;
        }
        return low;
    }
};
```

__Java__:

```Java
/* The isBadVersion API is defined in the parent class VersionControl.
      boolean isBadVersion(int version); */

public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int low = 1;
        while (low < n) {
            int mid = low + ((n - low) >> 1);
            if (isBadVersion(mid)) n = mid;
            else low = ++mid;
        }
        return low;
    }
}
```

__Python__:

```Python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return a bool
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n: int) -> int:
        low = 1
        while low < n:
            mid = low + ((n - low) >> 1)
            if isBadVersion(mid):
                n = mid
            else:
                low = mid + 1
        return low
```
