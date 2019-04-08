__Description__:
Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.

Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

The order of elements can be changed. It doesn't matter what you leave beyond the new length.

__Example__:
**Example 1:**
Given *nums* = **[3,2,2,3]**, *val* = **3**,

Your function should return length = **2**, with the first two elements of *nums* being **2**.

It doesn't matter what you leave beyond the returned length.

**Example 2:**
Given *nums* = **[0,1,2,2,3,0,4,2]**, *val* = **2**,

Your function should return length = **`5`**, with the first five elements of *`nums`* containing **`0`**, **`1`**, **`3`**, **`0`**, and **4**.

Note that the order of those five elements can be arbitrary.

It doesn't matter what values are set beyond the returned length.

**Clarification:**

Confused why the returned value is an integer but your answer is an array?

Note that the input array is passed in by **reference**, which means modification to the input array will be known to the caller as well.

Internally you can think of this:
```
// **nums** is passed in by reference. (i.e., without making a copy)
int len = removeElement(nums, val);

// any modification to **nums** in your function would be known by the caller.
// using the length returned by your function, it prints the first **len** elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

给定一个数组 *nums *和一个值 *val*，你需要**[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)**移除所有数值等于 *val *的元素，返回移除后数组的新长度。

不要使用额外的数组空间，你必须在**[原地](https://baike.baidu.com/item/%E5%8E%9F%E5%9C%B0%E7%AE%97%E6%B3%95)修改输入数组**并在使用 O(1) 额外空间的条件下完成。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

**示例 1:**

给定 *nums* = **[3,2,2,3]**, *val* = **3**,

函数应该返回新的长度 **2**, 并且 *nums* 中的前两个元素均为 **2**。

你不需要考虑数组中超出新长度后面的元素。


**示例 2:**

给定 *nums* = **[0,1,2,2,3,0,4,2]**, *val* = **2**,

函数应该返回新的长度 **`5`**, 并且 *nums* 中的前五个元素为 **`0`**, **`1`**, **`3`**, **`0`**, **4**。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**“引用”**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:
```
// **nums** 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中**该长度范围内**的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```
