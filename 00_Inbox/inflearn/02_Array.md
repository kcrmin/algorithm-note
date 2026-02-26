---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 2 - Array

## 1. Array 기초

### 기본 조작
| 코드                        |    요약 / 결과     | 예시                   |
|:--------------------------|:--------------:|:---------------------|
| `int[] arr = new int[n];` | 배열 선언 (기본값 0)  | `n=3 → [0,0,0]`      |
| `arr.length`              |     배열 길이      | `[1,2,3].length → 3` |
| `for (int x : arr)`       |    향상된 for문    | 모든 원소 순회             |
| `Arrays.toString(arr)`    |     배열 출력      | `[1, 2, 3]`          |

### 초기화 / 복사
| 코드                                     |    요약 / 결과     | 예시             |
|:---------------------------------------|:--------------:|:---------------|
| `Arrays.fill(arr, -1);`                |     전체 초기화     | `[-1, -1, -1]` |
| `int[] copy = arr.clone();`            |     복사본 생성     | 값만 복사          |
| `int[][] a = new int[n][m];`           |     2차원 배열     | 기본 0           |
| `for (int[] r : a) Arrays.fill(r, 0);` |     2D 초기화     | 모든 행 0         |

### *정렬 / 탐색
| 코드 | 요약 / 결과 | 예시 |
|------|--------------|------|
| `Arrays.sort(arr);` | 오름차순 정렬 | `[3,1,2]→[1,2,3]` |
| `Arrays.sort(arr, Collections.reverseOrder());` | 내림차순 (객체형) | `Integer[]` 필요 |
| `Arrays.binarySearch(arr, key);` | 정렬 후 탐색 | 인덱스 or 음수 |


## 2. 기본 코드
### 2-1 배열 입력 (1D / 2D)
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        // 배열 입력
        int n = scanner.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) arr[i] = scanner.nextInt();

        // 행렬 입력
        int m = scanner.nextInt();
        int[][] matrix = new int[n][m];
        for (int y = 0; y < n; y++) {
            for (int x = 0; x < m; x++) {
                matrix[y][x] = scanner.nextInt();
            }
        }
    }
}
```

### 2-2. 2차원 누적합 (Prefix Sum 2D) 템플릿
```java
int[][] prefix = new int[n + 1][n + 1];

// init prefix sum
for (int y = 1; y <= n; y++) {
    for (int x = 1; x <= n; x++) {
        prefix[y][x] = matrix[y - 1][x - 1]
                        + prefix[y][x - 1]
                        + prefix[y - 1][x]
                        - prefix[y - 1][x - 1]; 
    }
}

// get value
int result = 0;
for (int y = m; y <= n; y++) {
    for (int x = m; x <= n; x++) {
        int temp = prefix[y2][x2]
                     - prefix[y1 - 1][x2]
                     - prefix[y2][x1 - 1]
                     + prefix[y1 - 1][x1 - 1];
    }
}
```


## 3. 주의 포인트
|  코드 / 개념  | 요약                                                |
|:---------:|:--------------------------------------------------|
| 인덱스 범위 주의 | 0 ~ n-1 (Off-by-one 주의)                           |
| 얕은 복사 주의  | `b=a`는 참조 공유, `arr.clone();` 사용                   |
|   행렬 접근   | `matrix[i][j]` <br/>- `i: 세로(행)`<br/>- `j: 가로(열)` |


## **핵심 요약**
- 정렬: `Arrays.sort()`
- 복사: `clone()`

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section02_array)**
| 분류  | 문제 이름                                                                      | 포인트 메모     |
|:---:|:---------------------------------------------------------------------------|:-----------|
| 기초  | [02-05 소수(에라토스테네스 체)](../../src/inflearn/section02_array/INF_S02_P05.java) | O(n)으로 처리  |
| 기초  | [02-06 뒤집은 소수](../../src/inflearn/section02_array/INF_S02_P06.java)        | 1은 소수가 아니다 |
| 응용  | [02-11 임시반장 정하기](../../src/inflearn/section02_array/INF_S02_P11.java)      | 문제 잘 읽기    |
| 고난도 | [02-12 멘토링](../../src/inflearn/section02_array/INF_S02_P12.java)           | 문제 잘 읽기    |