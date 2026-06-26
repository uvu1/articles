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
なんとなく参加したんですけど楽しくて毎週やると思うので記録つけていきます
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

# B - 