---
title: 백준_1074_Z_Divide_conquer
key: 20181030
tags: Algorithm_problem
---

# 백준_1074_Z_Divide_conquer

https://www.acmicpc.net/problem/1074

```java
  private static int divideConquer(int n, int r, int c, int prev) {
        if (n == 1) {
            if (r == 0 && c == 0) {
                return 0;
            } else if (r == 0 && c == 1) {
                return 1;
            } else if (r == 1 && c == 0) {
                return 2;
            } else {
                return 3;
            }
        }
      
        int t = (int) Math.pow(2, n - 1);
        if (r < t && c < t) {
            return divideConquer(n - 1, r, c, prev);
        } else if (r < t && c >= t) {
            return divideConquer(n - 1, r, c - t, prev) + t * t;
        } else if (r >= t && c < t) {
            return divideConquer(n - 1, r - t, c, prev) + t * t * 2;
        } else {
            return divideConquer(n - 1, r - t, c - t, prev) + t * t * 3;
        }
    }
```

n == 1 일 때 0 1 2 3 중 적절한 값을 반환(base case)	

평면을 4개로 나눈다(divide)

나눈 평면중 Z의 경로에 있지만 점이 포함되지 않는 것은 간단한 수식으로 합친다. 점이 포함된 곳은 재귀(merge).