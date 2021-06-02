[問題へのLink (ABC173 [ProblemC])](https://atcoder.jp/contests/abc173/tasks/abc173_c)
### 費やした回答時間：約10分 ###
### Source Code ###
```
from itertools import product

H,W,K = map(int,input().split())
img = []
for i in range(H):
  img.append(list(input()))
  
bitH = list(product([1, 0], repeat=H))
bitW = list(product([1, 0], repeat=W))

def count_black(bH, bW):
  num = 0
  for i in range(H):
    for j in range(W):
      if bH[i] == 0 and bW[j] == 0:
        num += (img[i][j] == '#')
  return num

ans = 0
for i in bitH:
  for j in bitW:
    if count_black(i,j) == K:
      ans += 1
print(ans)
```

### Idea ###
行数、及び列数は高々6であるので、各行、各列が塗られるか否かの全探索をしても高々4000回程のループで済む(行数が6とすれば各行が塗られるか否かは2^(6)通りであり、各列に関しても同様なので、2^(6)×2^(6))。よってbit全探索を用いれば楽。

### Details ###
##### sample1 #####
```
2 3 2
..#
###
```

1. 入力処理
```
H,W,K = map(int,input().split())
img = []
for i in range(H):
  img.append(list(input()))

-----
H=2, W=3, K=2
img=[['.', '.', '#'], ['#', '#', '#']]
```
2. bit全探索用  
bitW[2] = (1, 0, 1)は1,3列目を塗ることを意味する。  
bitHとbitWの2重ループにより行、列の塗り方全てを網羅可能。
```
bitH = list(product([1, 0], repeat=H))
bitW = list(product([1, 0], repeat=W))

-----
bitH=[(1, 1), (1, 0), (0, 1), (0, 0)]
bitW=[(1, 1, 1), (1, 1, 0), (1, 0, 1), (1, 0, 0), (0, 1, 1), (0, 1, 0), (0, 0, 1), (0, 0, 0)]
```

3. カウント  
bitHとbitWの2重ループを行い、各場合における残った黒マスの数を関数count_blackが返す。  
その値がKと等しい場合にansをインクリメントし、最終結果を表示する。
```
def count_black(bH, bW):
  num = 0
  for i in range(H):
    for j in range(W):
      if bH[i] == 0 and bW[j] == 0:
        num += (img[i][j] == '#')
  return num

ans = 0
for i in bitH:
  for j in bitW:
    if count_black(i,j) == K:
      ans += 1
print(ans)
```

### Impression ###
行と列それぞれの全探索をすればいけるというのはすぐに分かった。  
しかし、bit全探索の利用経験が無ければ意外と実装に苦戦していたかもしれない。  
積み重ねが大事。
