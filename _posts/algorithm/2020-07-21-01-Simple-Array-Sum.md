---
layout: post
title: hackerrank - 01. Simple Array Sum
category: 알고리즘 문제풀이
permalink: /algorithm/:year/:month/:day/:title/
tags: [알고리즘, 프로그래밍, 파이썬]
comments: true
---

> [문제출처](https://www.hackerrank.com/challenges/simple-array-sum/problem)

## 문제

```
Given an array of integers, find the sum of its elements.
For example, if the array ar = [1, 2, 3], 1 + 2 + 3 = 6, so return 6.

Function Description:
Complete the simpleArraySum function in the editor below. It must return the sum of the array elements as an integer.
simpleArraySum has the following parameter(s):
  ar: an array of integers

Input Format:
The first line contains an integer, , denoting the size of the array.
The second line contains  space-separated integers representing the array's elements.

Constraints:
0 < n, ar[i] <= 1000

Output Format:
Print the sum of the array's elements as a single integer.

Sample Input:
6
1 2 3 4 10 11

Sample Output:
31

Explanation:
We print the sum of the array's elements: 1 + 2 + 3 + 4 + 10 + 11 = 31.
```

---

## 풀이코드
- 배열 안에 담긴 정수형 숫자들을 모두 더해 반환한다.
- 파이썬 내장함수 sum()을 사용했다.

```python
#!/bin/python3

import os
import sys

#
# Complete the simpleArraySum function below.
#
def simpleArraySum(ar):
    return sum(ar)

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    ar_count = int(input())

    ar = list(map(int, input().rstrip().split()))

    result = simpleArraySum(ar)

    fptr.write(str(result) + '\n')

    fptr.close()
```
