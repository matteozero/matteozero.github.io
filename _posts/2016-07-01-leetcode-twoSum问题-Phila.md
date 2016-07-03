---
layout: post
title: leetcode Two Sum问题
date: 2016-07-01 15:34:24.000000000 +09:00
---

Two Sum问题是[leetcode](https://leetcode.com/problems/two-sum/)上面的的第一题,题目是这样的：

```
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

Example:
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

给定一个整型数组，从中找出2个数等于给定的target。返回这两个数的下标(题目规定只有一组解)。
 
###暴力解法
我首先想到的是暴力解法，2个for循环，遍历数组，找到符合条件的2个数。代码如下

``` swift
func twoSum(arr: [Int],target: Int) -> [Int]
{
    var result = [Int]()
    for i in 0..<arr.count
    {
        for j in i+1..<arr.count
        {
            if arr[i] + arr[j] == target
            {
                result.append(i)
                result.append(j)
            }
        }
    }
    return result

}
```

这个算法的时间复杂度是0(N²)。


###排序解法
其实可以先对数组进行排序，然后从两头开始向中间查找。

``` swift
func twoSum_sorted(arr: [Int],target: Int) -> [Int]
{
    let sortArr = arr.sort()
    var left = 0
    var right = sortArr.count - 1
    var result = [Int]()
    while left < right
    {
        if sortArr[left] + sortArr[right] > target
        {
            right -= 1
        }else if sortArr[left] + sortArr[right] < target
       
        {
            left += 1
        }else
        {
            result.append(left)
            result.append(right)
            right -= 1
            left += 1
        }
    }
    return result
}
```

swif的sort()算法为快速排序，时间复杂度为0(n log n)，while循环里的查找算法时间复杂度 为O(N)，所以`twoSum_sorted()`的时间复杂度为0(n log n)。但是这个算法找到的结果是排序后数组元素的下标，并不能找到排序前元素的下标。不符合题目的要求。


###使用字典
其实有一个更简单的方法，就是动态规划。具体来说，使用字典来保存值和值在数组中对应的下标。遍历数组，查询字典中是否存在 target-arr[i],如果不存在，就把第i个元素和下标加入字典。如果存在，代表答案已经找到了，返回i和dict[target-arr[i]]。

``` swift
func twoSum_dict(arr: [Int],target: Int) -> [Int]
{
    var dict = [Int:Int]()
    var result = [Int]()
    
    for i in 0..<arr.count
    {
        guard let lastIndex = dict[target - arr[i]] else
        {
            dict[arr[i]] = i
            continue
        }
        result.append(lastIndex)
        result.append(i)
    }
    return result
}
```

这个算法的时间复杂度为O(n).


