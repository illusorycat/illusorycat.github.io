---
title: Swift 算法-1-哈希
date: 2024-05-23 11:01:33
tags: Swift, 算法
---
# 目录
[前言](#0)
[两数之和](#1)
[字母异位词分组](#49)
[最长连续序列](#128)

<h1 id="0">前言</h1>

本文题目来源 LeetCode ，主要用于记录笔者的一些解题思路和思考。<br>
本文的主题是哈希。<br>

<h1 id="1">两数之和</h1>

LeetCode 1. 两数之和<br>
## 题目
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。<br>
你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。<br>
你可以按任意顺序返回答案。<br>
## 解题思考
题目中提到了可以假定每种输入只会对应一个答案，因此我们找到答案后立即返回。<br>
题目要求每个元素只能使用一次，但是并没有说元素的值是否会重复。不过只本题中只有前者条件是有意义的。<br>
暴力解法，遍历数组中的所有元素 `x` ，同时遍历剩余的元素 `y` ，判断 `x + y == target` 。<br>
优化解法，在遍历数组元素 `x` 时，查找是否存在 `target - y` 。同时在编译时将元素 `x` 与它的下标 `index` 插入哈希表保存。<br>
## 代码
```Swift
class Solution {
    func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
        var hashtable = [Int: Int]()
        for (i, e) in nums.enumerated() {
            if let v = hashtable[target - e] {
                return [i, v]
            }
            hashtable[e] = i
        }
        return []
    }
}
```

<h1 id="49">字母异位词分组</h1>

LeetCode 49. 字母异位词分组<br>
## 题目
给你一个字符串数组，请你将**字母异位词**组合在一起。可以按任意顺序返回结果列表。<br>
字母异位词是由重新排列源单词的所有字母得到的一个新单词。<br>
## 解题思路
字母异位词指两个字符串中，字母值和数量一致。<br>
因此我们需要考虑怎么怎么表示一组字母异位词，最简单的方法就是用字符串的字典序字符串来表示.然后使用哈希表存储，最后再转换成数组输出。<br>
## 代码
```Swift
class Solution {
    func groupAnagrams(_ strs: [String]) -> [[String]] {
        var hashList = [String: [String]]()
        for str in strs {
            let key = String(str.sorted())
            if hashList[key] == nil {
                hashList[key] = [str]
            } else {
                hashList[key]!.append(str)
            }
        }
        return Array(hashList.values)
    }
}
```

<h1 id="128">最长连续序列</h1>

LeetCode 128. 最长连续序列<br>
## 题目
给定一个未排序的整数数组 nums ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。<br>
请你设计并实现时间复杂度为 O(n) 的算法解决此问题。<br>
## 解题思路
最长序列，也就是说我们要找出数组中的连续序列，并获得其中最大的长度。<br>
题目并没有说数组元素不重复，因此我们要先去重，最简单的就是使用 Set 。<br>
接着，我们要在数组中找到一个序列 `x, x+1, x+2,x+n` ，则该序列长度就是 `n` 。<br>
暴力搜索，遍历每个元素 `x` ，然后通过 `+1` 查找序列。
优化，因为暴力搜索会出现对序列的子序列重复查找。因此，我们先通过判断 `x-1` 是否中数组中，找到序列中的最小值，再开始查找整个序列，获取序列长度。<br>
## 代码
```Swift
class Solution {
    func longestConsecutive(_ nums: [Int]) -> Int {
        var num_set = Set<Int>(nums)
        
        var longestStreak = 0
        
        for num in num_set {
            if !num_set.contains(num - 1) {
                var currentStreak = 1
                var currentNum = num
                
                while num_set.contains(currentNum + 1) {
                    currentNum = currentNum + 1
                    currentStreak += 1
                }
                
                longestStreak = max(longestStreak, currentStreak)
            }
        }
        
        return longestStreak
    }
}
```