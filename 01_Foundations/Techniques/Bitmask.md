---
type: technique
tags:
  - bitmask
  - bit-flag
language: java
status: in-review
source: []
---

# Bit Flag & Bitmask
> **한 줄 요약:** 비트로 권한/상태를 관리하고, mask로 부분집합을 처리하는 코딩 테스트 기본기

## Core Concept
* **When to use:**  
  - 여러 ON/OFF 상태를 하나의 정수로 관리할 때  
  - 부분집합을 전수 탐색해야 할 때
* **Key Point:**
  - Bit Flag: 비트 단위 상태 관리
  - Bitmask: mask 하나 = 하나의 부분집합

## API & State Change (if applicable)
| Operation | Code Snippet | 설명 |
| :--- | :--- | :--- |
| Init | `int flags = 0;` | 초기화 |
| Add | `flags \|= PERM;` | 플래그 추가 |
| Remove | `flags &= ~PERM;` | 플래그 제거 |
| Toggle | `flags ^= PERM;` | 플래그 토글 |
| Check | `(flags & PERM) != 0` | 보유 여부 |
| Count | `Integer.bitCount(flags)` | 켜진 개수 |

## Patterns
### Pattern A: Bit Flag (권한/기능 관리)
```java
// 각 비트가 하나의 권한 또는 기능
final int READ  = 1 << 0;   // 0001
final int WRITE = 1 << 1;   // 0010
final int EXEC  = 1 << 2;   // 0100
final int ADMIN = 1 << 3;   // 1000

int flags = 0;              // 0000

// 권한 추가
flags |= READ;              // 0001
flags |= WRITE;             // 0011

// 권한 확인
boolean canRead  = (flags & READ)  != 0;
boolean canExec  = (flags & EXEC)  != 0;

// 권한 제거
flags &= ~WRITE;            // 0001

// 토글
flags ^= EXEC;              // 0101
flags ^= EXEC;              // 0001

// 여러 권한을 한 번에 처리
flags |= (READ | EXEC);     // 0101
flags &= ~(READ | EXEC);    // 0000

// 둘 다 가지고 있는지
boolean hasReadAndWrite =
    (flags & (READ | WRITE)) == (READ | WRITE);

// 하나라도 가지고 있는지
boolean hasReadOrExec =
    (flags & (READ | EXEC)) != 0;

// 켜진 권한 개수
int cnt = Integer.bitCount(flags);
```

### Pattern B: Bitmask (부분집합 처리)
```java
for (int mask = 0; mask < (1 << N); mask++) {
    // mask 자체가 하나의 부분집합
    for (int i = 0; i < N; i++) {
        if ((mask & (1 << i)) != 0) {
            // 부분집합에 포함된 i번째 원소 처리
        }
    }
}
```

## Pitfalls & Tips
- 인덱스 실수 방지를 위해 항상 괄호 사용
- 합성 플래그는 OR로 묶어서 처리
- 코테에서는 단순한 for-loop가 가장 안전
