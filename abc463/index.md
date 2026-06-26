---
title: ABC463 参加記
date: 2026-06-26
draft: true
pin: false
tags:
  - atcoder
  - abc
---
  
# 初参加です
なんとなく参加したんですけど楽しくて毎週やると思うので記録つけていきます。
D,E,Fは挑戦すらしてないので書いてないです。
# A - 16:9
解くだけ、といってはあれですが…
x:y = 16:9なので、x\*9 = y \*16が成り立てば良いですね。

```cpp
#include <atcoder/all>
#include <bits/stdc++.h>
using namespace std;
using namespace atcoder;

using ll = long long;

#define rep(i, n) for (int i = 0; i < (int)(n); i++)
#define all(x) (x).begin(), (x).end()

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);

  int X, Y;
  cin >> X >> Y;
  if (X * 9 == Y * 16) {
    cout << "Yes" << endl;
  } else {
    cout << "No" << endl;
  }

  return 0;
}
```


# B - Train Reservation
全ての列車のx列がoになっているかを判定すればいいです。
```cpp
#include <atcoder/all>
#include <bits/stdc++.h>
using namespace std;
using namespace atcoder;

using ll = long long;

#define rep(i, n) for (int i = 0; i < (int)(n); i++)
#define all(x) (x).begin(), (x).end()

int main() {
  ios::sync_with_stdio(false);
  cin.tie(nullptr);

  int N;
  char S;
  cin >> N >> S;
  vector<string> v(N);
  for (string &x : v) {
    cin >> x;
  }

  map<char, int> m;
  m['A'] = 0;
  m['B'] = 1;
  m['C'] = 2;
  m['D'] = 3;
  m['E'] = 4;

  rep(i, N) {
    if (v[i][m[S]] == 'o') {
      cout << "Yes" << endl;
      return 0;
    }
  }
  cout << "No" << endl;

  return 0;
}
```

解説を見ると計算でx列を求めているみたいです。
英大文字は文字コード上で順番に並んでいるだけなので、`'A'`からどれだけ進んでいるかを計算することで求まります。(初めて知った❗)
```cpp
if (v[i][S - 'A'] == 'o') {
	...
}
```
ってことらしい
# C - Tallest at the Moment
クエリ時刻`q + 0.5`に残っている人の身長最大値を求める問題です。
純粋に全探索する場合、クエリ時刻以降に残っている人を毎回探索し最悪計算量はO(NQ)となります。
```cpp
int ans = 0;
for (int i = index; i < N; i++) {
  ans = max(ans, H[i]);
}
```
そこで、事前に残っている人の身長順にソートされた配列を持っておくことを考えました。


```cpp
```