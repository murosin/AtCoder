[問題へのLink (ABC206 [ProblemD])](https://atcoder.jp/contests/abc206/tasks/abc206_d)
### 費やした回答時間：約30分 (2021/06/19) ###
### Source Code ###
```
N = int(input())
A = list(map(int,input().split()))
 
change = [0]*2*(10**5)
for i in range(2*(10**5)):
  change[i] = i+1
  
def dfs(a):
  temp = 0
  renew_list = []
  while True:
    temp = change[a-1]
    if temp == a:
      for i in renew_list:
        change[i-1] = a 
      return temp
    else:
      renew_list.append(a)
      a = temp
ans = 0      
left, right = 0, N-1  
for i in range(int(N/2)):
  lt, rt = dfs(A[left]), dfs(A[right])
  if lt != rt:
    change[lt-1] = A[right]
    ans += 1
  left += 1
  right -= 1
print(ans)
```

### Idea ###
基本的に左右から順に見ていって、違う数字があれば一方をもう一方に変える。各数字の変換先を表すリストchangeを用意し、デフォルトでは[1,2,3,4,5,..]等、change[i]=i+1となるように設定する。
例えば[1 5 3 2 5 2 3 1]を扱うとき、まず1,1をみて一緒なのでスルー。次に5と3をみて5を3に変換するためにchange[5-1]=3とする。次に3と2を見てchange[3-1]=2とする。次に2と5を見るが、ここで
最初見落としていたのは、change[2-1]=2とchange[5-1]=3との比較で2-->3に変換に変換するのでは無く、change[5-1]=3,change[3-1]=2より2,2での比較を行わなければならない点。よって2つの各数字に対して
深さ優先探索(?)で最終的に得られる値同士で比較する。
### Details ###
##### sample1 #####
```
8
1 5 3 2 5 2 3 1
```

1. 入力処理
```
N = int(input())
A = list(map(int,input().split()))

-----
N=8
A=[1, 5, 3, 2, 5, 2, 3, 1]
```
2. 変換の対応付けのためのリストchangeを用意  

```
change = [0]*2*(10**5)
for i in range(2*(10**5)):
  change[i] = i+1
 
```

3. dfs(?)の実装  
このとき、計算時間短縮のために逐一更新を行う。
```
def dfs(a):
  temp = 0
  renew_list = []
  while True:
    temp = change[a-1]
    if temp == a:
      for i in renew_list:
        change[i-1] = a 
      return temp
    else:
      renew_list.append(a)
      a = temp
```

4. カウント
```
ans = 0
left, right = 0, N-1  
for i in range(int(N/2)):
  lt, rt = dfs(A[left]), dfs(A[right])
  if lt != rt:
    change[lt-1] = A[right]
    ans += 1
  left += 1
  right -= 1
print(ans)
```

### Impression ###
C問題よりも速く解けた。  
Dの中でも相当簡単な部類だったと思うけど、初めて時間内にD問題が解けてうれしいv(^^)
