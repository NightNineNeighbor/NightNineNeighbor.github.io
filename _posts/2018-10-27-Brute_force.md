---
title: Brute Force, 조합 탐색
key: 20181027
tags: Algorithm
---

# Brute Force, 조합 탐색

> 가장 많이 하는 실수는 쉬운 문제를 어렵게 푸는 것이다.



재귀호출

> tip : 입력이 잘못되거나 범위에서 벗어난 경우도 기저 사례로 택해서 맨 처리에 처리하면, 함수를 호출하는 시점에 이런 오류를 검사할 필요가 없다.

```c++
const int dx[8] = {-1,-1,-1,1,1,1,0,0};
const int dy[8] = {-1,0,1,-1,0,1,-1,1};

bool hasWord(int x , intx, const string& word){
    if(!inRange(y,x)) return false;
    if(board[x][y] ! = word[0]) return false;
    if(word.size() == 1) return true;
    for(int direction = 0; dirction < 8; ++direction){
        int nextY = y + dy[direction], nextX = x + dx[direction];
        //위에서 확인하므로, 다음칸이 범위 안에 있는지, 첫 글자는 일치하는지 확인할 필요가 없다.
        if(hasWord(nextY, nextX, word.substr(1))){
            return true;
        }
    }
    return false;
}
```



완전 탐색 레피시

1. 최대 크기의 입력을 가정했을 때 답의 개수를 계산하고 이들을 모두 제한 시간에 생성할 수 있을지를 가늠한다.
2. 가능한 모든 답의 후보를 만드는 과정을 여러개의 선택으로 나눈다. 각 선택은 답의 후보를 만드는 과정의 한 조각이 된다.
3. 그중 하나의 조각을 선택해 답의 일부를 만들고, 나머지 답을 재귀 호출을 통해 완성한다.
4. 조각이 하나밖에 남지 않은 경우, 혹은 하나도 남지 않은 경우에는 답을 생성했으므로, 이것을 기저 사례로 선택해 처리한다.



## 조합 탐색

적절한 분할 방법이 없다면 분할 정복을 쓸 수 없고, 중복되는 문제가 없거나 메모리를 너무많이 사용한다면 동적 계획법을 쓸 수 없다.

그렇다면 완전 탐색으로 돌아와야 한다.

조합 탐색은 완전 탐색에서 최적해가 될 가능성이 없는 답들을 탐새갛는것을 방지하는 것이다.





