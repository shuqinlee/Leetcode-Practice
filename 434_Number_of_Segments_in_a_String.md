# 434. Number of Segments in a String



### Problem

Count the number of segments in a string, where a segment is defined to be a contiguous sequence of non-space characters.

Please note that the string does not contain any **non-printable** characters.

**Example:**

```
Input: "Hello, my name is John"
Output: 5
```

(Easy)



### Solution

Just traverse through the string, determine whether it's a new segment.

```python
class Solution(object):
    def countSegments(self, s):
        """
        :type s: str
        :rtype: int
        """
        if s == '': return 0
        newSeg = False
        cnt = 0
        for i in s:
            if i != ' 'and not newSeg:
                newSeg = True
                cnt += 1
            elif i == ' ':
                newSeg = False
        return cnt
```



简洁版：

```python
class Solution(object):
	    def countSegments(self, s):
		"""
		:type s: str
		:rtype: int
		"""
		if s == '': return 0
		s = ' ' + s.rstrip()
		cnt = 0
		for i in range(len(s)-1):
			cnt = s[i] == ' ' and s[i+1] != ' '
		return cnt
```

### Learn

今天想加一点tips:

平常总想取得内容和索引，实现方法很ugly, 明明可以用 enumerate!

enumerate 接受的参数是 iterable 对象或可迭代对象，第二个参数可选，用于设置起始index的值。

下面是用例：

```python
# enumerate
num = xrange(5)

print('Test 1:')
for ind, c in enumerate(num):
	print([ind,c])

print('\n\nTest 2:')
for ind, c in enumerate(num,8):
	print([ind,c])

# 统计行数的快速方法
count = -1 
for index, line in enumerate(open(filepath,'r'))： 
    count += 1
```

```
Test 1:
[0, 0]
[1, 1]
[2, 2]
[3, 3]
[4, 4]

Test 2:
[8, 0]
[9, 1]
[10, 2]
[11, 3]
[12, 4]
```