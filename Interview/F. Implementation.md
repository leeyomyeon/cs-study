## 손코딩

1. 각종 정렬 알고리즘을 구현해 보세요.
2. 각종 자료구조 (ex. LinkedList, Queue, Stack, Tree, ...) 를 구현해 보세요.
3. 배열에 1,000,000 개의 수가 있다고 가정할 때, 여기에서 원하는 수가 몇 번째 인덱스에 위치해 있는지 확인하는 프로그램을 작성해 보세요. (단, 원하는 수가 하나가 아닐 수 있습니다.)
```c++
int N; cin << N;
vector<int> list;
for(int i = 0; i < N; i++) {
  int k; cin >> k;
  list.push_back(k);
}
sort(list.begin(), list.end());
int query; cin >> query;
while(query-->0) {
  int target; cin >> target;
  int s = 0, e = N, res = -1;
  while(s < e) {
    int mid = (s + e) / 2;
    if(list[mid] == target) {
      res = mid;
      break;
    }
    if(list[mid] < target) {
      s = mid + 1;
    } else {
      e = mid - 1;
    }
  }
  cout << res << '\n';
}
```
4. 20개의 숫자가 주어졌을 때, 이 수중 일부 숫자들을 더해서 원하는 수를 만들 수 있는지 확인하는 프로그램을 작성해 주세요.
```c++
vector<int> list(20);
for(int i = 0; i < 20; i++) {
  cin >> list[i];
}
sort(list.begin(), list.end());

```
5. 1 ~ 999 까지의 아라비아 숫자가 주어졌을 때, 이 숫자를 로마숫자로 변환하는 프로그램을 작성해 보세요.
6. 배열을 정렬하지 않고, 배열의 중앙값을 구하는 코드를 작성해 보세요.
```
```
7. 알파벳으로 이뤄진 단어가 있을 때, 이 단어에서 중복된 값을 제거하고 오름차순으로 정렬하는 코드를 작성해 보세요. (단, 최대한 효율적으로 작성해 주세요.)
8. 두 배열이 주어졌을 때, 이 배열의 **교집합**을 구하는 코드를 작성해 보세요. (단, 배열을 제외한 추가적인 자료구조는 사용하지 말아주세요.)
9. 길이가 N인 배열이 있을 때, 이 배열에서 K 번째로 큰 수를 추출하는 프로그램을 작성해 보세요. (단, 시간복잡도는 O(NlogN) 보다 작아야 합니다.)
[**K번째 수 문제**](https://www.acmicpc.net/problem/11004)
```c++
// priority queue (1200ms)
#include <bits/stdc++.h>
using namespace std;
int main() {
  int N, K; cin >> N >> K;
  priority_queue<int, vector<int>, greater<int>> pq;
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);
  for(int i = 0; i < N; i++) {
    int t; cin >> t;
    pq.push(t);
  }
  while(K-->1) {
    pq.pop();
  }
  cout << pq.top();
}
```
```c++
// Binary Search + radix sort (784ms)
#include <bits/stdc++.h>
using namespace std;
int N, K;
vector<int> v;
int kth(vector<int> &a, int k);
int main() {
    ios_base::sync_with_stdio(false); 
    cout.tie(nullptr); cin.tie(nullptr);
    cin >> N >> K;
    for(int i = 0; i < N; i++) {
        int k; cin >> k;
        v.push_back(k);
    }
    cout << kth(v, K);
}
int bucket = 45000; // ≒ sqrt(1'000'000'000)
int b_search(vector<int> &a, int target) {
    int s = 0;
    int e = bucket;
    int res = 0;
    while(s <= e) {
        int mid = (s + e) / 2;
        if(a[mid] >= target) {
            res = mid;
            e = mid - 1;
        } else {
            s = mid + 1;
        }
    }
    return res;
}
int kth(vector<int> &a, int k) {
    vector<int> cA(bucket);
    vector<int> cB(bucket);

    for(int & i : a) {
        i += 1000000000;
        cA[i / bucket]++;
    }
    // bucket + radix sort
    for(int i = 1; i < cA.size(); i++) {
        cA[i] += cA[i - 1];
    }
    int b = b_search(cA, k);
    for(int i : a) {
        if(i / bucket != b) {    // 범위 밖 값들은 정렬할 필요가 없음
            continue;
        }
        cB[i % bucket]++;
    }
    for(int i = 1; i < cB.size(); i++) {
        cB[i] += cB[i - 1];
    }
    int t = k - (b == 0 ? 0 : cA[b - 1]);
    int d = b_search(cB, t);
    return b * bucket + d - 1000000000;
}
```
10. 범위가 주어졌을 때 **랜덤 함수를 사용하지 않고,** 20개의 수를 랜덤으로 추첨해주는 프로그램을 작성해 보세요. (Hint: 시간)
```c++
// millisecond % K
#include<bits/stdc++.h>
#include <chrono>
#include <ctime>
using namespace std;
using std::chrono::duration_cast;
using std::chrono::milliseconds;
using std::chrono::seconds;
using std::chrono::system_clock;
int main() {
  long long N;
  cin >> N;
  long long millisec = duration_cast<milliseconds>(system_clock::now().time_since_epoch()).count();
  cout << millisec % N;
}
```
11. 2,000,000 자리의 숫자 여러개가 있고, 이 숫자들을 모두 더해야 한다고 합니다. 어떻게 코드를 작성하면 좋을까요? (Python을 사용하고 있다고 해도, 다른 언어의 long long 범위까지만 사용할 수 있다고 가정합시다.)
```c++
// FFT 이용
```
12. 정렬된 두 LinkedList를 합쳐 하나의 정렬된 LinkedList로 만드는 코드를 작성해 보세요.
13. 어떤 수가 주어졌을 때, 이 수의 소인수를 모두 구하는 코드를 작성해 보세요.
[**문제 링크**](https://www.acmicpc.net/problem/11653)
```c++
#include<bits/stdc++.h>
using namespace std;
int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr); cout.tie(nullptr);
  int N;
  cin >> N;
  if(N != 1) {
    int num = 2;
    while(num <= N) {
      if(N % num == 0) {
        cout << num << '\n';
        N /= num;
      } else {
        num++;
      }
    }
  }
}
```
[**코드 결과**](http://boj.kr/f4c8eb4e3ec84b628ed18b32e7bb57f4)

14. 1 부터 1,000,000 까지의 수를 이어 붙인다고 가정할 때, 이 수에서 0이 몇 번이나 등장하는지 확인하는 코드를 작성해 보세요.
```c++
// 10의 배수일 때 및 자리수(log)로 계산
```
15. 스택 두개로 큐를, 큐 두개로 스택을 구현하는 코드를 작성해 보세요.
