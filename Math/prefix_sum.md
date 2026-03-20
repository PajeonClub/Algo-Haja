# 수학: 누적합 ( Prefix Sum )


### 소개

> 배열 일부 구간의 합을 빠르게 구하기 위해, 시작점부터 각 인덱스까지의 합을 미리 저장해두는 알고리즘

### 핵심 아이디어

> 💡 미리 계산해둔 결과를 캐싱하여 중복 연산을 줄인다!
* [L, R]의 합은 prefix_sum[R] - prefix_sum[L - 1] 로 구할 수 있다. 한 번의 연산으로 답을 구한다!

### 특징

- 원본 배열의 값이 중간에 바뀌지 않아야 한다!(Static)
- 특정 구간의 합을 여러 번 물어볼 때, 의미가 있다!
- 2차원 배열로 확장될 수도...

### 필요 자료구조

- 1차원 배열: 누적 합을 저장할 배열!

### 심화 / 유형

* 2차원 누적합: 특정 직사각형 영역의 합을 구할 때 사용
* 이모스법: 특정 구간에 특정 값을 더하는 기법

### 시간복잡도

> 💡O(N) 전처리 * O(1) 쿼리 + O(M) 쿼리 횟수 = O(N + M)
> 사용하지 않는다면, O(N) 쿼리 * O(M) 쿼리 횟수 = O(N*M)


### 구현 코드 (Kotlin)

```kotlin

/*
arr은 누적합을 구해야 하는 배열
query는 (from, to) 형식으로 저장된 배열
 */

fun getPrefixSum(arr: IntArray, query: Array<Pair<Int, Int>>): List<Int> {
    val prefixSum = IntArray(arr.size + 1) { 0 }

    // 누적 배열 초기화
    for (i in 0..arr.lastIndex) {
        prefixSum[i + 1] = prefixSum[i] + arr[i]
    }

    // 각 쿼리에 대해서 값 저장
    val ans = mutableListOf<Int>()
    for ((from, to) in query) {
        ans.add(prefixSum[to] - prefixSum[from - 1])
    }

    return ans
}


```

### 구현 코드 (Python)

```python

```

### 구현 코드 (C++)

```cpp

#include <numeric>
#include <utility>

using namespace std;

vector<long long> get_prefix_sum(const vector<int>& arr, const vector<pair<int, int>>& query) {

    vector<long long> prefixSum(arr.size() + 1, 0);

    partial_sum(arr.begin(), arr.end(), prefixSum.begin() + 1);

    vector<long long> ans;
    ans.reserve(query.size());

    for(const auto& [from, to]: query) {
        ans.push_back(prefixSum[to] - prefixSum[from - 1]);
    }

    return ans;
}

```