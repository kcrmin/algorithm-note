---
deprecated: true
note: "Inbox 정리용 노트 (deprecated)"
---

# 섹션 3 - Two Pointers

## 1. Two Pointers 기초

### 기본 구조
```java
public class Main {
    public static void main(String[] args) {
        int lt = 0, rt = 0, sum = 0, ans = 0;
        while (rt < n) {
            // 확장
            sum += arr[rt++];
            
            // 축소
            while (sum > k) sum -= arr[lt++];
            
            // 최대 구간
            ans = Math.max(ans, rt - lt);
        }
    }
}
```
> `확장` → `조건 확인` → `축소` 패턴은 모든 투포인터의 기본 형태

### 고정 길이 (슬라이딩 윈도우)
```java
public class Main {
    public static void main(String[] args) {
        int sum = 0;
        int lt = 0;
        
        // 초기 윈도우 세팅 (0 ~ k-1)
        for (int i = 0; i < k; i++) sum += arr[i];
        int ans = sum;
        
        // 메인 슬라이딩 (k-1: 시작점)
        for (int i = k; i < n; i++) {
            // 이동 (확장 + 축소)
            sum += arr[i] - arr[i - k];
            
            // 조건 확인
            ans = Math.max(ans, sum);
        }
    }
}
```
> 처음 k개의 구간을 윈도우에 초기화 후 메인으로 슬라이딩

### 연속된 부분합 = M 
```java
public class Main {
    public static void main(String[] args) {
        int lt = 0, sum = 0, cnt = 0;
        
        for (int rt = 0; rt < n; rt++) {
            // 확장
            sum += arr[rt];
            
            // 축소
            while (sum > M) sum -= arr[lt++];
            
            // 조건 확인
            if (sum == M) cnt++;
        }
    }
}
```
> 대표적인 “수들의 합” / “연속된 부분 수열” 문제 기본형

### 서로 다른 원소 K개 이하 (HashMap 활용)
```java
public class Main {
    public static void main(String[] args) {
        int lt = 0, ans = 0;

        for (int rt = 0; rt < n; rt++) {
            // 확장
            map.put(arr[rt], map.getOrDefault(arr[rt], 0) + 1);
            
            // 축소
            while (map.size() > K) {
                map.put(arr[lt], map.get(arr[lt]) - 1);
                if (map.get(arr[lt]) == 0) map.remove(arr[lt]);
                lt++;
            }
            
            // 조건 확인
            ans = Math.max(ans, rt - lt + 1);
        }
    }
}
```
> “서로 다른 K개 이하 원소” → 문자열/배열 모두 응용 가능


## 3. 주의 포인트
| 코드 / 개념 | 요약                                  |
|:-------:|:------------------------------------|
| 고정 윈도우  | 윈도우 초기화 후 슬라이딩 윈도우                  |
| 가변 윈도우  | `if`가 아닌 `while` 사용해서 조건 만족할 때까지 축소 |


## **핵심 요약**
- `확장(rt++)` → `조건 확인` → `축소(lt++)`
- “최대/최소 구간 길이”, “부분합 카운트” 문제는 거의 이 패턴

## 문제 다시 풀기
### **[Inflearn](../../src/inflearn/section03_two_pointers)**
| 분류  | 문제 이름                                                                             | 포인트 메모    |
|:---:|:----------------------------------------------------------------------------------|:----------|
| 기초  | [03-03: 최대 매출](../../src/inflearn/section03_two_pointers/INF_S03_P03.java)        | O(n)으로 풀기 |
| 기초  | [03-04: 연속_부분수열](../../src/inflearn/section03_two_pointers/INF_S03_P04.java)      | 가변 윈도우    |
| 감잡기 | [03-06: 최대 길이 연속부분수열](../../src/inflearn/section03_two_pointers/INF_S03_P06.java) | 감잡기 좋은 문제 |