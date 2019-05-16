---
title: 【LeetCode】07 整数反转
categories: leetcode
tags: 整数反转
---
## 题目
给出一个 32 位的有符号整数，你需要将这个整数中每位上的数字进行反转。

**示例 1:**
```
输入: 123
输出: 321
```
**示例 2:**
```
输入: -123
输出: -321
```
**示例 3:**
```
输入: 120
输出: 21
```
**注意:**

假设我们的环境只能存储得下 32 位的有符号整数，则其数值范围为 [−231,  231 − 1]。请根据这个假设，如果反转后整数溢出那么就返回 0。

## 思路

此题难度：Easy。

翻转前的整数位 x，反转后的整数为 reverseX，那么 x 的最后一位一定是 reverseX 的第一位，通过循环取余，得到 x 的末位数字。假设 x 有 n 位，则循环n次，每次循环都使 x 左移一位，最后刚好 x 的末位变为 reverseX 的首位。依次类推，即可得到反转后的整数 reverseX。注意处理反转后整数溢出的情况。

## 解答
```js
/**
 * @param {number} x
 * @return {number}
 */
var reverse = function (x) {
    let reverseX = 0
    while (true) {
        reverseX = reverseX * 10 + x % 10
        x = parseInt(x / 10)
        if ((reverseX > 2147483647) || reverseX < -2147483648) {
            return 0
        } else if (x === 0) {
            return reverseX
        }
    }
};

```
## 我的提交执行用时
执行用时：124 ms
内存消耗：36 MB	
已经战胜 87.76 % 的 javascript 提交记录

