# 91. Decode Ways

### Problem

A message containing letters from `A-Z` is being encoded to numbers using the following mapping:

```
'A' -> 1
'B' -> 2
...
'Z' -> 26

```

Given an encoded message containing digits, determine the total number of ways to decode it.

For example,
Given encoded message `"12"`, it could be decoded as `"AB"` (1 2) or `"L"` (12).

The number of ways decoding `"12"` is 2.



### Solution

**Idea: dynamic programming**

Identify the connection between subproblem and larger problem, at last, solve the final problem. For this problem, we only need to store one more subproblem's result so that the space complextity is O(1).

```python
# space complexity: O(1)
# time complexity: O(n)
class Solution(object):
	def numDecodings(self, s):
		"""
		:type s: str
		:rtype: int
		"""
		if len(s) == 0 or s == '0': return 0
		elif len(s) == 1: return 1
		before2 = 1
		before1 = (s[0] != '0')
		
		for i in range(1,len(s)):
			temp = before1
			before1 = before1 * (s[i] != '0')+ ((int(s[i-1:i+1]) <27) and s[i-1] != '0') * before2
			before2 = temp
		return before1	
```



