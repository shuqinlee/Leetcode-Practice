# 503. Next Greater Element II

### Problem

Given a circular array (the next element of the last element is the first element of the array), print the Next Greater Number for every element. The Next Greater Number of a number x is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, output -1 for this number.

**Example 1:**

```
Input: [1,2,1]
Output: [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number; 
The second 1's next greater number needs to search circularly, which is also 2.

```

**Note:** The length of given array won't exceed 10000.



### Solution

#### 1. 回溯

思路是：从头i=0开始，查找比它大的j, 一旦找到，索引存入result;并从j-1的数开始往前更新到开头的i，寻找他们的nearest larger number。直到result 都填完.

本来不是很确定时间复杂度是多少，主要是因为回填的时候每个数最多需要填的次数不确定，像

`[…, 5,1,2,3,4,6,…]`这样的，回填5那个位置，就要check五次。但这样其他的又少了，所以应该是恒定的。不管是顺序（直接跳转）太多还是逆序太多（直接更新），都保证很快结束。

```python
# time complexity: O(n)
# space complexity: O(n)
class Solution(object):
	def index(self, lens, ind):
		return (ind+lens)%lens
	def inRange(self, left, right, i):#(left, right)
		if left < right:
			if left < i < right: return True
			else: return False
		elif right < left:
			if right <= i <= left: return False
			else: return True
		else: return False
	
	def nextGreaterElements(self, nums):
		"""
		:type nums: List[int]
		:rtype: List[int]
		"""
		i, lens, result = 0, len(nums), [-1] * lens 

		while i < lens:
			if i == lens -1:
				print('!')
			large = self.index(lens, i+1)
			while nums[large] <= nums[i] and large != i:
				large = self.index(lens, large+1)
				
			if large == i:
				i = i + 1
			else:
				result[i] = large
			    # backward
				result[self.index(lens, large-1)] = large
				j = self.index(lens, large-2) 
			
				while self.inRange(i, large, j):
					if nums[j] < nums[self.index(lens, j+1)]: # 直接更新
						result[j] = self.index(lens, j+1)
					else:
						t = j+1
						while nums[self.index(lens, t)] <= nums[j]:
							t = result[self.index(lens, t)] # 跳转
						result[j] = self.index(lens, t)
					j = self.index(lens, j - 1)
				i = i + (large - i + lens)% len(nums)
		ret = [-1 if i == -1 else nums[i] for i in result]
		return ret
```



#### 2. 栈实现

思路：准备一个栈，入栈的内容是数组下标，

1. 如果栈为空，或者栈顶元素大于等于当前元素，就把当前入栈，也就是说栈里的元素的排列是大->小，循环到数组的下一个元素
   2.一旦碰到栈顶元素比当前元素要小，就设置当前栈顶的元素的next greater number为当前循环的元素，并出栈，
2. 然后继续判断栈顶元素，重复1或者2
3. 第二遍遍历数组时，只要栈为空就结束循环，
4. 如果不为空，判断栈顶元素，大于等于当前元素，循环下一个元素，如果是栈顶元素小于当前元素，重复2， 5

```python
class Solution:
	def nextGreaterElements(self, nums):
		"""
		:type nums: List[int]
		:rtype: List[int]
		"""
		if not nums: return []
		stack, res = [], [-1] * len(nums)
		for i in xrange(-len(nums), len(nums)):
			while stack and nums[i] > nums[stack[-1]]:
				j = stack.pop()
				res[j] = nums[i]
			if i < 0:
				stack.append(i)
		return res
				
s = Solution()
print s.nextGreaterElements([6,5,1,7])
# [1,2,3,4,5,6,5,4,5,1,2,3]
```



（没处理的先放着）