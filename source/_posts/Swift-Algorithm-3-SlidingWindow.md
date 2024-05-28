---
title: Swift 算法-3-滑动窗口
date: 2024-05-28 10:23:46
tags: 算法
---
# 目录
[前言](#0)
[无重复字符的最长子串](#3)
[找到字符串中所有字母异位词](#438)

<h1 id="0">前言</h1>

本文题目来源 LeetCode ，主要用于记录笔者的一些解题思路和思考。<br>
本文的主题是滑动窗口。<br>

<h1 id="3">无重复字符的最长子串</h1>

LeetCode 3. 无重复字符的最长子串<br>
## 题目
给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串的长度。<br>
## 解题思考
使用一个滑动窗口[left, right]。右移 right 指针遍历字符串，使用哈希表记录字母的下标。当遇到重复字母时，判断前一个位置 prev 是否在滑动窗口中，如果是，移动 left 指针到 prev + 1 。<br>
遍历过程中实时更新滑动窗口长度和最长子串长度。<br>
## 代码
```swift
class Solution {
    func lengthOfLongestSubstring(_ s: String) -> Int {
        var hashTable = [Character: Int]()
        var maxLen = 0
        var left = 0
        for (i, c) in s.enumerated() {
            if let index = hashTable[c] {
                left = max(left, index + 1)
            }
            maxLen = max(maxLen, i - left + 1)
            hashTable[c] = i
        }
        return maxLen
    }
}
```

<h1 id="438">找到字符串中所有字母异位词</h1>

LeetCode 438. 找到字符串中所有字母异位词<br>
## 题目
给定两个字符串 s 和 p，找到 s 中所有 p 的 异位词 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。<br>
异位词 指由相同字母重排列形成的字符串（包括相同的字符串）。<br>
## 解题思考
字符串 p 的长度 pLen 是固定的，所以就是遍历字符串 s ，取出其中长度为 pLen 的子串判断与字符串 p 是不是字母异位词。这就相当于一个定长的滑动窗口。<br>
重点是如何判断字母异位词。<br>
在之前哈希题目中，我们判断字母异位词的方法是将字符串排序，字典序一致的是字母异位词。<br>
但是在这道题中，如果我们每次都截取出子串再排序，未免有些繁琐，并且每个子串直接存在很大的重叠部分，多了很多重复排序。<br>
因此我们使用一个结构，存储字符串 p 中的字母种类和对应数量。同时使用另一个结构存储字符串 s 中的滑动窗口中的字母种类和对应数量。然后判断是否一致。<br>
对于上述判断，我们可以使用做差法。字符串 p 中每个字母以 -1 累计。字符串 s 的滑动窗口中每个字母以 +1 累计，移除滑动窗口则 -1 ，移入滑动窗口则 +1 。这样子就将两个结构合并，并且直接累计所有字母种类的差值是否等于零即可以判断是否是字母异位词。<br>
## 代码
```swift
class Solution {
    func findAnagrams(_ s: String, _ p: String) -> [Int] {
        let asciiA = Int(Character("a").asciiValue!)
        var ans = [Int]()
        let sArray = s.unicodeScalars.map({Int($0.value) - asciiA})
        let pArray = p.unicodeScalars.map({Int($0.value) - asciiA})

        let sCount = sArray.count
        let pCount = pArray.count
        guard sCount >= pCount else { return ans }

        var count = Array(repeating: 0, count: 26)
        for i in 0..<pCount {
            count[sArray[i]] += 1
            count[pArray[i]] -= 1
        }

        var diff = 0
        count.forEach({diff += abs($0)})

        if diff == 0 {
            ans.append(0)
        }

        for i in pCount..<sCount {
            if count[sArray[i - pCount]] > 0 {
                diff -= 1
            }
            else {
                diff += 1
            }
            count[sArray[i - pCount]] -= 1

            if count[sArray[i]] < 0 {
                diff -= 1
            }
            else {
                diff += 1
            }
            count[sArray[i]] += 1

            if diff == 0 {
                ans.append(i - pCount + 1)
            }
        }

        return ans
    }
}
```
