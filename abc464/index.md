---
title: ABC464 参加記
date: 2026-07-01
draft: true
pin: false
tags:
---
# 2回目です
~~1週間遅れくらいなんですけどabc464の参加記事です。~~
この記事を書いている途中で関西にお出かけ🎶していて、なんと2週間遅れになりました
そして、楽しすぎて465に参加していません。467からまた頑張ります。。
前回D問題が解けなかったのが悔しすぎて、色々なアルゴリズムを勉強しました。
##  勉強したアルゴリズム
- [[動的計画法(DP)]]
	- 1次元DP、2次元DP、ナップザック問題などを解く練習をしました
- データ構造
	- Union-Find, セグ木, Set/Mapなど
- 探索
	- 初回挑戦の前に少し勉強していましたが、[[幅優先探索(BFS)]]や[[深さ優先探索(DFS)]]なども勉強しました
- [[貪欲法]]
	- 前回解けなかった区間スケジューリング問題、みたいな

# A - Decisive Battle
文字列`S`に含まれる`E`, `W`の数を数える問題です。
制約より、`S`は奇数なので`E = W`になる可能性は考慮する必要がありません。
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

  string S;
  cin >> S;

  int w = count(S.begin(), S.end(), 'W');
  int e = count(S.begin(), S.end(), 'E');

  if (w > e)
    cout << "West" << endl;
  else
    cout << "East" << endl;

  return 0;
}

```

# B - Crop
`.`と`#`を含む文字列がグリッドとして与えられます。
`.`を白、`#`を黒として画像の上下左右から全てが白い行・列を削除する実装を行います。
つまり、考えなければならないものは「すべての`#`を含む最小のグリッドを作る」ということです。(= `#`を削除してはいけない)

つまり、`#`が存在する最も上の行と下の行、左の列と右の列を調べその範囲を出力することで解くことが出来ると考えました。

実装としては、上下左右を二重ループで全探索し、
- 一番上の`#`
- 一番右の`#`
- 一番左の`#`
- 一番下の`#`
の位置をそれぞれ見つければよいです。

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

  int H, W;
  cin >> H >> W;
  vector<string> C(H);
  for (string &x : C) {
    cin >> x;
  }

  int t = H;
  int l = W;
  int r = -1;
  int b = -1;

  rep(i, H) {
    rep(j, W) {
      if (C[i][j] == '#') {
        t = min(t, i);
        l = min(l, j);
        r = max(r, j);
        b = max(b, i);
      }
    }
  }

  for (int i = t; i <= b; i++) {
    for (int j = l; j <= r; j++) {
      cout << C[i][j];
    }
    cout << endl;
  }

  return 0;
}

```

# C - Plumage Palette
C問題以降は、A,B問題と異なり工夫しないと間に合わないです。
単純に全ての日、全ての鳥の色を調べると計算量は単純に`O(MN)`となります。
制約では
```
N <= 3 * 10^5
M <= 3 * 10^5
```
なので最大計算量は`9 * 10^5`となり全く間に合わないです。

色々問題を整理している途中である点に気づきました。
問題の次のようなステップで進みます。
1. 最初は色Aiの鳥が1羽いる
2. Di日目に、色Aiの鳥が一羽減り、色Biの鳥が一羽増える
そして、色Biに変化した鳥は変化することがない、つまり各鳥について実際に処理しなければいけない変化は1回のみ。
計算量を考えると、各鳥の色の変化をN回、それをM日なので`O(N+M)`となり十分間に合いそうと考え実装しました。
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

  int N, M;
  cin >> N >> M;
  vector<tuple<int, int, int>> v(N);
  for (auto &[a, b, c] : v) {
    cin >> a >> b >> c;
  }

  sort(v.begin(), v.end(),
       [](const auto &x, const auto &y) { return get<1>(x) < get<1>(y); });

  vector<int> c(N + 1, 0);
  int k = 0;

  for (auto [A, D, B] : v) {
    if (c[A] == 0) {
      k++;
    }
    c[A]++;
  }

  int index = 0;
  for (int i = 1; i <= M; i++) {
    while (index < N && get<1>(v[index]) == i) {
      auto [A, D, B] = v[index];
      c[A]--;
      if (c[A] == 0) {
        k--;
      }

      if (c[B] == 0) {
        k++;
      }
      c[B]++;
      index++;
    }
    cout << k << endl;
  }

  return 0;
}

```


# D - Celester
この問題は[[動的計画法(DP)]]を使って解くことが出来ます。
なぜDPと使うと考えたかというと、
- 直前の天気が晴れなのか雨なのかの2種類で次の状態を決められること
- 直前の天気が同じものを複数残す必要性がないこと
の2点です。
例えば、3日目までで
```
SSR: 嬉しさ10
SRR: 嬉しさ20
RRR: 嬉しさ15
```
という3通りがあったとします。
ここで、4日目が`S`, `R`どちらの場合でもあっても次の嬉しさに影響するのは3日目がRだったという事実のみです。
また、この問題で求めなければならないものは達成可能な嬉しさの最大値であるため、
- `i-1`日目が雨だったときの嬉しさの最大値
- `i-1`日目が晴れだったときの嬉しさの最大値
この二つの状態を持つだけで、`i`日目が`S`, `R`だったとしても`i`日目の嬉しさの最大値を求めることができます。

DPを実装をする時の考え方も残しておきます。
まず、先ほどの考え方よりDPの定義は
```
dpS = i日目を晴れにしたときの嬉しさの最大値
dpR = i日目を雨にしたときの嬉しさの最大値
```
1日目は特殊な処理が必要です。
## 元の天気が`S`の場合
```cpp
dpS = 0; // 1日目をSにする = 変更なし = コストなし
dpR = -X[0]; // 1日目をRにする = 変更あり = X_1のコストがかかる
```
## 元の天気が`R`の場合
```cpp
dpS = 0; // 1日目をSにする = 変更あり = X_1のコストがかかる
dpR = -X[0]; // 1日目をRにする = 変更なし = コストなし
```

2日目以降は次のように考えられます。
## `i`日目を晴れにする場合
### `i-1`日目が`S`だった場合
`S -> S`なので追加の嬉しさは発生しません。候補は`dpS`です。
### `i-1`日目が`R`だった場合
`R -> S`なので嬉しさ`Y_i-1`を獲得します。なので、候補は`dpR + Y[i-1]`です。

この2点を統合し、`nextS = max(dpS, dpR + Y[i-1])`となります。
また、`i`日目が元々`R`だった場合、変更コストがかかるので`nextS -= X[i]`とします。
## `i`日目を雨にする場合
`i-1`日目が`S`, `R`どちらの場合でも追加の嬉しさは発生しません。
従って、`nextR = max(dpS, dpR)`です。
また、`i`日目が元々`S`だった場合、変更コストがかかるので`nextR -= X[i]`とします。

提出したコードは
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

  int T;
  cin >> T;

  while (T--) {
    int N;
    string S;
    cin >> N >> S;

    vector<ll> X(N);
    vector<ll> Y(N - 1);

    for (ll &x : X)
      cin >> x;
    for (ll &y : Y)
      cin >> y;

    auto cs = [&](int i) { return S[i] == 'S' ? 0LL : X[i]; };
    auto cr = [&](int i) { return S[i] == 'R' ? 0LL : X[i]; };

    ll dp_s = -cs(0);
    ll dp_r = -cr(0);

    for (int i = 1; i < N; i++) {
      ll ns = max(dp_s, dp_r + Y[i - 1]) - cs(i);
      ll nr = max(dp_s, dp_r) - cr(i);

      dp_s = ns;
      dp_r = nr;
    }

    cout << max(dp_s, dp_r) << endl;
  }

  return 0;
}
```
です。

# 感想
D問題を解くために色々なアルゴリズムを学習し、実際にD問題を解くことに成功して緑パフォを出すことが出来ました。
今年の夏の目標は入茶で、今年中に入緑したいです！