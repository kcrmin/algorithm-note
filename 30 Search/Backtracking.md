---
type: template
tags: ["Search"]
category: exhaustive-search
categories: [exhaustive-search]
---

# Backtracking

```java
static int[] numbers;
static int targetSize;
static boolean[] used;
static List<Integer> path;
static List<List<Integer>> answer;

static void dfs(int depth) {
    if (depth == targetSize) {
        // answer: 완성된 경로는 복사해서 저장
        answer.add(new ArrayList<>(path));
        return;
    }
    for (int i = 0; i < numbers.length; i++) {
        if (used[i]) continue;
        // choose: 아직 안 쓴 후보 선택
        used[i] = true;
        path.add(numbers[i]);
        // explore: 다음 자리 채우기
        dfs(depth + 1);
        // undo: 선택을 복구한 뒤 다음 후보 탐색
        path.remove(path.size() - 1);
        used[i] = false;
    }
}
```

서로 다른 `numbers`, 뽑을 개수 `targetSize`; `used=new boolean[n]`, 빈 `path`·`answer`에서 `dfs(0)`. O(n·P(n,r)).

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]

**비교** [[30 Search/Brute Force|Brute Force]] · [[80 Math and States/Bitmask|Bitmask]]
