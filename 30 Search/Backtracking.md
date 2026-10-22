---
type: template
tags: ["Search"]
---

# Backtracking

**언제** 서로 다른 n개 중 r개를 순서 있게 뽑을 때, 선택을 복구하며 모든 후보를 만든다.

```text
후보 [1,2,3], r=2
[] → [1] → [1,2] 저장
          ← [1] 복구 → [1,3] 저장
   ← [] 복구 → [2] …
```

**코드** 필드 `int[] numbers`, `int targetSize=r`. `boolean[] used=new boolean[numbers.length]`, `List<Integer> path`, `List<List<Integer>> answer`를 새로 준비하고 `dfs(0)` 호출. 두 List는 빈 ArrayList다.

```java
static void dfs(int depth) {
    if (depth == targetSize) {
        answer.add(new ArrayList<>(path)); // ①
        return;
    }
    for (int i = 0; i < numbers.length; i++) {
        if (used[i]) continue;
        used[i] = true;
        path.add(numbers[i]);
        dfs(depth + 1);
        path.remove(path.size() - 1); // ②
        used[i] = false;
    }
}
```

| 암기 포인트 | 이유 |
|---|---|
| 선택 → 재귀 → 복구 | 형제 가지는 같은 출발 상태에서 시작해야 한다. |
| ① 복사해서 저장 | 원본 path는 다음 복구에서 다시 바뀐다. |
| ② 값이 아닌 마지막 인덱스 | 이번 재귀 직전에 추가한 선택만 제거한다. |

**주의** 같은 값이 여러 인덱스에 있으면 값 기준 중복 결과가 생긴다. O(n·P(n,r)) 시간, O(n+r) 보조 공간.

[[DFS and Graphs]] · [[Brute Force]] · [[Bitmask]]
