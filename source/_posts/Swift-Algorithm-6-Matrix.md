---
title: Swift 算法-6-矩阵
date: 2024-06-07 20:48:44
tags: 算法
---
# 目录
[前言](#0)
[矩阵置零](#73)
[螺旋矩阵](#54)
[旋转图像](#48)
[搜索二维矩阵 II](#240)

<h1 id="0">前言</h1>

本文题目来源 LeetCode ，主要用于记录笔者的一些解题思路和思考。<br>
本文的主题是矩阵。<br>

<h1 id="73">矩阵置零</h1>

LeetCode 73. 矩阵置零<br>
## 题目
给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。<br>
示例：<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202406110928095.png)<br>
## 解题思考
从示例中的输入输出很容易看出一个使用额外 `m+n` 容量空间的解法。<br>
我们使用两个布尔数组分别标识行和列。然后遍历整个数组，当发现零时，将当前行列的标识置为 `true`。<br>
遍历结束后再遍历标识数组，然后将对应行列置零。<br>
## 代码
```swift
class Solution {
    func setZeroes(_ matrix: inout [[Int]]) {
        let m = matrix.count    //rows
        let n = matrix[0].count //colums

        var rowsArray = Array(repeating: false, count: m);
        var columnsArray = Array(repeating: false, count: n);

        for i in 0..<m {
            for j in 0..<n {
                if matrix[i][j] == 0 {
                    rowsArray[i] = true
                    columnsArray[j] = true
                }
            }
        }

        for i in 0..<m {
            for j in 0..<n {
                if rowsArray[i] || columnsArray[j] {
                    matrix[i][j] = 0
                }
            }
        }
    }
}
```
## 拓展解法
上述解法对于额外空间的容量我们还可以进一步压缩。<br>
我们可以使用第一行来作为列是否置零的标识数组，同时，我们需要使用一个额外的变量来单独记录第一行是否需要置零。这样子额外空间的使用就变成了 `m+1` 了。同样，我们可以使用第一列来作为行是否置零的标识数组，再单独用一个变量记录第一列是否需要置零。<br>
同时存在一些细节处理，请读者自行思考。<br>
经过上述变化，我们成功将空间复杂度将为 O(1) 。<br>
```swift
class Solution {
    func setZeroes(_ matrix: inout [[Int]]) {
        let m = matrix.count    //rows
        let n = matrix[0].count //columns
        var firstRow = false
        var firstColumn = false

        if matrix[0][0] == true  {
            firstRow = true
            firstColumn = true
        }
        else {
            for i in 1..<n {
                if matrix[0][i] == 0 {
                    firstRow = true
                    break
                }
            }
            for i in 1..<m {
                if matrix[i][0] == 0 {
                    firstColumn = true
                    break
                }
            }
        }

        for i in 1..<m {
            for j in 1..<n {
                if matrix[i][j] == 0 {
                    matrix[i][0] = 0
                    matrix[0][j] = 0
                }
            }
        }

        for i in 1..<m {
            for j in 1..<n {
                if matrix[i][0] == 0 || matrix[0][j] == 0 {
                    matrix[i][j] = 0
                }
            }
        }

        if firstRow {
            for i in 0..<n {
                matrix[0][i] = 0
            }
        }

        if firstColumn {
            for i in 0..<m {
                matrix[i][0] = 0
            }
        }
    }
}
```

<h1 id="54">螺旋矩阵</h1>

LeetCode 54. 螺旋矩阵<br>
## 题目
给你一个 m 行 n 列的矩阵 matrix ，请按照 顺时针螺旋顺序 ，返回矩阵中的所有元素。<br>
示例：<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202406111000477.png)
## 解题思考
最直接的思路就是跟着螺旋路线输出。根据边界情况判断螺旋路线方向的切换。<br>
## 代码
```swift
class Solution {
    func spiralOrder(_ matrix: [[Int]]) -> [Int] {
        var ans = [Int]()
        var top = 0
        var bottom = matrix.count
        var left = 0
        var right = matrix[0].count

        enum Direction {
            case RIGHT, DOWN, LEFT, UP
        }

        var direction = Direction.RIGHT

        while top < bottom && left < right {
            switch direction {
                case .RIGHT:
                    for i in left..<right {
                        ans.append(matrix[top][i])
                    }
                    direction = .DOWN
                    top += 1
                case .DOWN:
                    for i in top..<bottom {
                        ans.append(matrix[i][right - 1])
                    }
                    direction = .LEFT
                    right -= 1
                case .LEFT:
                    for i in stride(from: right - 1, through: left, by: -1) {
                        ans.append(matrix[bottom - 1][i])
                    }
                    direction = .UP
                    bottom -= 1    
                case .UP:
                    for i in stride(from: bottom - 1, through: top, by: -1) {
                        ans.append(matrix[i][left])
                    }
                    direction = .RIGHT
                    left += 1
            }
        }

        return ans
    }
}
```

<h1 id="48">旋转图像</h1>

LeetCode 48. 旋转图像<br>
## 题目
给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。<br>
你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。<br>
## 解题思考
因为这个矩阵的行列数一致，我们可以直接使用一些翻转来实现旋转。<br>
图像顺时针旋转 90 度，可以变成矩阵左右翻转 + 副对角线翻转。<br>
现在的问题是，副对角线翻转的公式是什么。<br>
结果数学推导，得到 `[i, j]` 位置对应的翻转位置为 `[n - j - 1, n - i - 1]` 。<br>
## 代码
```swift
class Solution {
    func rotate(_ matrix: inout [[Int]]) {
        let n = matrix.count
        //左右翻转
        for i in 0..<n {
            matrix[i].reverse()
        }
        //副对角线翻转
        for i in 0..<n {
            for j in 0..<(n - i) {
                let temp = matrix[i][j]
                matrix[i][j] = matrix[n - j - 1][n - i - 1]
                matrix[n - j - 1][n - i - 1] = temp
            }
        }
    }
}
```

<h1 id="240">搜索二维矩阵 II</h1>

LeetCode 240. 搜索二维矩阵 II<br>
## 题目
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target 。该矩阵具有以下特性：<br>

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。
示例：<br>
![](https://raw.githubusercontent.com/illusorycat/MyPictureBase/main/image/202406111032792.png)
## 解题思考
根据矩阵的特效，我们可以过滤掉很多不必要的检索。<br>
如果当前行的最后一个元素比目标值小，那当前行剩余的元素肯定小于目标值。如果当前行的最后一个元素比目标值大，那其所在列中，行数大于当前行的元素值肯定大于目标值。<br>
因此我们可以从左上角的元素开始遍历，根据上述规则去缩减搜索的行和列。<br>
## 代码
```swift
class Solution {
    func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {
        let m = matrix.count
        let n = matrix[0].count

        var row = 0
        var column = n - 1

        while row < m && column >= 0 {
            if matrix[row][column] == target {
                return true
            }
            else if matrix[row][column] < target {
                row += 1
            }
            else {
                column -= 1
            }
        }

        return false
    }
}
```