---
type: template
tags: ["Search"]
category: exhaustive-search
categories: [exhaustive-search]
---

# Backtracking

```text
numbers = [1, 2, 3], 2개를 순서 있게 선택
[]
├─ [1] ─┬─ [1,2] ✓ → [1]로 복구
│       └─ [1,3] ✓ → [1]로 복구
├─ [2] ─┬─ [2,1] ✓
│       └─ [2,3] ✓
└─ [3] ─┬─ [3,1] ✓
        └─ [3,2] ✓
```

| Step | Pseudocode |
|---|---|
| Finish | // 필요한 개수를 채웠으면 현재 선택을 복사해서 저장한다. |
| Choose | // 이번 경로에서 쓰지 않은 원소 하나를 선택한다. |
| Explore | // 선택한 상태로 다음 자리를 채운다. |
| Undo | // 돌아오면 선택과 사용 표시를 되돌려 다음 후보를 준비한다. |

## Java

```java
static int[] numbers;
static int targetSize;
static boolean[] used;
static List<Integer> path;
static List<List<Integer>> answer;

static void dfs(int depth) {
    if (depth == targetSize) {
        // 이후 path가 바뀌므로 복사한다.
        answer.add(new ArrayList<>(path));
        return;
    }

    for (int i = 0; i < numbers.length; i++) {
        if (used[i]) {
            continue;
        }

        used[i] = true;
        path.add(numbers[i]);
        dfs(depth + 1);

        // 이번 선택을 모두 되돌린다.
        path.remove(path.size() - 1);
        used[i] = false;
    }
}
```

## Watch points

| 선택 | 이유 / 실수 |
|---|---|
| 선택 → 재귀 → 복구 | 형제 탐색은 같은 출발 상태여야 한다. path와 used를 모두 되돌린다. |
| `new ArrayList<>(path)` | 같은 path 객체를 저장하면 이후 복구가 이미 저장한 답까지 바꾼다. |
| Permutation / Combination | 현재는 순서가 다른 선택도 센다. 조합은 시작 인덱스를 넘겨 앞에서 쓴 후보로 돌아가지 않는다. |
| 초기화 | used는 모두 false, path·answer는 빈 리스트. 매 문제마다 새로 준비한다. |

`numbers`는 서로 다른 값, `targetSize=r`은 뽑을 개수. `dfs(0)` 호출. **O(n·P(n,r))**.

**분류** [[00 Maps/Exhaustive Search|Exhaustive Search]]

**비교** [[30 Search/Brute Force|Brute Force]] · [[80 Math and States/Bitmask|Bitmask]]
