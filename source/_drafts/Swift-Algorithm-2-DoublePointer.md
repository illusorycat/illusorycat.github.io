---
title: Swift 算法-1-双指针
date: 2024-05-25 18:19:14
tags: Swift, 算法
---


```Swift
//283. 移动零

class Solution {
    func moveZeroes_violence(_ nums: inout [Int]) {
        for i in stride(from: nums.count - 1, through: 0, by: -1) {
            if nums[i] == 0 {
                var curIndex = i
                while (curIndex + 1) < nums.count && nums[curIndex + 1] != 0 {
                    nums.swapAt(curIndex, curIndex + 1)
                    curIndex += 1
                }
            }
        }
    }
    //类似于快排的思路，先找到第一个 0 ，将 0 作为一个区间，从区间后开始遍历，遇到非0就与0区间的第一个交互
    //等价于移动0区间
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

var nums = [0,1,0,3,12]
Solution().moveZeroes(&nums)
print(nums)


// 11. 盛最多水的容器
class Solution2 {
    //超出时间限制
    func maxArea_violence(_ height: [Int]) -> Int {
        var maxArea = 0
        for i in 0..<height.count {
            var iHeight = height[i]
            var jHeight = 0
            for j in stride(from: height.count - 1, to: i, by: -1) {
                guard height[j] > jHeight else { continue }
                jHeight = height[j]
                var curArea = min(iHeight, jHeight) * (j - i)
                if curArea > maxArea {
                    maxArea = curArea
                }
            }
        }
        return maxArea
    }
    func maxArea(_ height: [Int]) -> Int {
        var i = 0
        var j = height.count - 1
        var iHeight = 0
        var jHeight = 0
        var maxArea = 0
        while i < j {
            iHeight = height[i]
            jHeight = height[j]
            if iHeight <= jHeight {
                let curArea = iHeight * (j - i)
                maxArea = max(maxArea, curArea)
                while i < j {
                    i += 1
                    if height[i] > iHeight {
                        break
                    }
                }
            }
            else {
                let curArea = jHeight * (j - i)
                maxArea = max(maxArea, curArea)
                while j > i {
                    j -= 1
                    if height[j] > jHeight {
                        break
                    }
                }
            }
        }
        return maxArea
    }
    func maxArea_simple(_ height: [Int]) -> Int {
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

print(Solution2().maxArea_simple([1,8,6,2,5,4,8,3,7]))

// 15. 三数之和
class Solution3 {
    // 超出时间限制
    func threeSum_violence(_ nums: [Int]) -> [[Int]] {
        var ans = [[Int]]()
        let nums_sorted = nums.sorted()
        var i = 0
        while i < nums_sorted.count {
            var j = i + 1
            while j < nums_sorted.count {
                var k = j + 1
                while k < nums_sorted.count {
                    if nums_sorted[i] + nums_sorted[j] + nums_sorted[k] == 0 {
                        ans.append([nums_sorted[i], nums_sorted[j], nums_sorted[k]])
                    }
                    k += 1
                    while k < nums_sorted.count && nums_sorted[k] == nums_sorted[k - 1] {
                        k += 1
                    }
                }
                j += 1
                while j < nums_sorted.count && nums_sorted[j] == nums_sorted[j - 1] {
                    j += 1
                }
            }
            i += 1
            while i < nums_sorted.count && nums_sorted[i] == nums_sorted[i - 1] {
                i += 1
            }
        }
        return ans
    }
    
    //固定一个数，简化为两数之和
    //排序+双指针确定两数之和      //两数之和的哈希那道题也可以使用排序+双指针
    func threeSum(_ nums: [Int]) -> [[Int]] {
        var ans = [[Int]]()
        let nums_sorted = nums.sorted()
        var count = nums_sorted.count
        for i in 0..<count - 2 {
            if nums_sorted[i] + nums_sorted[i + 1] > 0 {
                break
            }
            if i == 0 || nums_sorted[i] != nums_sorted[i - 1] {
                var j = i + 1
                var k = count - 1
                while j < k {
                    let sum = nums_sorted[i] + nums_sorted[j] + nums_sorted[k]
                    if sum == 0 {
                        ans.append([nums_sorted[i], nums_sorted[j], nums_sorted[k]])
                        repeat {
                            j += 1
                        } while j < k && nums_sorted[j] == nums_sorted[j - 1]
                        repeat {
                            k -= 1
                        }while j < k && nums_sorted[k] == nums_sorted[k + 1]
                    }
                    else if sum > 0 {
                        k -= 1
                    }
                    else {
                        j += 1
                    }
                }
            }
        }
        return ans
    }
}

Solution3().threeSum([1,-1,-1,0])

```