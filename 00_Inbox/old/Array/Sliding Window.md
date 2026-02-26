---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java Prefix Sum (1D / 2D)
> **한 줄 요약:** 구간 합을 O(1)로 만들기 위해 누적합 배열을 미리 만들어 두는 전처리 기법

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * Prefix Sum = **미리 누적해 두고, 질의는 빠르게**
    * 1D는 연속 구간 합, 2D는 직사각형 합에 특화
    * 인덱스 실수를 줄이기 위해 **1-based 누적합 배열**을 권장
    * 합의 범위가 커질 수 있으므로 보통 `long` 사용
* **Time Complexity:**  
  * Build: 1D O(N), 2D O(R·C)  
  * Query: 1D O(1), 2D O(1)

## 2. API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init (1D) | `long[] ps = new long[n+1];` | ps 초기화 | 1-based 누적합 |
| Build (1D) | `ps[i] = ps[i-1] + arr[i];` | 상태 변경 | 누적합 채움 |
| Query (1D) | `ps[r] - ps[l-1]` | long | 구간합 |
| Init (2D) | `long[][] ps = new long[r+1][c+1];` | ps 초기화 | 1-based 2D |
| Build (2D) | `ps[y][x]=...` | 상태 변경 | 포함-배제 |
| Query (2D) | `rectSum(ps, y1, x1, y2, x2)` | long | 직사각형 합 |

## 3. Code Templates (Patterns)

### Pattern A: 1D Prefix Sum (Build + Query + m 최대합)
설명: 1D 누적합을 만들고, 구간 [l..r] 합을 O(1)로 구한다.  
```java
// ps 초기화 (1-based)
long[] ps = new long[n + 1];

// Build
for (int i = 1; i <= n; i++) {
    ps[i] = ps[i - 1] + arr[i - 1];
}

// Query: 구간합 [l..r]
int l = 2, r = 4;
long sum = ps[r] - ps[l - 1];

// 길이 m 구간 최대합
int m = 3;
long best = Long.MIN_VALUE;

for (int r = m; r <= n; r++) {
    int l = r - m + 1;
    long cur = ps[r] - ps[l - 1];  // 여기서 길이 m 합 계산
    best = Math.max(best, cur);
}
```

### Pattern B: 2D Prefix Sum (Build + rectSum + m×m)
설명: 2D 누적합을 만들고, `rectSum`으로 직사각형 합을 O(1)로 구한다.
```java
// (y1, x1) ~ (y2, x2) 범위의 합 (모두 1-based, inclusive)
static long rectSum(long[][] ps, int y1, int x1, int y2, int x2) {
    return ps[y2][x2]
         - ps[y1 - 1][x2]
         - ps[y2][x1 - 1]
         + ps[y1 - 1][x1 - 1];
}

// ps 초기화 (1-based)
long[][] ps = new long[R + 1][C + 1];

// Build: 포함-배제
for (int y = 1; y <= R; y++) {
    for (int x = 1; x <= C; x++) {
        ps[y][x] = ps[y - 1][x]
                 + ps[y][x - 1]
                 - ps[y - 1][x - 1]
                 + grid[y - 1][x - 1];
    }
}

// m×m 정사각형 최대 합 예시 (y, x가 오른쪽 아래)
int m = 3;
long best = Long.MIN_VALUE;

for (int y = m; y <= R; y++) {
    for (int x = m; x <= C; x++) {
        int sy = y - m + 1;
        int sx = x - m + 1;
        long cur = rectSum(ps, sy, sx, y, x);  // 여기서 m×m 합 계산
        best = Math.max(best, cur);
    }
}
```

## 4. Logic Trace (Step-by-Step)
| Step | Operation | Current State | Target/Result | Explanation |
| :--- | :--- | :--- | :--- | :--- |
| 1 | Initialize | 원본 배열/격자 | ps 준비 | 1-based 크기 |
| 2 | Build | ps 비어 있음 | ps 완성 | 누적 계산 |
| 3 | Query | 여러 구간 | O(1) | 뺄셈/덧셈 |
| End | Output | result | 정답 | 출력 |

## 5. Pitfalls & Tips
* **1-based 핵심:** `ps[0]` / `ps[0][*]` / `ps[*][0]`은 경계 처리용 0
* **Query 공식:** 1D → `[l..r] = ps[r] - ps[l-1]`, 2D → `전체 - 위 - 왼쪽 + 대각선`
* **왜 -1을 쓰나:** `ps[i]`는 `1..i` 합이므로 시작 이전(`l-1`)을 빼야 함
* **0-based 원본 주의:** `ps[i] += arr[i-1]`, `ps[y][x] += grid[y-1][x-1]`
* **Index Rule:** 2D 좌표는 `(y, x)` 순서 고정
* **Overflow:** 합이 커지면 반드시 `long`
