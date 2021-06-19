[問題へのLink (ABC206 [ProblemC])](https://atcoder.jp/contests/abc206/tasks/abc206_c)
### 費やした回答時間：約40分 (2021/06/19) ###
### Source Code ###
```
N=int(input())
A = list(map(int,input().split()))
A = sorted(A)
 
import math
def cmb(n, r):
  if n <= 2:
    return n-1
  return math.factorial(n) // (math.factorial(n - r) * math.factorial(r))
 
ans = 0
before = -1
kaburi = 0
for i in range(N):
  if A[i] == before:
    kaburi += 1
  else:
    before = A[i]
    ans -= cmb(kaburi+1, 2)
    kaburi = 0 
  ans += (N-i-1)
if kaburi > 0:
  ans -= cmb(kaburi+1, 2)
print(ans)
```

### Idea ###
仮に配列Aの内容が昇順でかつ全て異なる(例えばA=[1,2,3,4,5])とき、「A[0]=1とそれ以降の4つとの組、A[1]=2とそれ以降の3つとの組,...,A[3]=4とそれ以降の1つ(A[4]=5)との組」という流れで
組が出来るため、求める答えは4+3+2+1=10となる。しかし、数字が共通の組の分を引かなければならないので、逐一共通な数字が無いかチェックし、あればその数同士の組み合わせの分を引かなければならない。
### Details ###
##### sample1 #####
```
3
1 7 1
```

1. 入力、及びリストAのsort処理
```
N=int(input())
A = list(map(int,input().split()))
A = sorted(A)

-----
N=3
A=[1, 1, 7]
```
2. 組み合わせの数の関数  
数字が共通である組数を計算するため
```
import math
def cmb(n, r):
  if n <= 2:
    return n-1
  return math.factorial(n) // (math.factorial(n - r) * math.factorial(r))
 
```

3. カウント部分  
答えはansに格納。beforeは一つ前(A[i-1])の値が格納されていて、A[i]とbeforeが一致するたびに
共通な数字の数を表すkaburiをインクリメントする。共通な数字の羅列が終わる度、引くべき組数を関数cmbにより決定し、
ansから引く。
```
ans = 0
before = -1
kaburi = 0
for i in range(N):
  if A[i] == before:
    kaburi += 1
  else:
    before = A[i]
    ans -= cmb(kaburi+1, 2)
    kaburi = 0 
  ans += (N-i-1)
if kaburi > 0:
  ans -= cmb(kaburi+1, 2)
print(ans)
```

### Impression ###
C問題なのになかなか解けずに焦ってしまった。多分もっと簡単
な方法で解けるだろうなぁ
