__Description__:
Given an Iterator class interface with methods: next() and hasNext(), design and implement a PeekingIterator that support the peek() operation -- it essentially peek() at the element that will be returned by the next call to next().

__Example:__

Assume that the iterator is initialized to the beginning of the list: [1,2,3].

Call next() gets you 1, the first element in the list.
Now you call peek() and it returns 2, the next element. Calling next() after that still return 2. 
You call next() the final time and it returns 3, the last element. 
Calling hasNext() after that should return false.

__Follow up:__
How would you extend your design to be generic and work with all types, not just integer?

__题目描述__:
给定一个迭代器类的接口，接口包含两个方法： next() 和 hasNext()。设计并实现一个支持 peek() 操作的顶端迭代器 -- 其本质就是把原本应由 next() 方法返回的元素 peek() 出来。

__示例 :__

假设迭代器被初始化为列表 [1,2,3]。

调用 next() 返回 1，得到列表中的第一个元素。
现在调用 peek() 返回 2，下一个元素。在此之后调用 next() 仍然返回 2。
最后一次调用 next() 返回 3，末尾元素。在此之后调用 hasNext() 应该返回 false。

__进阶：__
你将如何拓展你的设计？使之变得通用化，从而适应所有的类型，而不只是整数型？

__思路__:
维护一个缓存, 一个 bool变量检查是否到结尾
每次调用 peek的时候存入缓存
每次调用 next的时候先查看缓存
时间复杂度O(n), 空间复杂度O(1)

__代码__:
__C++__:
/*
 * Below is the interface for Iterator, which is already defined for you.
 * **DO NOT** modify the interface for Iterator.
 *
 *  class Iterator {
 *		struct Data;
 * 		Data* data;
 *		Iterator(const vector<int>& nums);
 * 		Iterator(const Iterator& iter);
 *
 * 		// Returns the next element in the iteration.
 *		int next();
 *
 *		// Returns true if the iteration has more elements.
 *		bool hasNext() const;
 *	};
 */

class PeekingIterator : public Iterator {
public:
	PeekingIterator(const vector<int>& nums) : Iterator(nums) {
	    // Initialize any member here.
	    // **DO NOT** save a copy of nums and manipulate it directly.
	    // You should only use the Iterator interface methods.
	    is_end = false;
        if (Iterator::hasNext()) result = Iterator::next();
        else is_end = true;
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	int peek() {
        return result;
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	int next() {
	    int next = result;
        if (Iterator::hasNext()) result = Iterator::next();
        else is_end = true;
        return next;
	}
	
	bool hasNext() const {
	    return !is_end;
	}
private:
    int result;
    bool is_end;
};
```

__Java__:
```Java
// Java Iterator interface reference:
// https://docs.oracle.com/javase/8/docs/api/java/util/Iterator.html

class PeekingIterator implements Iterator<Integer> {
    private Integer result = null;
    private Iterator<Integer> iter;
    
	public PeekingIterator(Iterator<Integer> iterator) {
	    // initialize any member here.
	    iter = iterator;
	}
	
    // Returns the next element in the iteration without advancing the iterator.
	public Integer peek() {
        if (result == null) result = iter.next();
        return result;
	}
	
	// hasNext() and next() should behave the same as in the Iterator interface.
	// Override them if needed.
	@Override
	public Integer next() {
	    if (result == null) return iter.next();
        Integer next = result;
        result = null;
        return next;
	}
	
	@Override
	public boolean hasNext() {
	    return result != null || iter.hasNext();
	}
}
```

__Python__:
```Python
# Below is the interface for Iterator, which is already defined for you.
#
# class Iterator:
#     def __init__(self, nums):
#         """
#         Initializes an iterator object to the beginning of a list.
#         :type nums: List[int]
#         """
#
#     def hasNext(self):
#         """
#         Returns true if the iteration has more elements.
#         :rtype: bool
#         """
#
#     def next(self):
#         """
#         Returns the next element in the iteration.
#         :rtype: int
#         """

class PeekingIterator:
    def __init__(self, iterator):
        """
        Initialize your data structure here.
        :type iterator: Iterator
        """
        self.iterator = iterator
        self.result = None

        
    def peek(self):
        """
        Returns the next element in the iteration without advancing the iterator.
        :rtype: int
        """
        if self.result is None:
            self.result = self.iterator.next()
        return self.result
        

    def next(self):
        """
        :rtype: int
        """
        if self.result is None:
            return self.iterator.next()
        result, self.result = self.result, None
        return result
        

    def hasNext(self):
        """
        :rtype: bool
        """
        return self.result is not None or self.iterator.hasNext()
    
        

# Your PeekingIterator object will be instantiated and called as such:
# iter = PeekingIterator(Iterator(nums))
# while iter.hasNext():
#     val = iter.peek()   # Get the next element but not advance the iterator.
#     iter.next()         # Should return the same value as [val].
```