---
title: "이분 탐색"
date: 2020-12-23 00:00:00
categories:
- Tutorial
tags:
- Tutorial
---

정렬된 배열에서 사용할 수 있는 강력한 탐색 알고리즘인 이분 탐색에 대해 알아봅시다.

### 업다운 게임
이런 게임을 생각해봅시다.

> A는 마음 속으로 1부터 100 사이의 자연수를 하나 생각합니다.<br>
B는 A가 생각한 수를 맞춰야 합니다.<br>
B가 어떤 수를 외치면 A는 그 수가 자신이 생각한 수보다 큰지, 작은지, 같은지 알려줍니다.

가운데 수를 부르는 것이 좋다는 것을 직관적으로 생각할 수 있습니다.<br>
수를 한 번 외칠 때마다 정답이 존재할 수 있는 구간이 항상 절반씩 줄어들기 때문에 $\lceil log_2 N \rceil$번만 외치면 항상 정답을 구할 수 있습니다.

이런 방식으로 **정렬된 배열**에서 원하는 값을 찾는 알고리즘을 이분 탐색이라고 합니다.

### 이분 탐색
크기가 $N$인 정렬된 배열 $A$에서 어떤 값 $x$가 몇 번째 위치에 있는지 구하는 알고리즘입니다.

초기에는 정답은 구간 $[1, N]$ 안에 존재합니다. 구간 $[1, N]$ 안에 있는 적당한 수 $M$을 선택해서 $x$와 $A_M$을 비교합시다.
* 만약 $x$가 $A_M$과 같다면 정답은 $M$이 됩니다.
* 만약 $x$가 $A_M$보다 작으면 정답은 $M$보다 **왼쪽**에 존재합니다. 정답이 존재하는 구간은 $[1, M-1]$로 바뀝니다.
* 만약 $x$가 $A_M$보다 크면 정답은 $M$보다 **오른쪽**에 존재합니다. 정답이 존재하는 구간은 $[M+1, N]$으로 바뀝니다.

정답이 존재하는 구간이 $[s, e]$일 때 M을 어떻게 잡는지에 따라 최악의 경우 시간 복잡도가 달라집니다.
* 만약 $M = s$라면 최악의 경우 구간의 크기가 1씩 감소합니다. 시간 복잡도는 $O(N)$이 됩니다.
* 만약 $M = \lfloor \frac{s+e}{2} \rfloor$라면 최악의 경우 구간의 크기가 절반씩 감소합니다. 시간 복잡도는 $O(\log N)$입니다.
* 만약 $M = e$라면 최악의 경우 구간의 크기가 1씩 감소합니다. 시간 복잡도는 $O(N)$이 됩니다.

$M = t$로 잡으면 구간의 크기가 $t-s+1$만큼 감소하거나, $e-t+1$만큼 감소합니다. 최악의 경우에는 둘 중 구간이 더 커지는 쪽으로 이동하기 때문에, $t-s+1$과 $e-s+1$이 되도록 같아지도록 선택해야 합니다.<br>
그러므로 중점인 $M = \lfloor \frac{s+e}{2} \rfloor$로 선택하는 것이 최선입니다.

이때 시간 복잡도는 $O(\log N)$입니다.

#### 연습 문제 - BOJ10815 숫자 카드
[문제 링크](http://icpc.me/10815)

이분 탐색을 구현하는 문제입니다.
```cpp
#include <bits/stdc++.h>
using namespace std;

int N, M, A[505050];

int is_exist(int x){
    int s = 1, e = N; // 정답이 존재하는 구간 [s, e]
    while(s <= e){
        int m = (s + e) / 2;
        if(x == A[m]) return 1;
        else if(x < A[m]) e = m - 1; // [s, m-1]
        else s = m + 1; // [m+1, e]
    }
    return 0;
}

int main(){
    ios_base::sync_with_stdio(false); cin.tie(nullptr);
    cin >> N;
    for(int i=1; i<=N; i++) cin >> A[i];
    sort(A+1, A+N+1);
    cin >> M;
    for(int i=1; i<=M; i++){
        int x; cin >> x;
        cout << is_exist(x) << " ";
    }
}
```

이진 탐색을 직접 구현하지 않고, C++에 내장된 binary_search 함수를 사용해도 됩니다. ([cppreference](https://en.cppreference.com/w/cpp/algorithm/binary_search))

```cpp
#include <bits/stdc++.h>
using namespace std;

int N, M, A[505050];

int main(){
    ios_base::sync_with_stdio(false); cin.tie(nullptr);
    cin >> N;
    for(int i=1; i<=N; i++) cin >> A[i];
    sort(A+1, A+N+1);
    cin >> M;
    for(int i=1; i<=M; i++){
        int x; cin >> x;
        cout << binary_search(A+1, A+N+1, x) << " ";
    }
}
```

#### lower bound와 upper bound
문제를 풀 때 자주 등장하는 개념입니다.

오름차순으로 정렬된 배열 A와 어떤 값 K가 있을 때
* 정렬된 상태를 유지하면서 넣을 수 있는 첫 위치를 lower bound라고 합니다.
* 정렬된 상태를 유지하면서 넣을 수 있는 마지막 위치를 upper bound라고 합니다.

A가 [1, 2, 3, 3, 3, 4]이고 K가 3이라고 합시다. 이때 lower bound는 2, upper bound는 5입니다. (0-based)

첫 위치 / 마지막 위치가 잘 이해가 안 된다면 **K 이상의 수**가 등장하는 첫 위치 / **K 초과의 수**가 등장하는 첫 위치로 생각해도 좋습니다.

이 위치를 구하는 것 또한 이분 탐색으로 가능합니다.
```cpp
int lower_bound(){
    if(A[N] < K) return N;
    int s = 0, e = N-1;
    while(s < e){
        int m = (s + e) / 2;
        if(K <= A[m]) e = m; // A[m]이 K 이상이라면 정답은 m 이하
        else s = m + 1; // 그렇지 않다면 정답은 m 초과
    }
    return e;
}
int upper_bound(){
    if(A[N] < K) return N;
    int s = 0, e = N-1;
    while(s < e){
        int m = (s + e) / 2;
        if(K < A[m]) e = m; // A[m]이 K 초과라면 정답은 m 이하
        else s = m + 1; // 그렇지 않다면 정답은 m 초과
    }
    return e;
}
```

lower bound와 upper bound도 C++에 내장이 되어 있습니다. 앞으로 강의에서 많이 사용할 것이기 때문에 알아두는 것이 좋습니다. ([cppreference-lower bound](https://en.cppreference.com/w/cpp/algorithm/lower_bound), [cppreference-upper bound](https://en.cppreference.com/w/cpp/algorithm/upper_bound))

### Parametric Search
Parametric Search는 최적화 문제를 결정 문제로 바꿔서 푸는 것을 의미합니다.<br>
이때 최적화 문제는 주어진 조건을 만족하는 최댓값/최솟값을 찾는 문제를, 결정 문제는 "이것이 가능한가?"를 판단하는 문제를 의미합니다. 예시 문제를 봅시다.

#### 예시 문제 - BOJ2805 나무 자르기
[문제 링크](http://icpc.me/2805)

나무를 $M$ 이상 가져갈 수 있는 절단 높이의 최댓값을 구하는 **최적화 문제**입니다.<br>
**높이가 $h$인 지점에서 잘랐을 때 $M$ 이상 가져갈 수 있는지 확인**하는 결정 문제로 바꿔봅시다. 이 문제는 $O(N)$에 풀 수 있습니다.
```cpp
bool decision(ll h){
	long long sum = 0;
	for(int i=0; i<n; i++){
		if(tree[i] > h) sum += (tree[i] - h);
	}
	return sum >= m;
}
```

$M$ 이상 가져갈 수 있으면 1, 없으면 0이라고 합시다. 어떤 지점 $P$가 있어서 $h \leq P$이면 1일 것이고, $h > P$이면 0일 것입니다. 이때 $P$를 찾는 것이 문제이며, 이것은 이분 탐색으로 해결할 수 있습니다.

```cpp
long long l = 0, r = 2e9;
while(l < r){
  long long mid = (l + r + 1) / 2;
  if(decision(m)) l = m; // 정답은 m 이상
  else r = m - 1; // 정답은 m 미만
}
cout << l;
```

#### Parametric Search의 구현에 대한 이야기
사람마다 Parametric Search를 구현하는 방법이 다 다릅니다. 특히 off-by-one 에러가 많이 발생할 수 있기 때문에 한 가지 스타일을 완벽히 익힐 필요가 있습니다. 여기에서는 제가 구현하는 방법을 소개합니다.

**최솟값을 구하는 경우**
```cpp
while(l < r){
  int m = (l + r) / 2;
  if(decision(m)) r = m; // 정답이 m 이하
  else l = m + 1; // 정답이 m 초과
}
return r;
```

**최댓값을 구하는 문제**
```cpp
while(l < r){
  // (l+r)/2로 하는 경우 l = 7, r = 8, decision(7) = true이면 무한 루프에 빠진다.
  int m = (l + r + 1) / 2;
  if(decision(m)) l = m; // 정답이 m 이상
  else r = m - 1; // 정답이 m 미만
}
return l;
```
