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
        answer.add(new ArrayList<>(path)); // path가 곧 바뀌므로 복사본 저장
        return;
    }
    for (int i = 0; i < numbers.length; i++) {
        if (used[i]) continue;
        used[i] = true;
        path.add(numbers[i]);
        dfs(depth + 1);
        path.remove(path.size() - 1); // 선택 전 상태로 복구해야 다음 가지가 독립
        used[i] = false;
    }
}
```

서로 다른 `numbers`, 뽑을 개수 `targetSize`; `used=new boolean[n]`, 빈 `path`·`answer`에서 `dfs(0)`. O(n·P(n,r)).

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]

**비교** [[30 Search/Brute Force|Brute Force]] · [[80 Math and States/Bitmask|Bitmask]]
