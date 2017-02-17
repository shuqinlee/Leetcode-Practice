#### Simplify Path

Given an absolute path for a file (Unix-style), simplify it.

For example,
**path** = `"/home/"`, => `"/home"`
**path** = `"/a/./b/../../c/"`, => `"/c"`

**Corner Cases:**Did you consider the case where **path** = `"/../"`?
In this case, you should return `"/"`.Another corner case is the path might contain multiple slashes `'/'` together, such as `"/home//foo/"`.
In this case, you should ignore redundant slashes and return `"/home/foo"`.

My code: 

```python
class Solution(object):
    def simplifyPath(self, path):
        """
        :type path: str
        :rtype: str
        """
        stack = []
        for s in path.split('/'):
            if s == '..':
                if stack:
                    stack.pop()
            elif s and s != '.':
                stack.append(s)
        return '/'+'/'.join(stack)
```

.. = parent folder

.  = current folder

思想：栈实现

另外看到一个C++版本的，写的很简洁：

```c++
// Credit to @monaziyi 
string simplifyPath(string path) {
    string res, tmp;
    vector<string> stk;
    stringstream ss(path);
    while(getline(ss,tmp,'/')) {
        if (tmp == "" or tmp == ".") continue;
        if (tmp == ".." and !stk.empty()) stk.pop_back();
        else if (tmp != "..") stk.push_back(tmp);
    }
    for(auto str : stk) res += "/"+str;
    return res.empty() ? "/" : res;
}
```

