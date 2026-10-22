---
type: template
tags: ["Strings"]
aliases: ["Rolling Hash"]
category: string-algorithms
categories: [string-algorithms]
---

# String Hashing

```text
문자열   a b [c d] e
인덱스   0 1  2 3  4      query [2,4)

H(abcd)  = H(ab) × B² + H(cd)
           └── 제거 ──┘
H(cd)    = H(abcd) - H(ab) × B²  (mod M)
         = 3 × 31 + 4 = 97
```

| Step | Pseudocode |
| --- | --- |
| Build | // 앞부분의 해시와 밑의 거듭제곱을 저장한다. |
| Append | // 기존 해시를 한 자리 밀고 새 문자의 값을 붙인다. |
| Align | // 제거할 앞부분을 구간 길이만큼 밀어 자릿수를 맞춘다. |
| Remove | // 끝까지의 해시에서 앞부분을 빼고, 나머지를 음수가 아니게 보정한다. |

## Java

```java
long MOD = 1_000_000_007L, BASE = 31;
long[] prefix, power;

void build(String text) {
    prefix = new long[text.length() + 1];
    power = new long[text.length() + 1];
    power[0] = 1;

    for (int i = 0; i < text.length(); i++) {
        long value = text.charAt(i) - 'a' + 1;
        prefix[i + 1] = (prefix[i] * BASE + value) % MOD;
        power[i + 1] = power[i] * BASE % MOD;
    }
}

long hash(int left, int right) {
    // [left, right): 제거할 앞부분의 자릿수를 먼저 맞춘다.
    long removed = prefix[left] * power[right - left] % MOD;
    return (prefix[right] - removed + MOD) % MOD;
}
```

## Watch points

| 선택 | 이유 / 실수 |
| --- | --- |
| `[left, right)` | 오른쪽 끝 제외. 길이는 `right - left`; 빈 구간의 해시는 0이다. |
| `power[right - left]` | 앞부분 해시를 그대로 빼면 자릿수가 달라진다. 구간 길이만큼 곱한 뒤 뺀다. |
| `+ MOD` 후 `% MOD` | Java의 음수 나머지를 막는다. 두 해시가 이미 0 이상 MOD 미만이라 한 번 더하면 충분하다. |
| 같은 해시 ≠ 같은 문자열 | 충돌이 가능하다. 정확한 일치가 필요하면 길이와 원문을 확인한다. |

`build(text)` 후 사용 · 소문자 a~z · 0≤left≤right≤n · 전처리·공간 O(n), 조회 O(1).

**분류** [[00 Maps/String Algorithms|String Algorithms]]
