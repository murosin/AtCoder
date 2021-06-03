[問題へのLink (ABC182 [ProblemD])](https://atcoder.jp/contests/abc182/tasks/abc182_d)
### 費やした回答時間：約15分 (2021/06/03) ###
### Source Code ###
```
N = int(input())
A = list(map(int,input().split()))

rel_max, rel_end, position = A[0], A[0], A[0]
ans = max(0, rel_max)

for i in range(1, N):
  rel_max = max(rel_max, rel_end + A[i])
  rel_end += A[i]
  ans = max(ans, position + rel_max)
  position += rel_end
print(ans)
```
### Idea ###
A1, A2,...,Aiの移動を行う動作を動作iとする。全ての動作i(1<=i<=N)の移動を愚直に計算していては間に合わない。  
そこで、最低限の情報を保持しつつ、各動作について情報を更新していく。保持しておくべき情報は以下の3つ。
- 動作iを始めた地点に対する動作iの過程で移動できる相対距離の最大値(**rel_max**)。  
(例えば、動作i=[2,-4,1]だった場合、動作iを始めた地点(start)に対して
動作iの過程で移動できる場所はそれぞれ[start+2, start-2, start-1]であるため、相対距離はそれぞれ[2,-2,-1]であると分かる。よって相対距離の最大値は2となる。
- 動作iを始めた地点に対する動作i終了地点の相対距離(**rel_end**)。  
(例えば、動作i=[2,-4,1]だった場合、動作iを始めた地点(start)に対して動作iの過程で移動できる場所はそれぞれ[start+2, start-2, start-1]であるため、
始めた地点に対する終了地点の相対距離は-1となる。
- 動作i終了時のロボットの位置(**position**)。  
(例えば現時点でロボットが10の位置にいて動作i=[2,-4,1]を行った場合、(動作iを始めた地点+rel_end =)9となる。

動作iに対する具体的な情報の更新は以下のように考えられる。なお、それぞれの初期値は当然rel_max, rel_end, position = A1, A1, A1である。
- **rel_max** --- 今のrel_maxと、rel_end+Aiとを比較して大きい方に更新すればよい。なぜなら、A1～Ai-1まででの相対距離の最大値が今のrel_maxで確定なので、
変更の有無はrel_end+Aiの値次第で変わる。
- **rel_end** --- 今のrel_endの値にAiの値を足せばよい。なぜなら、今のrel_endがA1,A2,...,Ai-1に対する終了地点の相対距離であるので、これにAiを足すだけでよい。
- **position** --- 今のpositionの値に更新後のrel_endを足すだけでよい。

そして、求める答えは各動作iに対して(更新前の**position**の値+更新後の**rel_max**)を求め、それらの最大値を出力すればよい。

### Details ###
##### sample1 #####
```
3
2 -1 -2
```

1. 入力処理
```
N = int(input())
A = list(map(int,input().split()))

-----
N=3
A=[2, -1, -2]
```
2. 上記の情報の更新、及び答えの出力。
```
rel_max, rel_end, position = A[0], A[0], A[0]
ans = max(0, rel_max)

for i in range(1, N):
  rel_max = max(rel_max, rel_end + A[i])
  rel_end += A[i]
  ans = max(ans, position + rel_max)
  position += rel_end
print(ans)
```

### Impression ###
これに関しては簡単なサンプルでメモしながら考えたらすぐに一般化ができ、実装までたどり着いた。  
やはりいくつかのサンプルから一般化するのが大事。
