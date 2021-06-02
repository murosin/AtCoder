[問題へのLink (ABC174 [Problem])](https://atcoder.jp/contests/abc174/tasks/abc174_d)
### 費やした回答時間：約25分 ###
### Source Code ###
```
N = int(input())
S = list(input())

leftIndex, rightIndex = 0, N - 1
ans = 0
while True:
  for i in range(leftIndex, N):
    if i >= rightIndex:
      print(ans)
      exit()
    if S[i] == 'W':
      leftIndex = i
      break
      
  for j in range(rightIndex, -1, -1):
    if j <= leftIndex:
      print(ans)
      exit()
    if S[j] == 'R':
      rightIndex = j
      break
  leftIndex += 1
  rightIndex -= 1
  ans += 1
```

### Idea ###
まず、2種類の操作の内、石の色を変える操作については、目的を満たす最小操作数を求める上ではあってもなくても変わらないことが分かる。  
例えばWRRWWRという並びがあったとして、「3つのRをWに変える」(=操作数3)、「一番左のWをR、一番右のRをWに変える」(=操作数2)という処理を行うよりも、「一番左のWと一番右のRとを入れ替える」(=操作数1)方が良い。  
今回の問題は「あるindexを境として左(Rのみ)と右(Wのみ)に分ける」ための最小操作数を求めるわけだが、色を変えるというのはあくまで「あるindexより左にあるWをRに変える」または「あるindexより右にあるRをWに変える」ための操作を指し、
それらは最小操作数を考える上で、「あるindexより左にあるWと右にあるRとを入れ替える」というより効率の良い操作で包含される。  
よって今回は「あるindexより左にあるWと右にあるRとを入れ替える」ことを繰り返すことで最小操作数が得られると言うことになる。  
以上より、左から順に'W'(index = l)を、右から順に'R'(index = r)を探し、l>=rとなるまでの操作数をカウントすれば良い。
### Details ###
##### sample1 #####
```
4
WWRR
```

1. 入力処理
```
N = int(input())
S = list(input())

-----
N=4
S=['W', 'W', 'R', 'R']
```
2. ループ処理  
以上で述べた考え方を元にループ処理を行う。  
はじめにleftIndex = 0, rightIndex = N-1とし、左から順に'W'を見つけるまでleftIndexをインクリメント、右から順に'R'を見つけるまでrightIndexをデクリメントさせる。leftIndex < rightIndexであるうちに、交換させるべき組が見つかる度にansをインクリメントし、leftIndex >= rightIndexとなれば終了する。
```
leftIndex, rightIndex = 0, N - 1
ans = 0
while True:
  for i in range(leftIndex, N):
    if i >= rightIndex:
      print(ans)
      exit()
    if S[i] == 'W':
      leftIndex = i
      break
      
  for j in range(rightIndex, -1, -1):
    if j <= leftIndex:
      print(ans)
      exit()
    if S[j] == 'R':
      rightIndex = j
      break
  leftIndex += 1
  rightIndex -= 1
  ans += 1
```

### Impression ###
つい最近似たような問題を見たせいか、解法自体はすぐに浮かんだ。  
左右両端から順に探す処理で若干もたついてしまった。
