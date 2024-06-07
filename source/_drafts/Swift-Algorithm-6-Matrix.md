---
title: Swift-Algorithm-6-Matrix
date: 2024-06-07 20:48:44
tags:
---


```swift
// 73. 矩阵置零
// 试着用这个算法做一个泡泡堂游戏，这个算法其实就是一个炸弹爆炸覆盖范围的算法
//两个解法都是要遍历两次数组
class Solution {
    //看示例就可以很容易想出解法
    //输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
    //输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]
    //这个解法的空间复杂度是 O(n+m)
    func setZeroes(_ matrix: inout [[Int]]) {
        var rows = matrix.count
        var columns = matrix[0].count
        var rowArray = Array(repeating: false, count: rows)
        var colunmArray = Array(repeating: false, count: columns)
        
        for i in 0..<rows {
            for j in 0..<columns {
                if matrix[i][j] == 0 {
                    rowArray[i] = true
                    colunmArray[j] = true
                }
            }
        }
        
        for i in 0..<rows {
            for j in 0..<columns {
                if rowArray[i] || colunmArray[j] {
                    matrix[i][j] = 0
                }
            }
        }
    }
    //仅使用常量级别的空间的解法
    //使用第一行和第一列分别来记录后续行列中是否出现 0
    //同时，因为第一行和第一列会增加 0 作为标记，所以需要使用另外两个变量标记第一行和第一列是否存在 0
    func setZeroes_(_ matrix: inout [[Int]]) {
        let rows = matrix.count
        let columns = matrix[0].count
        
        var flag_row0 = false
        var flag_column0 = false
        for i in 0..<columns {
            if matrix[0][i] == 0 {
                flag_row0 = true
                break
            }
        }
        for i in 0..<rows {
            if matrix[i][0] == 0 {
                flag_column0 = true
                break
            }
        }
        
        for i in 1..<rows {
            for j in 1..<columns {
                if matrix[i][j] == 0 {
                    matrix[i][0] = 0
                    matrix[0][j] = 0
                }
            }
        }
        
        for i in 1..<rows {
            for j in 1..<columns {
                if matrix[i][0] == 0 || matrix[0][j] == 0 {
                    matrix[i][j] = 0
                }
            }
        }
        
        if flag_row0 {
            for i in 0..<columns {
                matrix[0][i] = 0
            }
        }
        if flag_column0 {
            for i in 0..<rows {
                matrix[i][0] = 0
            }
        }
    }
}


// 54. 螺旋矩阵
class Solution2 {
    //这是直接走了一次螺旋路径
    func spiralOrder(_ matrix: [[Int]]) -> [Int] {
        var left = 0
        var right = matrix[0].count
        var top = 0
        var bottom = matrix.count
        var ans = [Int]()
        
        enum Direction: Int {
            case Right = 1
            case Down = 2
            case Left = 3
            case Up = 4
        }
        
        var direction = Direction.Right
        while left < right && top < bottom {
            switch direction {
            case .Right:
                for i in left..<right {
                    ans.append(matrix[top][i])
                }
                top += 1
                direction = .Down
            case .Down:
                for i in top..<bottom {
                    ans.append(matrix[i][right - 1])
                }
                right -= 1
                direction = .Left
            case .Left:
                for i in stride(from: right - 1, through: left, by: -1) {
                    ans.append(matrix[bottom - 1][i])
                }
                bottom -= 1
                direction = .Up
            case .Up:
                for i in stride(from: bottom - 1, through: top, by: -1) {
                    ans.append(matrix[i][left])
                }
                left += 1
                direction = .Right
            }
        }
        
        return ans
    }
    //可以将矩阵分隔，
    //然后从外到内遍历每一层，
    //相比上面的做法就是不需要定义一个枚举，并且减少了判断次数
}

// 48. 旋转图像
class Solution3 {
    //从示例看出解法
    //输入：matrix = [[5,1,9,11],[2,4,8,10],[13,3,6,7],[15,14,12,16]]
    //输出：[[15,13,2,5],[14,3,4,1],[12,6,8,9],[16,7,10,11]]
    func rotate(_ matrix: inout [[Int]]) {
        let n = matrix.count
        
    }
    
    //
    func rotate_change(_ matrix: inout [[Int]]) {
        let n  = matrix.count
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

var matrix = [[1,2,3],[4,5,6],[7,8,9]]
print(Solution3().rotate_change(&matrix))

// 240. 搜索二维矩阵 II
class Solution4 {
    func searchMatrix(_ matrix: [[Int]], _ target: Int) -> Bool {
        var column = matrix[0].count
        while column > 0 && matrix[0][column - 1] > target {
            column -= 1
        }
        var row = 0
        let rows = matrix.count
        while row < rows && matrix[row][0] <= target {
            if matrix[row].last! >= target {
                for j in 0..<column {
                    if matrix[row][j] == target {
                        return true
                    }
                }
            }
            row += 1
        }
        return false
    }
    
    func searchMatrix_simple(_ matrix: [[Int]], _ target: Int) -> Bool {
        let rows = matrix.count
        let columns = matrix[0].count
        var row = 0
        var column = columns - 1
        
        while column >= 0 && row < rows {
            let num = matrix[row][column]
            if num == target {
                return true
            }
            else if num > target {
                column -= 1
            }
            else {
                row += 1
            }
        }
        return false
    }
}

```