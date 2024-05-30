---
title: Swift 算法-4-子串
date: 2024-05-30 10:11:33
tags: 算法
---
# 目录
[前言](#0)
[和为 K 的子数组](#560)
[滑动窗口最大值](#239)
[最小覆盖子串](#76)

<h1 id="0">前言</h1>

本文题目来源 LeetCode ，主要用于记录笔者的一些解题思路和思考。<br>
本文的主题是子串。<br>

<h1 id="560">和为 K 的子数组</h1>

LeetCode 560. 和为 K 的子数组<br>
## 题目
给你一个整数数组 nums 和一个整数 k ，请你统计并返回 该数组中和为 k 的子数组的个数 。<br>
子数组是数组中元素的连续非空序列。<br>
## 解题思考
像这一类的题目，本质上都是找出子串。那子串有两个标识，一个是子串开头，一个是子串长度。<br>
我们可以遍历数组 nums ，将其每个元素作为子串开头，再来找出以该元素作为子串开头的所有子串中满足条件的子串。<br>
## 代码
```swift
class Solution {
    func subarraySum(_ nums: [Int], _ k: Int) -> Int {
        var ans = 0
        for i in 0..<nums.count {
            var sum = 0
            for j in i..<nums.count {
                sum += nums[j]
                if sum == k {
                    ans += 1
                }
            }
        }
        return ans
    }
}
```
## 拓展解法
上述遍历的方法的时间复杂度是 O($n^2$)。<br>
我们可以使用数学来获得另一个解法。<br>
$$[i...j] == k  => [0...j] = k + [0...i-1] (0<i \leq j)$$
$$[0...j] == k (0 <= j)$$
问题就简化成了计算数组中 $[0...j]$ 的值，然后再判断是否存在与之相等的 $k + [0...i - 1]$ 。我们可以使用哈希表来存储 $[0...j]$ 的值。<br>
实际上在编写代码时我们一般会使用 $[0...j] - K == [0...i - 1]$ ，因此有一种边界情况需要处理，$[0...j] == k$ 。这种特殊情况下，是不存在 $[0...i - 1]$ 子串的，所以我们需要在初始化哈希表时对这种情况做处理。<br>
```swift
class Solution {
    func subarraySum(_ nums: [Int], _ k: Int) -> Int {
        var ans = 0
        var map = [Int: Int]()
        map[0] = 1
        var sum = 0
        for num in nums {
            sum += num
            if let count = map[sum - k] {
                ans += count
            }
            map[sum] = (map[sum] ?? 0) + 1
        }
        return ans
    }
}
```

<h1 id="239">滑动窗口最大值</h1>

LeetCode 239. 滑动窗口最大值<br>
## 题目
给你一个整数数组 nums，有一个大小为 k 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 k 个数字。滑动窗口每次只向右移动一位。<br>
返回 滑动窗口中的最大值 。
## 解题思考
找到滑动窗口中的最大值，一个直接的思路就是，遍历数组，在滑动窗口中，如果后一个元素比当前滑动窗口中找到的最大值大，那这个就是新的最大值。如果后一个元素比较小，那这个元素仍然得保留，因为有可能是滑动窗口移动过程中，前面的那个最大值会被移出滑动窗口，而这个元素可能成为最大值。<br>
所以首先我们应该有一个存储结构 `queue_num` ，存储遍历过程中找到的最大值。在遍历一个新的元素时，我们要从尾部往头部比较，把小于这个元素的值移出存储结构。<br>
同时，有个问题就是，当滑动窗口移动时，我们怎么判断 `queue_num` 中的元素已经不在滑动窗口中，应该移除。因此我们对于 `queue_num` 中的元素，我们要使用一个存储结构 `queue` 保存其下标，当下标不在滑动窗口时，应该移除 `queue_num` 中的元素。<br>
同时，因为我们已经在 `queue` 中保存了元素的下标，同时其排序与 `queue_num` 中是一一对应的。所以我们可以不使用 `queue_num` 存储结构，而是通过 `queue` 和 `nums` 数组本身找到对应的最大值。<br>
## 代码
```swift
class Solution {
    func maxSlidingWindow(_ nums: [Int], _ k: Int) -> [Int] {
        var queue = [Int]()
        var ans = [Int]()

        for i in 0..<k {
            while !queue.isEmpty && nums[queue.last!] <= nums[i] {
                queue.removeLast()
            }
            queue.append(i)
        }

        ans.append(nums[queue.first!])

        for i in k..<nums.count {
            while !queue.isEmpty && nums[queue.last!] <= nums[i] {
                queue.removeLast()
            }
            while !queue.isEmpty && queue.first! < i - k + 1 {
                queue.removeFirst()
            }
            queue.append(i)
            ans.append(nums[queue.first!])
        }

        return ans
    }
}
```

<h1 id="76">最小覆盖子串</h1>

LeetCode 76. 最小覆盖子串<br>
## 题目
给你一个字符串 s 、一个字符串 t 。返回 s 中涵盖 t 所有字符的最小子串。如果 s 中不存在涵盖 t 所有字符的子串，则返回空字符串 "" 。<br>
## 解题思考
找子串的第一个思路，遍历字符串，找出以元素作为子串开头的子串集。<br>
题目要求的是最小子串，因此我们只需要找到以每个元素作为开头的第一个子串。<br>
怎么判断子串是我们要找的呢。我们先给 t 字符串创建一个哈希表，记录 t 中出现的字母及次数。接着在找子串时通过这个哈希表对比。<br>
我们在建立哈希表时，如果是 t 中存在的字符串，我们就在哈希表对应的字母下 `+ 1` ,如果是子串中存在的字符串我们就是在对应的字母下 `- 1` 。很明显，我们并不关心不在 t 中出现的字母的情况，因此在找子串时，如果出现的字母没有在 t 中出现，我们之间跳过处理。很明显，最小子串如果存在，一定是以 t 中存在的字母作为开头和结尾的字母。<br>
我们使用一个变量 `diff` 记录子串和 t 的差值。因为允许子串中的对应字母数量大于 t 中的数量，因此只有当哈希表中字母的数量在大于零时 `-1` ，我们才 `diff -= 1` ，当字母的数量在等于零时 `+1` ，我们才 `diff += 1` 。当 `diff == 0` 时，表示我们找到了子串。<br>
我们可以使用两个指针，一个指向子串开头，一个指向子串结尾。当 `diff != 0` 时移动尾指针， 当 `diff == 0` 时记录子串，然后移动头指针。存在一种情况，当 `diff != 0 && right >= s.count` 时，这个时候，在剩下的子串中，已经不可能出现 `diff == 0` 的情况，所以我们就可以中断查找。<br>
## 代码
```swift
class Solution {
    func minWindow(_ s: String, _ t: String) -> String {
        var sArray = Array(s)
        var count = sArray.count
        var len = Int.max
        var begin = -1
        var end = -1    //end 指向子串末尾元素的下一位置

        var map = [Character: Int]()
        var diff = 0
        for c in t {
            map[c] = (map[c] ?? 0) + 1
            diff += 1
        }

        var left = 0
        var right = 0
        while left < count || right < count {
            while diff != 0 && right < count {
                if let num = map[sArray[right]] {
                    if num > 0 {
                        diff -= 1
                    }
                    map[sArray[right]] = num - 1
                }
                right += 1
            }
            while diff == 0 && left < count {
                if let num = map[sArray[left]] {
                    if diff == 0 {
                        if right - left < len {
                            begin = left
                            end = right
                            len = end - begin
                        }
                    }
                    if num >= 0 {
                        diff += 1
                    }
                    map[sArray[left]] = num + 1
                }
                left += 1
            }
            if diff != 0 && right >= count {
                break
            }
        }

        return begin == -1 ? "" : String(sArray[begin..<end])
    }
}
```
