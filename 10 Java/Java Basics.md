---
type: template
tags: ["Java"]
---

# Java Basics

```java
int n = arr.length;
int m = s.length();
int size = list.size();
boolean same = s.equals(t);
long sum = (long) a + b;
long product = (long) a * b;
```

| 암기 포인트 | 이유 |
|---|---|
| `length / length() / size()` | 배열은 필드, 문자열·컬렉션은 메서드를 쓴다. |
| `s.equals(t)` | 문자열의 내용 비교다. ==는 같은 객체인지 비교한다. |
| `(long) a + b` | (long)(a+b)는 int에서 먼저 넘친 뒤 변환하므로 늦다. |

**예시**

```text
길이: 배열 length / 문자열 length() / 컬렉션 size()
"abc"와 새로 만든 "abc" → 내용 비교는 equals
20억 + 20억 → long으로 먼저 변환 → 40억
            → int로 먼저 계산   → 이미 넘침
```

**적용** 길이·비교·누적에서 Java 문법이 헷갈릴 때.

**준비** `arr` int[], `s,t` String, `list` List, `a,b` int.

**주의** 마지막 인덱스는 길이−1. char 비교는 `'a' == ch`로 한다.

[[Fast Input and Output]] · [[Sorting]] · [[Hashing]] · [[Stack and Queue]]
