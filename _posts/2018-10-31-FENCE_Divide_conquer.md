---
title: FENCE_Divide_conquer
key: 20181031
tags: Algorithm_problem
---

# FENCE_Divide_conquer

https://algospot.com/judge/problem/read/QUADTREE

```java
private static String divideConquer(Iterator<String> iterator) {
        String head = iterator.next();
        if (head.equals("b") || head.equals("w")) {
            return head;
        }

        String upperLeft = divideConquer(iterator);
        String upperRight = divideConquer(iterator);
        String lowerLeft = divideConquer(iterator);
        String lowerRight = divideConquer(iterator);
        return "x" + lowerLeft + lowerRight + upperLeft + upperRight;
    }
```

b나 w 면 뒤집을 필요가 없으므로 그대로 반환(base case)

x 이면 각각의 재귀 4번 (divide)

상하를 반전시켜 합친다. (merge)

