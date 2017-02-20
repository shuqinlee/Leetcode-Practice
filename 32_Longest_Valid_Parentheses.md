# 32.  Longest Valid Parentheses

### Problem

Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

For `"(()"`, the longest valid parentheses substring is `"()"`, which has length = 2.

Another example is `")()())"`, where the longest valid parentheses substring is `"()()"`, which has length = 4.

longest valid parentheses 以下简称 LVP

### Method

#### 思路1：stack + Dynamic programming

用栈来保存左括号的位置，遇到一个右括号则pop栈顶左括号；

除此之外，还需要维护一个list `pos`，因为知道最多有`len(s)`个右括号，大小提前设为`len(s)`，这个list用来保存相应位置的右括号之前的LVP的最左端. 通过栈中左括号位置这个信息我们可知道到右括号相匹配的左括号 长度，但我们还需要加上之前的长度。

举个例子，对于`()()`这种情况，（位置从0开始），第三个位置遇到右括号，pop左括号，即位置2，这样得到长度为2的完整括号长度，但位置2前面还有，所以还需要加上匹配的左括号前面那个符号的LVP，即2-1-pos[2-1]+1.

DP思想就体现在长度的更新上，每次更新都与前面的情况有关，依赖于substructure。

```python
# time complextity: O(n)
# space complextity: O(n), pos and left 
class Solution(object):
	    def longestValidParentheses(self, s):
		"""
		:type s: str
		:rtype: int
		"""
		lens, left, maxLen = len(s), [], 0
		pos = [i+1 for i in range(lens)]
		for i in xrange(lens):
			if s[i] == '(':
				left.append(i)
			elif left:
				t = pos[i] = left.pop()
				if t != 0:
					maxLen = max([maxLen, i - pos[i] + 1 + t - pos[t-1]])
					pos[i] = pos[t-1]
				else:
					maxLen = i - pos[i] + 1
		return maxLen
```



#### 思路2：Double direction scan 

摘自@TK, 写的很完整啦 

![D5C8C7FD-141D-4A61-AC9E-52EAEA5D5B7C](https://github.com/shuqinlee/Leetcode-Practice/blob/master/D5C8C7FD-141D-4A61-AC9E-52EAEA5D5B7C.png)

每种题目要求都有它内在的规律，经常一种算法出来的时候，往往都是发现了其中的某种rule，一旦发现了，就很愉快了 ；）。

```c++
class Solution {
	public:
		int longestValidParentheses(string s) {
			int n = s.size();
			if (n == 0) return 0;
			
			int ret = 0;
			
			// left to right
			int l = 0, r = 0;
			for (int i = 0;i < n; ++i) {
				if (s[i] == '(') {
					l++;
				} else {
					r++;
				}
				if (r > l) {
					l = r = 0;
				} else if (r == l){
					ret = max(ret, l*2);
				}
			}
			
			// right to left
			l = r = 0;
			for (int i = n-1; i >= 0; --i) {
				if (s[i] == '(') {
					l++;
				}else {
					r++;
				}
				if (l > r) {
					l = r = 0;
				} else if (l == r) {
					ret = max(ret, l*2);
				}
			}
			return ret;
		}
};
```

