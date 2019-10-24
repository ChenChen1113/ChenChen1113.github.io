---
layout:     post
title:      "leetcode 46-全排列、47-全排列II  （多题联合练习回溯，未完）"
date:       2019-10-22 21:57:52
author:     "dxc"
header-img: "img/post-bg-rwd.jpg"
tags:
    - 刷题
---

**46-全排列**  
给定一个没有重复数字的序列，返回其所有可能的全排列。  
> 输入: [1,2,3]  
输出:  
[  
  [1,2,3],  
  [1,3,2],  
  [2,1,3],  
  [2,3,1],  
  [3,1,2],  
  [3,2,1]  
]  

回溯：深度优先遍历 + 状态重置 + 剪枝
递归方法返回后，要做两件事：  
（1）释放对最后一个数的占用  
（2）将最后一个数从当前选取的排列中弹出  
程序第 1 次走到一个结点的时候，表示考虑一个数，要把它加入列表，经过更深层的递归又回到这个结点的时候，需要“状态重置”、“恢复现场”，需要把之前考虑的那个数从末尾弹出，这都是在一个列表的末尾操作，最合适的数据结构是栈（Stack）。  
```python
class Solution(object):
    def permute(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if len(nums) == 0:
            return []
            
        res = []
        used = [False] * len(nums)
        
        self.backtrack(nums, used, res, [])
        
        return res
    
    
    def backtrack(self, nums, used, res, pre):
        if len(pre) == len(nums):
            res.append(pre[:])
            return

        for i in range(len(nums)):
            if used[i] == False:
                pre.append(nums[i])
                used[i] = True

                self.backtrack(nums, used, res, pre)

                used[i] = False #递归前后的代码是对称的   
                pre.pop()
```

资料：<https://leetcode-cn.com/problems/permutations/solution/hui-su-suan-fa-python-dai-ma-java-dai-ma-by-liweiw/>

**47-全排列II**  
给定一个可包含重复数字的序列，返回所有不重复的全排列。  
> 输入: [1,1,2]  
输出:  
[  
  [1,1,2],  
  [1,2,1],  
  [2,1,1]  
]  

```python
class Solution(object):
    def permuteUnique(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        if len(nums) == 0:
            return []
        
        nums.sort()
        res = []
        used = [False] * len(nums)
        self.backtrack(res, nums, used, [])
        return res

    def backtrack(self, res, nums, used, pre):
        if len(nums) == len(pre):
            res.append(pre[:])
            return

        for i in range(len(nums)):
            if i > 0 and nums[i] == nums[i-1] and used[i-1] == True:    #注意这里    
                continue
            if used[i] == False:
                pre.append(nums[i])
                used[i] = True

                self.backtrack(res, nums, used, pre)

                used[i] = False
                pre.pop()
```