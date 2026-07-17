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