[問題へのLink (ABC173 [ProblemD])](https://atcoder.jp/contests/abc173/tasks/abc173_d)
### 費やした回答時間：約50分 ###
### Source Code ###
```
N = int(input())
A = list(map(int, input().split()))
A.sort(reverse=True)
if N % 2 == 0:
  print(A[0] + 2 * sum(A[1:int(N/2)]))
else:
  print(sum(A[:int((N+1)/2)]) + sum(A[1:int((N+1)/2)-1]))
```

### Idea ###
例として心地よさがそれぞれ1, 2, 3, 4, 5, 6である6人(p1, p2, p3, p4, p5, p6)がいたとする。  
はじめはp6を到着させ、その後、時計周りに順にp5,p4,...,p1を配置すれば良いと考えた(その場合はmin(6,4)+min(5,3)+min(4,2)+min(3,1)+min(2,6)となる)。  
しかし、最小値をとる際に差があるのがもったいなく感じた(当然min(6,4)<=min(6,5)が成り立つので)。  
そこで、以下のような貪欲的なstepを考えた。
- step1. p6を到着させる。
- step2. p5をp6の隣に到着させる。(p5の心地よさは6)
- step3. p4をp5とp6の隣に到着させる。(p4の心地よさはmin(5,6)=5)
- step4. p3をp5とp6の隣(A4とは別の位置)に到着させる。(p3の心地よさはmin(5,6)=5)
- step5. p2をp4とp6の隣に到着させる。(p2の心地よさはmin(4,6)=4)
- step6. p1をp4とp5の隣に到着させる。(p1の心地よさはmin(4,5)=4)  


以上より次の図のような配置が理想であると考えた。

<img src="images/ABC173_D1.png" width="150">

また、仮にこれ以上心地よさが1のプレイヤーが続くとしても、A3の両端->A2の両端という順番で配置させていけば3,3,2,2がそれぞれ得られるので、以下のように一般化できる。  
プレイヤーの人数がN人であり、
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
