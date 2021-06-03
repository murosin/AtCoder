[問題へのLink (ABC195 [ProblemD])](https://atcoder.jp/contests/abc195/tasks/abc195_d)
### 費やした回答時間：約37分 (2021/06/03) ###
### Source Code ###
```
N, M, Q = map(int,input().split())
pac = []
for n in range(N):
  w, v = map(int,input().split())
  pac.append([w, v])
pac = sorted(pac, key=lambda x: x[1], reverse=True)
X = list(map(int,input().split()))

def allot(X_q):
  ans = 0
  for w, v in pac:
    temp = sum([1 for x in X_q if x >= w])
    if temp == 0:
      continue
    else:
      X_q.pop(temp-1)
      ans += v
  return ans

for q in range(Q):
  L, R = map(int, input().split())
  X_q = X[:L-1]
  X_q.extend(X[R:len(X)])
  X_q = sorted(X_q, reverse=True)
  print(allot(X_q))
```

### Idea ###
荷物を価値が高い順にsortし、順番に入る箱がないかを探す。  
このとき、入る箱なら何でも良いわけでは無く、その荷物を入れることが出来る最小の箱に入れる必要がある。  
なぜなら、例えば(大きさ、価値)がそれぞれ(2,5),(3,4)である荷物1,2があり、箱1,箱2のサイズがそれぞれ2,4だった場合、
荷物1を箱2にいれると荷物2が入る箱が無い。それに対し荷物1が入る最小の箱1に入れれば荷物2も箱2に入れることが出来る。よって、最適化するにはその荷物が入る最小の箱に入れる必要がある。  
ちなみに価値の高い順に箱を貪欲的に求めても問題ないと考えたのは、箱に入る荷物が1個だけと決まっているからである。  
(大きさ、価値)がそれぞれ(5,5),(4,4),(3,3)である荷物1,2,3と大きさが7である箱が1つだけあり、かつ1つの箱に複数の荷物が入れられるなら
価値5の荷物では無く価値4,3の荷物2,3を入れた方が良いが、今回のケースではそのようなことが起こらない。  


### Details ###
##### sample1 #####
```
3 4 3
1 9
5 3
7 8
1 8 6 9
4 4
1 4
1 3
```

1. 入力処理(その1)  
入力処理、及び貪欲的に求めるために価値をkeyとして荷物をsortしている。
```
N, M, Q = map(int,input().split())
pac = []
for n in range(N):
  w, v = map(int,input().split())
  pac.append([w, v])
pac = sorted(pac, key=lambda x: x[1], reverse=True)
X = list(map(int,input().split()))

-----
N=3
M=4
Q=3
pac=[[1, 9], [7, 8], [5, 3]]
X=[1, 8, 6, 9]
```
2. 入力処理(その2)＆貪欲法  

貪欲法適用のために、クエリ毎に使用できる箱を、サイズをkeyとしてsortしている。  
関数allot内では、サイズwを入れることが出来る箱の中があれば、その中の最小の箱をpopし、価値をansに加算する。これを全ての荷物に対して繰り返すことで、そのクエリにおける答えが分かる。
```
def allot(X_q):
  ans = 0
  for w, v in pac:
    temp = sum([1 for x in X_q if x >= w])
    if temp == 0:
      continue
    else:
      X_q.pop(temp-1)
      ans += v
  return ans

for q in range(Q):
  L, R = map(int, input().split())
  X_q = X[:L-1]
  X_q.extend(X[R:len(X)])
  X_q = sorted(X_q, reverse=True)
  print(allot(X_q))
```

### Impression ###
箱1個につき荷物1個までという設定のおかげで割と楽に考えられた。
