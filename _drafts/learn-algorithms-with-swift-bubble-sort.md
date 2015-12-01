---
layout: post
title: "用Swift学习算法——冒泡排序"
category:  Algorithm
---

## 算法原理
从小到大排序：
假设数组的长度为N。
1. 比较相邻的元素，如果前一个比后一个大，就交换这两个元素；
2. 这样对数组的0 ～ N-1个数据进行一次遍历后，最大的元素就到了最后，即第N-1的位置；
3. N = N-1，重复前两步。如果N == 0则排序完成
从大到下排序反过来遍历就可以了。

## 算法实现
放到Playground中即可运行。

```Swift
// Playground - noun: a place where people can play

import UIKit

// create a random integer array
var random = [Int]()
for i in 0..<10 {
    random.append(Int(arc4random()) % 100)
}
random

// define an exchange function
func exchange<T>(inout array: [T], i: Int, j: Int) {
    println("Exchange item \(i) and item \(j)")
    let temp = array[i]
    array[i] = array[j];
    array[j] = temp
}
exchange(&random, 1, 2)
random

// bubble sort
func bubbleSort<T: Comparable>(inout array: [T]) {
    for i in 0..<array.count {
        for j in 0..<(array.count-i-1) {
            if array[j] > array[j+1] {
                exchange(&array, j, j+1)
            }
        }
    }
}
bubbleSort(&random)
random
```
