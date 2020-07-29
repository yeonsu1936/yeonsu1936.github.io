---
layout: post
title: hackerrank - 04. Diagonal Difference
category: 알고리즘 문제풀이
permalink: /algorithm/:year/:month/:day/:title/
tags: [알고리즘, 프로그래밍, 파이썬]
comments: true
---

> [문제출처](https://www.hackerrank.com/challenges/a-very-big-sum/problem)

## 문제

```
Calculate and print the sum of the elements in an array, keeping in mind that some of those integers may be quite large.

Function Description:
Complete the aVeryBigSum function in the editor below. It must return the sum of all array elements.

aVeryBigSum has the following parameter(s):
- int ar[n]: an array of integers.

Return:
- int: the sum of all array elements

Input Format:
The first line of the input consists of an integer n.
The next line contains n space-separated integers contained in the array.

Output Format:
Return the integer sum of the elements in the array.

Constraints:
1 <= n <= 10
0 <= ar[i] <= 10^10

Sample Input:
5
1000000001 1000000002 1000000003 1000000004 1000000005

Output:

5000000015
Note:
The range of the 32-bit integer is (-2^31) to (2^31-1) or [-2147483648, 2147483647].
When we add several integer values, the resulting sum might exceed the above range. You might need to use long int C/C++/Java to store such sums.
```

---

## 풀이코드
- 정수로 이루어진 정사각형의 2차원 배열이 주어진다.
- 해당 배열에서 좌상->우하로 향하는 요소들을 모두 더한 값을 A, 우상->좌하로 향하는 요소들을 모두 더한 값을 B라고 했을 때 A-B=절댓값 X를 구한다.
- X는 절대값(abs)이어야 한다.

```python
#!/bin/python3

import math
import os
import random
import re
import sys

#
# Complete the 'diagonalDifference' function below.
#
# The function is expected to return an INTEGER.
# The function accepts 2D_INTEGER_ARRAY arr as parameter.
#

def diagonalDifference(arr):
    left_sum = 0
    right_sum = 0

    for i in range(len(arr)):
        left_sum += arr[i][i]
        right_sum += arr[i][n-1-i]
    return abs(right_sum-left_sum)

if __name__ == '__main__':
    fptr = open(os.environ['OUTPUT_PATH'], 'w')

    n = int(input().strip())

    arr = []

    for _ in range(n):
        arr.append(list(map(int, input().rstrip().split())))

    result = diagonalDifference(arr)

    fptr.write(str(result) + '\n')

    fptr.close()
```
