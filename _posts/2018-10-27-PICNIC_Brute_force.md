---
title: PICNIC_Brute_force
key: 20181027
tags: Algorithm_problem
---

# PICNIC_Brute_force

https://algospot.com/judge/problem/read/PICNIC

```java
Sprivate static void recursive(int index, Set<Integer> set) {
        if (set.size() == n ) {
            cnt++;
            return;
        }
        if (index == m) {
            return;
        }
        for (int i = index; i < m; i++) {
            Pair tmp = list.get(i);
            if (!set.contains(tmp.a) &&!set.contains(tmp.b)) {
                set.add(tmp.a);
                set.add(tmp.b);
                recursive(i + 1, set);
                set.remove(tmp.a);		//recover
                set.remove(tmp.b);
            }
        }
    }
```

set은 친구끼리 짝지어진 학생의 목록이다.

index는 고려된 쌍의 인덱스이다.

set에 모든 학생이 들어가면 cnt를 올리고 끝(기저)

모든 친구들의 쌍이 고려되면 끝(기저), 기저의 순서도 고려되어야 한다.

각 친구들의 쌍을 순열로 순회한다.(재귀)

