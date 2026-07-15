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

つまり、上下左右を二重ループで全探索し、