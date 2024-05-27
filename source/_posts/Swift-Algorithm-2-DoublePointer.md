---
title: Swift 算法-1-双指针
date: 2024-05-25 18:19:14
tags: Swift, 算法
---
# 目录
[前言](#0)
[移动零](#283)
[盛最多水的容器](#11)
[三数之和](#15)
[接雨水](#42)

<h1 id="0">前言</h1>

本文题目来源 LeetCode ，主要用于记录笔者的一些解题思路和思考。<br>
本文的主题是双指针。<br>

<h1 id="283">移动零</h1>

LeetCode 283. 移动零<br>
## 题目
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。<br>
请注意 ，必须在不复制数组的情况下原地对数组进行操作。<br>
## 解题思考
最直接的思路，遍历数组，遇到 0 时，使用冒泡法将 0 交换到数组末尾。使用冒泡是为了保存非零元素的相对顺序。<br>
但是上述的时间复杂度是 O($n^2$)。<br>
事实上，我们可以先找到数组中第一个 0 的位置，然后使用指针 1 指向零区间的头部，使用指针 2 指向零区间之后的第一个位置，表示零区间的尾部。这样子我们就形成了一个零区间。<br>
然后从零区间尾部开始遍历。当找到 0 时，纳入零区间。当找到非零值时，将其与零区域的第一个元素交换，也就等价于移动零区间。<br>
## 代码
```Swift
class Solution {
    func moveZeroes(_ nums: inout [Int]) {
        var zeroBegin = nums.firstIndex(of: 0)
        guard var zeroBegin else { return }
        for i in (zeroBegin + 1)..<nums.count {
            if nums[i] != 0 {
                nums.swapAt(zeroBegin, i)
                zeroBegin += 1
            }
        }
    }
}
```

<h1 id="11">盛最多水的容器</h1>

LeetCode 11. 盛最多水的容器<br>
## 题目
给定一个长度为 n 的整数数组 height 。有 n 条垂线，第 i 条线的两个端点是 (i, 0) 和 (i, height[i]) 。<br>
找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。<br>
返回容器可以储存的最大水量。<br>
说明：你不能倾斜容器。<br>
示例图如下：<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405271346208.png)
## 解题思考
使用一个变量 maxArea 存储可能的最大水量。<br>
第一个可能的最大水量的第0条线 left 和最后一条线 right 组成的区域。最大水量公式是<br>
$$maxArea = min(height[left], height[right]) * (right - left)$$<br>
之后，不管是移动左指针还是移动右指针，其底边总是在缩短，所以最大水量如果能更大，只能是高在增大。<br>
对于高而已，受到 height[left] 和 height[right] 其中更小值的制约。所以是移动指向更小值的那个指针。<br>
## 代码
```swift
class Solution {
    func maxArea(_ height: [Int]) -> Int {
        var left = 0
        var right = height.count - 1
        var maxArea = 0
        while left < right {
            let curArea = min(height[left], height[right]) * (right - left)
            maxArea = max(curArea, maxArea)
            
            if height[left] < height [right] {
                left += 1
            }
            else {
                right -= 1
            }
        }
        return maxArea
    }
}
```

<h1 id="15">三数之和</h1>

LeetCode 15. 三数之和<br>
## 题目
给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请<br>
你返回所有和为 0 且不重复的三元组。<br>
注意：答案中不可以包含重复的三元组。<br>
## 解题思考
寻找三元组，先固定一个 i， 问题就可以简化为两数之和问题。但是，这儿的两数之和需要找出所有的可能性。<br>
使用双指针 + 排序。首先对数组做递增排序。然后使用双指针，一个 left ， 一个 right 。如果 $nums[left] + nums[right] + nums[i] == 0$ ，则找到一个三元组。如果 $>0$ ，则 $right -= 1$ ，如果 $<0$ ，则 $left += 1$ 。<br>
同时，题目要求不能包含重复的三元组，因此 $+= 1$ 一类的操作都要换成找到下一个不重复的元素。<br>
同时，数组已经是递增的了，有一个优化操作，当 n
## 代码
```swift
class Solution {
    func threeSum(_ nums: [Int]) -> [[Int]] {
        var ans = [[Int]]()
        let nums_sorted = nums.sorted()
        var count = nums_sorted.count
        for i in 0..<count - 2 {
            if i == 0 || nums_sorted[i] != nums_sorted[i - 1] {
                var left = i + 1
                var right = count - 1
                while left < right {
                    let sum = nums_sorted[i] + nums_sorted[left] + nums_sorted[right]
                    
                    if sum == 0 {
                        ans.append([nums_sorted[i], nums_sorted[left], nums_sorted[right]])

                        repeat {
                            left += 1
                        } while left < right && nums_sorted[left] == nums_sorted[left - 1]
                        repeat {
                            right -= 1
                        } while left < right && nums_sorted[right] == nums_sorted[right + 1]
                    }
                    else if sum > 0 {
                        repeat {
                            right -= 1
                        } while left < right && nums_sorted[right] == nums_sorted[right + 1]
                    } 
                    else {
                        repeat {
                            left += 1
                        } while left < right && nums_sorted[left] == nums_sorted[left - 1]
                    }
                }
            }
        }
        return ans
    }
}
```

<h1 id="42">接雨水</h1>

LeetCode 42. 接雨水<br>
## 题目
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。<br>
示例图如下：<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202405271426890.png)
## 解题思考
使用双指针 left 和 right 。因为容量的限制因素是 $min(height[left], height[right])$ ，因此移动指向较小值的指针。<br>
保存一个 leftMax 和 rightMax ，如果 $height[left] < height[right]$ , 则必有 $leftMax < rightMax$ ,那么 left 处可以存储的雨水量为 $leftMax - height[left]$，反之必然。因为我们移动指向较小值的指针。<br>
同时使用 height[left] 和 height[right] 去更新 leftMax 和 rightMax 。<br>
## 代码
```swift
class Solution {
    func trap(_ height: [Int]) -> Int {
        if height.isEmpty { return 0 }
        var left = 0
        var right = height.count - 1
        var leftMax = height[left]
        var rightMax = height[right]
        var capcity = 0
        
        while left < right {
            if height[left] < height[right] {
                capcity += leftMax - height[left]
                left += 1
                leftMax = max(leftMax, height[left])
            }
            else {
                capcity += rightMax - height[right]
                right -= 1
                rightMax = max(rightMax, height[right])
            }
        }
        
        return capcity
    }
}
```
## 解法拓展
本题还可以使用动态规划来实现。从双指针的解法来看，下标 i 处可以存储的雨水量取决于 i 处临近的左右两边最大值中的最小值。<br>
我们可以使用两个数组 leftMax 和 rightMax 分别存储下标 i 处的左边最大值和右边最大值。<br>
在建立 leftMax 和 rightMax 时就可以使用动态规划来。<br>
```swift
class Solution {
    func trap(_ height: [Int]) -> Int {
        if height.isEmpty { return 0 }
        
        var leftMax = Array(repeating: 0, count: height.count)
        leftMax[0] = height[0]
        for i in 1..<height.count {
            leftMax[i] = max(height[i], leftMax[i - 1])
        }
        
        var rightMax = Array(repeating: 0, count: height.count)
        rightMax[rightMax.count - 1] = height[height.count - 1]
        for i in stride(from: height.count - 2, through: 0, by: -1) {
            rightMax[i] = max(height[i], rightMax[i + 1])
        }
        
        var capcity = 0
        for i in 0..<height.count {
            capcity += min(leftMax[i], rightMax[i]) - height[i]
        }
        
        return capcity
    }
}
```