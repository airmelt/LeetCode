# 1146 Snapshot Array 快照数组

__Description__:
Implement a SnapshotArray that supports the following interface:

SnapshotArray(int length) initializes an array-like data structure with the given length.  Initially, each element equals 0.
void set(index, val) sets the element at the given index to be equal to val.
int snap() takes a snapshot of the array and returns the snap_id: the total number of times we called snap() minus 1.
int get(index, snap_id) returns the value at the given index, at the time we took the snapshot with the given snap_id

__Example:__

Example 1:

Input: ["SnapshotArray","set","snap","set","get"]
[[3],[0,5],[],[0,6],[0,0]]
Output: [null,null,0,null,5]
Explanation:

```Java
SnapshotArray snapshotArr = new SnapshotArray(3); // set the length to be 3
snapshotArr.set(0,5);  // Set array[0] = 5
snapshotArr.snap();  // Take a snapshot, return snap_id = 0
snapshotArr.set(0,6);
snapshotArr.get(0,0);  // Get the value of array[0] with snap_id = 0, return 5
```

__Constraints:__

1 <= length <= 50000
At most 50000 calls will be made to set, snap, and get.
0 <= index < length
0 <= snap_id < (the total number of times we call snap())
0 <= val <= 10^9

__题目描述__:
实现支持下列接口的「快照数组」- SnapshotArray：

SnapshotArray(int length) - 初始化一个与指定长度相等的 类数组 的数据结构。初始时，每个元素都等于 0。
void set(index, val) - 会将指定索引 index 处的元素设置为 val。
int snap() - 获取该数组的快照，并返回快照的编号 snap_id（快照号是调用 snap() 的总次数减去 1）。
int get(index, snap_id) - 根据指定的 snap_id 选择快照，并返回该快照指定索引 index 的值。

__示例 :__

输入：["SnapshotArray","set","snap","set","get"]
     [[3],[0,5],[],[0,6],[0,0]]
输出：[null,null,0,null,5]
解释：

```Java
SnapshotArray snapshotArr = new SnapshotArray(3); // 初始化一个长度为 3 的快照数组
snapshotArr.set(0,5);  // 令 array[0] = 5
snapshotArr.snap();  // 获取快照，返回 snap_id = 0
snapshotArr.set(0,6);
snapshotArr.get(0,0);  // 获取 snap_id = 0 的快照中 array[0] 的值，返回 5
```

__提示:__

1 <= length <= 50000
题目最多进行50000 次set，snap，和 get的调用 。
0 <= index < length
0 <= snap_id < 我们调用 snap() 的总次数
0 <= val <= 10^9

__思路__:

二分
如果每次都记录一个完整的数组由于 snap 次数过多, 时间和空间都难以承受
考虑只记录快照的变化值
用哈希表记录每一次快照下的值
如果 snap_id 刚好在哈希表中直接返回
否则二分查找快照前的最后一次修改
时间复杂度为 O(lgn), 空间复杂度为 O(n), n 为快照次数

__代码__:
__C++__:

```C++
class SnapshotArray 
{
private:
    unordered_map<int, map<int, int>> data;
    int cur_id;
public:
    SnapshotArray(int length) 
    {
        cur_id = 0;    
    }
    
    void set(int index, int val) 
    {
        data[index][cur_id] = val;
    }
    
    int snap() 
    {
        return cur_id++;
    }
    
    int get(int index, int snap_id) 
    {
        if (!data.count(index)) return 0;
        auto it = data[index].upper_bound(snap_id);
        if (it != data[index].begin()) return prev(it) -> second;
        return 0;
    }
};
```

__Java__:

```Java
class SnapshotArray {
    private TreeMap<Integer, Integer>[] arr;
    private int curId = 0;

    public SnapshotArray(int length) {
        arr = new TreeMap[length];
        for (int i = 0; i < length; i++) {
            arr[i] = new TreeMap<>();
            arr[i].put(0, 0);
        }
    }
    
    public void set(int index, int val) {
        arr[index].put(curId, val);
    }
    
    public int snap() {
        return curId++;
    }
    
    public int get(int index, int snap_id) {
        return arr[index].floorEntry(snap_id).getValue();
    }
}
```

__Python__:

```Python
class SnapshotArray:

    def __init__(self, length: int):
        self.data = [{0: 0} for _ in range(length)]
        self.snap_id = 0


    def set(self, index: int, val: int) -> None:
        self.data[index][self.snap_id] = val


    def snap(self) -> int:
        self.snap_id += 1
        return self.snap_id - 1


    def get(self, index: int, snap_id: int) -> int:
        if snap_id in (d := self.data[index]):
            return d[snap_id]
        return d[(k := list(d.keys()))[bisect_left(k, snap_id) - 1]]


# Your SnapshotArray object will be instantiated and called as such:
# obj = SnapshotArray(length)
# obj.set(index,val)
# param_2 = obj.snap()
# param_3 = obj.get(index,snap_id)
```
