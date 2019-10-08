---
layout:     post
title:      "查找"
date:       2019-10-6 10:56:14
author:     "dxc"
header-img: "img/post-bg-malaban.jpg"
tags:
    - 刷题
---
 
**1、 基本的二分查找：**  
```python
def binarySearch(array, target):
    left = 0
    right = len(array) - 1
    
    while left <= right:
        mid = (left + right) // 2  #重点
        if array[mid] == target:
            return mid
        elif array[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```
为什么 while 中是 <= 而不是 < 呢？  
因为初始区间[0, length-1]是左闭右闭区间，[left, left]时仍可以搜索。  

**2、 左侧边界的二分查找：**  
```python
def binarySearchLeft(array, target):
    left = 0
    right = len(array)
    
    while left < right:   #重点
        mid = (left + right) // 2
        if array[mid] == target:
            right = mid   #重点 一直向左搜索
        elif array[mid] < target:
            left = mid + 1
        elif array[mid] > target:
            right = mid
    
    #重点
    if left == len(array):
        return -1
    if array[left] != target:
        return -1
    
    return left
```
left < right保证搜索区间是个左闭右开区间。  
一直向右搜索left = mid + 1使left有可能等于len(array)。   
一直向左搜索会使left == 0，所以最后要判断array[0]是否等于target。  

**3、右侧边界的二分查找：**   
```python
def binarySearchRight(array, target):
    left = 0
    right = len(array)
    
    while left < right:
        mid = (left + right) // 2
        if array[mid] == target:
            left = mid + 1
        elif array[mid] < target:
            left = mid + 1
        elif array[mid] > target:
            right = mid
            
    if left == 0:
        return -1   #一直向左搜索导致区间为[0,0)，没找到
    if array[left-1] != target:
        return -1   #一直向右搜索需要判断最右元素是否等于target
            
    return left - 1
```

**4、总结：**  
资料：<https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/solution/er-fen-cha-zhao-suan-fa-xi-jie-xiang-jie-by-labula/>
（摘自上述链接）   
基本二分查找：  
> 因为我们初始化 right = nums.length - 1
所以决定了我们的「搜索区间」是 [left, right]
所以决定了 while (left <= right)
同时也决定了 left = mid+1 和 right = mid-1

> 因为我们只需找到一个 target 的索引即可
所以当 nums[mid] == target 时可以立即返回   

寻找左侧的二分查找：  
> 因为我们初始化 right = nums.length
所以决定了我们的「搜索区间」是 [left, right)
所以决定了 while (left < right)
同时也决定了 left = mid + 1 和 right = mid

> 因为我们需找到 target 的最左侧索引
所以当 nums[mid] == target 时不要立即返回
而要收紧右侧边界以锁定左侧边界     

寻找右侧的二分查找：   
> 因为我们初始化 right = nums.length
所以决定了我们的「搜索区间」是 [left, right)
所以决定了 while (left < right)
同时也决定了 left = mid + 1 和 right = mid

> 因为我们需找到 target 的最右侧索引
所以当 nums[mid] == target 时不要立即返回
而要收紧左侧边界以锁定右侧边界

> 又因为收紧左侧边界时必须 left = mid + 1
所以最后无论返回 left 还是 right，必须减一   
