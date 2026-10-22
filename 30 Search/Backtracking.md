---
type: template
tags: ["Search"]
---

# Backtracking

```java
// int[] numbers는 서로 다른 후보, int targetSize는 뽑을 개수.
// used는 new boolean[numbers.length].
// List<Integer> path, List<List<Integer>> answer는 각각 빈 ArrayList.
// 빈 path에서 dfs(0) 호출. 한 가지 탐색이 끝나면 선택 직전 상태로 복구한다.
static void dfs(int depth) {
    if (depth == targetSize) { // 완성되면 더 고르지 않고 이 가지를 끝냄
        // path는 곧 바뀌므로 현재 내용의 복사본 저장
        answer.add(new ArrayList<>(path));
        return;
    }
    for (int i = 0; i < numbers.length; i++) {
        if (used[i]) continue; // 같은 인덱스를 한 경로에서 두 번 고르지 않음
        used[i] = true;
        path.add(numbers[i]);
        dfs(depth + 1);
        path.remove(path.size() - 1); // 이번에 추가한 마지막 값만 제거
        used[i] = false; // 다음 형제 가지에서도 이 후보를 고를 수 있게 복구
    }
}
```

**예시**

```text
후보 [1,2,3], r=2
[] → [1] → [1,2] 저장
          ← [1] 복구 → [1,3] 저장
   ← [] 복구 → [2] …
```

**주의** 같은 값이 여러 인덱스에 있으면 값 기준 중복 결과가 생긴다. O(n·P(n,r)) 시간, O(n+r) 보조 공간.

[[DFS and Graphs]] · [[Brute Force]] · [[Bitmask]]
