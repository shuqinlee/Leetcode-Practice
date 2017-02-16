## 49. Group Anagrams

My code:

```python
from collections import defaultdict

class Solution(object):
    def groupAnagrams(self, strs):
	    """
	    :type strs: List[str]
	    :rtype: List[List[str]]
	    """
	    table = defaultdict(list)
	    for s in strs:
	        temp = ''.join(sorted(s))
	        if temp in table: # discard has_key
	            table[temp].append(s)
	        else:
	            table[temp] = [s]
	    return table.values()
```



分类的重点在于将字符串所含元素相同的归为一类。

有两种区别方法：

1. 对字符串进行lexicalgraphcal排序;
2. 将字母与质数相对应，key的求法为字母对应质数的乘积（因为是质数，所以不会重复）这样abc 与 bac得到的key相同，abc 与 def的key不同。相当于帮助map求key.