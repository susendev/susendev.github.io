---
title: 【LeetCode】01 两数之和
date: 2019-04-14 22:43:12
categories: leetcode
tags: 两数之和
---
## 题

给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回他们的数组下标。
你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

## 示例

给定 nums = [2, 7, 11, 15], target = 9
因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
<!-- more -->
## 答

``` js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
    let map = {};
    for (let index = 0; index < nums.length; index++) {
        let element = nums[index]
        let num = target - element
        if (map[num] !== undefined) {
            return [map[num], index]
        }
        map[element] = index
    }
    return []
};
 ```

## 思路

此题难度：Easy。

最简单的方法是通过两次 for 循环暴力求解，复杂度 O(N^2)。
第一次循环取出一个值，第二次从当前值的后面开始循环，依次取出与第一次的值相加，若和等于 target，则直接返回。

另一种方法是通过新建 map 以空间换时间，一次循环即可求出答案，复杂度 O(N)。
在遍历数组时，将已经出现过的数字存放在 map 中，以数值为 key，索引为 value。每次循环查看余值是否在 map 中，如果存在，则返回当前的索引和 map 中的值。当然，如果循环结束，仍未查找到，返回空数组。
