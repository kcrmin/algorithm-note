---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# Java Array Basic (1D / 2D / 3D)
> **한 줄 요약:** 코딩테스트에서 가장 많이 쓰는 배열 선언/초기화/순회/복사 패턴을 1차원~3차원까지 정리

## 1. Core Concept
* **When to use:**  
* **Key Point:**
    * 배열은 **고정 길이**이고 인덱스로 **O(1) 접근**
    * 2D/3D는 “배열의 배열” 구조 (`int[][]`, `int[][][]`)
    * 초기값은 타입별 기본값으로 자동 초기화 (`int=0`, `boolean=false`, `char='\u0000'`, `ref=null`)
    * 반복 초기화/출력/복사 패턴을 익혀두면 실수 감소
* **Time Complexity:** O(1) 접근, O(N) 순회 (차원 포함)

## 2. API & State Change
| Operation | Code Snippet | State / Return | 설명 |
| :--- | :--- | :--- | :--- |
| Init (1D) | `int[] a = new int[n];` | 배열 생성 | 길이 n, 0으로 초기화 |
| Init (2D) | `int[][] g = new int[r][c];` | 배열 생성 | r행 c열 격자 |
| Init (3D) | `int[][][] v = new int[z][y][x];` | 배열 생성 | 3차원 공간 |
| Access | `a[i]`, `g[y][x]`, `v[z][y][x]` | 값 | 인덱스 접근 O(1) |
| Update | `a[i]=k;` | 상태 변경 | 값 갱신 |
| Length | `a.length`, `g.length`, `g[0].length` | int | 길이/행/열 크기 |
| Fill | `Arrays.fill(a, val)` | 상태 변경 | 1D 빠른 초기화 |
| Copy (1D) | `Arrays.copyOf(a, n)` | 새 배열 | 길이 n으로 복사 |
| Copy Range | `Arrays.copyOfRange(a,l,r)` | 새 배열 | [l,r) 범위 복사 |
| Deep Copy (2D) | `for ... copyOf(row)` | 새 2D | 행 단위로 복사 |

## 3. Code Templates (Patterns)

### Pattern A: 1D [Standard Form]
설명: 선언/입력/순회/최솟값·합 계산의 기본 패턴
```java
int n = 5;
int[] a = new int[n];        // [0,0,0,0,0]

for (int i = 0; i < n; i++) {
    a[i] = i + 1;            // [1,2,3,4,5]
}
```

### Pattern B: 1D [Advanced/Optimized Form]
설명: 빠른 초기화/복사/부분 복사 (실수 많이 나는 구간)
```java
import java.util.Arrays;

int[] a = new int[5];
Arrays.fill(a, 7);  // [7,7,7,7,7]

int[] b = Arrays.copyOf(a, a.length);  // b=[7,7,7,7,7] (새 배열)
b[0] = 99;  // a는 그대로, b만 변경

int[] c = Arrays.copyOfRange(b, 1, 4);  // [7,7,7] (인덱스 1~3)
```

### Pattern C: 2D [Standard Form]
설명: 격자(행/열) 선언과 순회 (BFS/DFS/DP 기본)
```java
int r = 3, c = 4;
int[][] g = new int[r][c]; // 모든 값 0

for (int y = 0; y < r; y++) {
    for (int x = 0; x < c; x++) {
        g[y][x] = y * 10 + x;
    }
}

// g[0] = [0,1,2,3]
// g[1] = [10,11,12,13]
// g[2] = [20,21,22,23]
```

### Pattern D: 2D [Advanced/Optimized Form]
설명: 2D 깊은 복사(Deep Copy)와 행/열 크기 주의
```java
import java.util.Arrays;

int[][] g = new int[2][3];
g[0][0] = 5;

int[][] copy = new int[g.length][];
for (int y = 0; y < g.length; y++) {
    copy[y] = Arrays.copyOf(g[y], g[y].length);  // 행 단위 복사
}

copy[0][0] = 99;  // 원본 g[0][0]는 5 그대로
```

### Pattern E: 3D [Standard Form]
설명: 3차원 상태(층/행/열) 선언과 순회 (3D BFS, DP 상태공간)
```java
int z = 2, y = 3, x = 4;
int[][][] box = new int[z][y][x];

for (int k = 0; k < z; k++) {
    for (int i = 0; i < y; i++) {
        for (int j = 0; j < x; j++) {
            box[k][i][j] = k * 100 + i * 10 + j;
        }
    }
}
// box[1][2][3] = 123
```

### Pattern F: 3D [Advanced/Optimized Form]
설명: 좌표 이동(6방향) 패턴 (3D BFS에서 거의 고정)
```java
// (z, y, x) 6방향 이동
int[] dz = { 1, -1,  0,  0,  0,  0};
int[] dy = { 0,  0,  1, -1,  0,  0};
int[] dx = { 0,  0,  0,  0,  1, -1};

// next: (nz, ny, nx) = (cz+dz[d], cy+dy[d], cx+dx[d])
```

## 4. Pitfalls & Tips
* **Edge Case:** 0 길이/1 길이, 인덱스 경계(`0 <= i < length`)
* **2D 길이:** `g.length`는 행 수, `g[0].length`는 열 수  
  (비정방/가변 행 배열도 가능하므로 행마다 열 길이가 다를 수 있음)
* **Shallow Copy 주의:** `int[][] copy = g;`는 같은 참조 공유 (복사 아님)  
  2D는 행 단위로 `copyOf`해서 Deep Copy
* **Performance:** 대량 초기화는 1D는 `Arrays.fill`이 편함. 2D/3D는 보통 루프 사용
* **Trick:** 격자 문제는 `(y,x)` 좌표를 습관처럼 고정하고, 방향 배열(`dy/dx`)로 실수 줄이기