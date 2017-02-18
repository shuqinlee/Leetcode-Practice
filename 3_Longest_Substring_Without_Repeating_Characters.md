## 3. Longest Substring Without Repeating Characters

#### **Problem** 

Given a string, find the length of the **longest substring** without repeating characters.

**Examples:**

Given `"abcabcbb"`, the answer is `"abc"`, which the length is 3.

Given `"bbbbb"`, the answer is `"b"`, with the length of 1.

Given `"pwwkew"`, the answer is `"wke"`, with the length of 3. Note that the answer must be a **substring**, `"pwke"` is a *subsequence* and not a substring.



#### **Code**

用两个思路实现了一下，一个是two pointer, 一个是slide window. 

**two pointer**

two pointer 指的是保存两个位置信息，至于两个位置信息是如何变化的，要根据题目。

本题中，除了two pointer, 还需要一个set, （set中不能有重复元素，Python底层实现是hash，所以查找操作是O(1)).

```python
# time complexity: O(n)
class Solution(object):
	def lengthOfLongestSubstring(self, s):
		"""
		:type s: str
		:rtype: int
		"""
		if not s or len(s) == 1: return s
		left = right = maxLen = 0
		uniStr = set()
		for right in xrange(len(s)): # O(n)
			if s[right] not in uniStr: # O(1)
				uniStr.add(s[right])
			else:
				while s[left] != s[right]:
					uniStr.remove(s[left])
					left += 1
				left += 1
			maxLen = max([maxLen, right-left])
		return maxLen
```





**slide window**

先简要说明slide window 是什么。顾名思义，就是一扇窗户从左到右滑过去。

```
[a b c d e f g h]
[a b c]
  [b c d]
    [c d e]
      [d e f]
        [e f g]
          [f g h]
 from http://stackoverflow.com/questions/8269916/what-is-sliding-window-algorithm-examples
```

我们关心窗户的两边，其实还是跟two pointer一样，关心两个位置。但是slide window 更强调通过一边，求另一边。（这是自己暂时的理解，还需要做其他相关的题）

```python
# time complexity: O(n)
class Solution(object):
	def lengthOfLongestSubstring(self, s):
		"""
		:type s: str
		:rtype: int
		"""
		if not s or len(s) == 1: return s
		pos = [-1] * 129 # stores the posision latest same character appeared 
		maxLen = left = 0
		for i in xrange(len(s)):
			# left index of unrepeated string that contains s[i]
			left = max([pos[ord(s[i])]+1, left]) 
			maxLen = max([maxLen, i-left+1])
			pos[ord(s[i])] = i
		return maxLen
```



**其他思路**

dynamic programming:

