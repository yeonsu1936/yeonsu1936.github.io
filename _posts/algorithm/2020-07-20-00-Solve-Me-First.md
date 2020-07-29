---
layout: post
title: hackerrank - 00. Solve Me First
category: 알고리즘 문제풀이
permalink: /algorithm/:year/:month/:day/:title/
tags: [알고리즘, 프로그래밍, 파이썬]
comments: true
---

> [문제출처](https://www.hackerrank.com/challenges/solve-me-first/problem)

## 문제

```
Complete the function solveMeFirst to compute the sum of two integers.

Function prototype:
int solveMeFirst(int a, int b);

where,
- a is the first integer input.
- b is the second integer input

Return values
-sum of the above two integers

Sample Input
a = 2
b = 3

Sample Output
5

Explanation
The sum of the two integers a and b is computed as: 2 + 3 = 5.
```

---

## 풀이코드
- 입력받은 num1과 num2를 더해 반환한다.

```python
def solveMeFirst(a,b):
    return a+b

num1 = int(input())
num2 = int(input())
res = solveMeFirst(num1,num2)
print(res)
```
