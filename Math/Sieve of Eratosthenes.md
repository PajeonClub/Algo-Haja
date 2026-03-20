# Math:Sieve of Eratosthenes


### 소개

> 2~N까지의 수 나열 -> 소수를 찾으면 그 배수를 전부 지운다. 남은 수가 전부 소수

에라토스테네스의 채는
주어진 범위의 모든 소수를 한 번에 구하는 전처리 알고리즘.\
합성수는 반드시 자기보다 작은 어떤 소수의 배수 형태로 표현되므로\
작은 수부터 차례로 배수를 제거하면 합성수는 모두 제거되고 마지막까지 남는 수가 소수이다.

소수 판별 알고리즘 X\
소수 생성(범위 전체 소수 찾기) 알고리즘 O

    ex)
    - N 이하 소수 전체 구하기
    - 소수 여부 빠르게 조회
    - 소수 합, 연속 소수 등 처리
    - 골드바흐의 추측 문제에도 사용 가능 (모든 짝수는 두 소수의 합으로 표현된다)


### 핵심 아이디어


<aside>
💡 

- **p^2부터 배수 제거 시작** -> p^2보다 작은 배수는 이미 더 작은 소수 단계에서 제거됨
- **√N까지만 탐색** -> 합성수는 반드시 √N 이하의 약수를 하나 이상 가짐

</aside>

### 특징

- 범위 전체 소수 구할 때 매우 빠른 알고리즘
- 소수 여부를 O(1)로 조회 가능 (배열 접근)
- 전처리형 알고리즘 (한 번 계산 후 반복 사용)
- 메모리 사용량 O(N)
- √N까지만 탐색하여 시간 최적화
- p^2부터 제거하여 이미 처리된 작은 배수를 다시 확인하는 낭비를 줄임

- **홀수 배열 최적화**
    짝수는 2를 제외하면 모두 합성수

    -> 따라서 홀수만 저장하면 메모리 절반 절약 가능\
        + 인덱스 변환 주의   
        ```
        number=2*i+3, i=(number-3)//2
        ```

### 필요 자료구조

- Boolean 배열 (is_prime[i] = i 소수 여부)
- 소수 리스트 저장용 배열 (primes = [])

### 심화 / 유형
1. 구간 소수 문제 (m~n 소수 출력) - BOJ 1929
2. 다중 test case - BOJ 4948
3. 소수 합 / 골든 바흐 - BOJ 9020
4. 소수 리스트 + two pointer - BOJ 1644

### 시간복잡도

<aside>
💡 

**O(N log log N)**


    1. 각 소수 p마다 약 N/p개의 배수를 제거한다.
    2. 만약 모든 자연수 단계가 수행된다면 전체 연산량은 N log N이 된다. 
       (조화급수: 각 항이 자연수의 역수인 무한급수) 
    3. 하지만 합성수 단계는 이미 제거되므로 소수 단계만 수행
    4. 소수 역수의 합은 log log N으로 증가하므로 전체 시간복잡도는 O(N log log N)이다. 

</aside>

### 구현 코드 (Kotlin)

```kotlin

fun sieve(n: Int): BooleanArray {
    if (n < 2) return booleanArrayOf()

    val isPrime = BooleanArray(n + 1) { true }

    isPrime[0] = false
    isPrime[1] = false

    var prime = 2
    while (prime * prime <= n) {
        if (isPrime[prime]) {
            var composite = prime * prime
            while (composite <= n) {
                isPrime[composite] = false
                composite += prime
            }
        }
        prime++
    }

    return isPrime
}

```

### 구현 코드 (Python)

```python
def sieve(n):
    is_prime = [True] * (n+1)
    if n>=0:
        is_prime[0] = False
    if n>=1:
        is_prime[1] = False
    for i in range(2, int(n**0.5)+1):
        if is_prime[i]:
            for j in range(i*i, n+1, i): 
                is_prime[j] = False
    return is_prime
```

### 구현 코드 (C++)

```cpp

#include <iostream>
#include <vector>

using namespace std;

vector<bool> sieve(int n) {
    if (n < 2)
        return vector<bool>();

    vector<bool> is_prime(n + 1, true);

    is_prime[0] = is_prime[1] = false;

    for (int prime = 2; prime * prime <= n; prime++) {
        if (is_prime[prime]) {
            for (int composite = prime * prime; composite <= n; composite += prime) {
                is_prime[composite] = false;
            }
        }
    }
    return is_prime;
}

```