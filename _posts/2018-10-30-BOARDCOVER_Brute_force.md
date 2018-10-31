---
title: BOARDCOVER_Brute_force
key: 20181030
tags: Algorithm_problem
---

# BOARDCOVER_Brute_force

https://algospot.com/judge/problem/read/BOARDCOVER

```java
private static void recursive(int x, int y) {
	    if (y == H) {
            cnt++;
            return;
        }

        int nextX, nextY;
        if (x == W - 1) {
            nextX = 0;
            nextY = y + 1;
        } else {
            nextX = x + 1;
            nextY = y;
        }

        if (!board.get(y).get(x)) {
            recursive(nextX, nextY);
            return;
        }

        for (int coverIndex = 0; coverIndex < 4; coverIndex++) {
            if (!isBoundIn(x, y, coverIndex)) {
                continue;
            }
            if (isFitted(x, y, coverIndex)) {
                cover(x, y, coverIndex, false);
                recursive(nextX, nextY);
                cover(x, y, coverIndex, true);
            }
        }
    }
```

Board의 끝에 도착하면 끝(기저)

현재 칸이 덮어졌다면 한칸 이동		//함수 호출이 아닌 nextX, nextY의 값 변경으로 충분할 듯

덮어지지 않았다면 덮을수 있는 4가지  경우로 (재귀)

