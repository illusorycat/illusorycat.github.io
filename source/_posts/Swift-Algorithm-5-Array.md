---
title: Swift 算法-5-数组
date: 2024-06-04 11:07:31
tags: 算法
---
# 目录
[前言](#0)
[最大子数组和](#53)
[合并区间](#56)
[轮转数组](#189)
[除自身以外数组的乘积](#238)
[缺失的第一个正数](#41)

<h1 id="0">前言</h1>

本文题目来源 LeetCode ，主要用于记录笔者的一些解题思路和思考。<br>
本文的主题是普通数组。<br>

<h1 id="53">最大子数组和</h1>

LeetCode 53. 最大子数组和<br>
## 题目
给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。<br>
子数组是数组中的一个连续部分。<br>
## 解题思考
根据题目要求，我们要找出数组中 `[i,j]` 集合的和最大值。<br>
$$
[i,j] = \begin{cases}
[0...j] - [0...i - 1] & (0 < i \leq j) \\
[0...j] & (0 < j)
\end{cases}
$$
也就是说，我们只需要遍历一次数组，计算 `[0...i]` 的值，并取出最大值和最小值相减，即可得到答案。<br>
同时，存在一个临界条件，如果 `[0...i]` 刚好就是题目的答案，则对应的应减去的最小值是 0 。<br>
并且，使用最大值和最小值时需保证不是同一个值。<br>
所以我们直接在遍历数组时计算 `[0...j]` 的值，并取出前面计算得到的最小值，然后相减，再判断其是不是和最大值。<br>
## 代码
```swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var minInt = 0
        var maxAns = Int.min

        var sum = 0
        for num in nums {
            sum += num
            maxAns = max(maxAns, sum - minInt)
            minInt = min(minInt, sum)
        }

        return maxAns
    }
}
```
## 拓展解法
上面的思路还是不够直接，我们直接遍历数组计算 `[0...j]` ，判断 `[0...j]` 和 `[0...j - 1]` 的大小。<br>
```swift
class Solution {
    func maxSubArray(_ nums: [Int]) -> Int {
        var prev = 0
        var ans = nums[0]
        for num in nums {
            prev = max(prev + num, num)
            ans = max(ans, prev)
        }
        return ans
    }
}
```

<h1 id="56">合并区间</h1>

LeetCode 56. 合并区间<br>
## 题目
以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间 。<br>
## 解题思考
首先我们要先排序数组，排序的规则时以区间 start 做递增排序。<br>
然后判断后一个区间的 start 是否小于等于前一个区间的 end ，如果是则可以合并。<br>
## 代码
```swift
class Solution {
    func merge(_ intervals: [[Int]]) -> [[Int]] {
        var sorted = intervals.sorted { $0[0] < $1[0] }
        var ans = [[Int]]()
        var i = 0
        while i < sorted.count {
            var interval = sorted[i]
            while i < sorted.count && sorted[i][0] <= interval[1] {
                interval[1] = max(interval[1], sorted[i][1])
                i += 1
            }
            ans.append(interval)
        }
        return ans
    }
}
```

<h1 id="189">轮转数组</h1>

LeetCode 189. 轮转数组<br>
## 题目
给定一个整数数组 nums，将数组中的元素向右轮转 k 个位置，其中 k 是非负数。<br>
## 解题思考

1. 第一种解题思路
    最简单的，使用一个新的数组，然后根据轮转要求，将旧数组移动到新数组
2. 第二种解题思路
    遍历数组，将下标 i 上的元素移动到 i + k 的位置上，再把 i + k 上的元素移动到 i + k + k 上。<br>
    在某些情况下，整个数组的元素会形成一个闭环。
    在某些情况下，数组中的元素会形成多个互相独立的闭环。因此你需要遍历这些闭环。比如你从下标 0 开始，移动元素 n 次后又回到了下标 0 。但是这时候数组中有些元素还没有被移动的。<br>
    根据笔者的实验，如果数组中存在 n 个闭环，则可以将数组开头的 n 个元素作为每个闭环的开头。<br>
    那怎么计算数组有几个闭环呢，答案就是数组的闭环数量等于数组元素个数 `count` 和轮转数 `k` 的最大公约数。<br>
3. 第三种解题思路
    我们举个实例的例子。现在有一个数组 [1, 2, 3, 4, 5, 6, 7] ，我们要轮转 3 个位置，显而易见答案是 [5, 6, 7, 1, 2, 3, 4] 。再仔细一看，可以看到下述的变换。<br>
    $$
    [1, 2, 3, 4, 5, 6, 7] \\
    => [7, 6, 5, 4, 3, 2, 1] \\
    => [5, 6, 7, 4, 3, 2, 1] \\
    => [5, 6, 7, 1, 2, 3, 4]
    $$
    其实也就是去翻转数组的某个区间

## 代码
```swift
class Solution{
    func rotate(_ nums: inout [Int], _ k: Int) {
        let count = nums.count
        let times = k % count
        guard times > 0 else { return }
        let origin = nums
        for i in 0..<count {
            nums[(i + times) % count] = origin[i]
        }
    }

    func rotate_2(_ nums: inout [Int], _ k: Int) {
        guard k > 0 else { return }
        let count = nums.count
        let times = gcd(count, k)
        let loop = count / times
        for i in 0..<times {
            var prev = nums[i]
            var nextIndex = i
            for j in 0..<loop {
                nextIndex = (nextIndex + k) % count
                swap(&prev, &nums[nextIndex])
            }
        }
    }

    func gcd(_ a: Int, _ b: Int) -> Int {
        var a = a
        var b = b
        while b != 0 {
            let r = a % b
            a = b
            b = r
        }
        return a
    }

    func rotate_3(_ nums: inout [Int], _ k: Int) {
        let count = nums.count
        let step = k % count
        guard step > 0 else { return }

        nums.reverse()
        nums[0..<step].reverse()
        nums[step..<count].reverse()
    }
}
```

<h1 id="238">除自身以外数组的乘积</h1>

LeetCode 238. 除自身以外数组的乘积<br>
## 题目
给你一个整数数组 nums，返回 数组 answer ，其中 answer[i] 等于 nums 中除 nums[i] 之外其余各元素的乘积 。<br>
题目数据 保证 数组 nums之中任意元素的全部前缀元素和后缀的乘积都在  32 位 整数范围内。<br>
请 不要使用除法，且在 O(n) 时间复杂度内完成此题。<br>
## 解题思考
可以计算前缀乘积和后缀乘积，然后再计算出除自身以外数组的乘积。<br>
一般情况下，使用两个数组分别存储前缀和后缀乘积。<br>
可以只使用一个数组，来解决前缀乘积和后缀乘积以及最终结果的存储吗？<br>
## 代码
```swift
class Solution{
    func productExceptSelf(_ nums: [Int]) -> [Int] {
        let count = nums.count
        var product = 1
        var ans = Array(repeating: 1, count: nums.count)
        for i in 0..<count {
            ans[i] = product
            product *= nums[i]
        }

        product = 1
        for i in stride(from: count - 1, through: 0, by: -1) {
            ans[i] *= product
            product *= nums[i]
        }

        return ans
    }
}
```

<h1 id="41">缺失的第一个正数</h1>

LeetCode 41. 缺失的第一个正数<br>
## 题目
给你一个未排序的整数数组 nums ，请你找出其中没有出现的最小的正整数。<br>
请你实现时间复杂度为 O(n) 并且只使用常数级别额外空间的解决方案。<br>
## 解题思考
设置一个阈值，从 0 开始。遍历数组，如果大于阈值，判断是否与阈值临近，然后临近，提升阈值。如果不是，暂时记录这个值。<br>
比如现在有一个数组 [-1, 0, 1, 2, 4, 3, 7,9]，将阈值从 0 开始， 遍历数组，发现数据 -1 ，0 时，直接丢弃。遍历到 1，2 时，将阈值分别提升到 1，2。遍历到 4 的时候，因为这个数值大于阈值，但是又与阈值不连续，所以暂时先存储起来，我们使用 Set 结构存储。再遍历到 3 时，因为与阈值连续，所以提升阈值到 3 ，同时判断到 Set 中有跟阈值连续的 4 ，所以将 4 从 Set 中移除，然后提升阈值到 4 。以此类推...<br>
## 代码
```swift
class Solution {
    func firstMissingPositive(_ nums: [Int]) -> Int {
        var threshold = 0
        var set = Set<Int>()

        for num in nums {
            if num > threshold {
                set.insert(num)
                while set.contains(threshold + 1) {
                    threshold += 1
                    set.remove(threshold)
                }
            }
        }

        return threshold + 1
    }
}
```
## 拓展解法
事实上，上述解法的空间复杂度是 O(n) 。但是题目要求使用常数级别额外空间。<br>
事实上，对于长度为 N 的数组，其缺失的第一个正数肯定在 [1...N + 1] 范围内。<br>
然后，我们在遍历数组之时，如果发现了这个范围内的数，我们可以做标记。最后查询第一个没有出现的标记即为答案。<br>
很明显，这个标识可以与数组绑定在一起，那我们就可以当找到数字 i 时，将下标为 i - 1 位置上的元素做个标记，因为我们只关注正数，所以我们可以使用负数来做标记。<br>
那对于原先就存在的负数，因为这些数据是我们不关注的数据，所以我们可以一开始就将所有的负数改为 N + 1 。因为 N + 1 这个数也不会影响结果。<br>
最后，我们就可以遍历数组，当找到第一个元素不是负数的下标 index 时，答案就是 index + 1 。<br>
```swift
class Solution {
    func firstMissingPositive(_ nums: [Int]) -> Int {
        let count = nums.count
        var nums = nums
        for i in 0..<count {
            if nums[i] <= 0 {
                nums[i] = count + 1
            }
        }
        for num in nums {
            if abs(num) <= count {
                nums[abs(num) - 1] = -abs(nums[abs(num) - 1])
            }
        }
        for i in 0..<count {
            if nums[i] > 0 {
                return i + 1
            }
        }
        return count + 1;
    }
}
```
