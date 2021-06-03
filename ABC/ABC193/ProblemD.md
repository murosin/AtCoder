[問題へのLink (ABC193 [ProblemD])](https://atcoder.jp/contests/abc193/tasks/abc193_d)
### 費やした回答時間：約40分 (2021/06/03) ###
### Source Code ###
```
import copy

K=int(input())
S=list(input())
T=list(input())
rem = [K] * 9
for i in range(9):
  rem[i] -= S.count(str(i+1)) + T.count(str(i+1))
sum_rem = sum(rem)

temp = [i for i in range(1,10)]
import itertools
Comb = list(itertools.product(temp, temp))

def score(Hand):
  score = 0
  for i in range(1,10):
    score += pow(10, Hand.count(str(i))) * i
  return score

def probability(c):
  if c.count(c[0]) > rem[int(c[0])-1] or c.count(c[1]) > rem[int(c[1])-1]:
    return 0
  copyS = copy.deepcopy(S)
  copyT = copy.deepcopy(T)
  copyS.append(str(c[0]))
  copyT.append(str(c[1]))
  if score(copyS) > score(copyT):
    return (((rem[int(c[0])-1]) * (rem[int(c[1])-1] - (c[0] == c[1])) / (sum_rem * (sum_rem - 1))))
  else:
    return 0
  
ans = 0
for c in Comb:
  ans += probability(c)
print(ans)
```
### Idea ###
割と安直にそれぞれあり得るパターンについて各々のスコアを算出し、条件を満たすとき、そのパターンになる確率を足すという流れで解答可能。  
例えばK=2で高橋君が始め「1144#」で、青木君が始め「2233#」だったとして、(高橋君の隠れたカード,青木君の隠れたカード) = (5,6)だったとすると、初期値ans=0として以下の流れになる。  
1. (5,6)となる組み合わせがあり得るか否かを判定する(例えば5が既に2枚出ている場合はあり得ないと判定できる)。  
2. 各々のスコアを判定する。
3. 高橋君のスコアが高かった場合、(5,6)が選ばれる確率をansに足す。

この流れを全ての組み合わせ(1,1),(1,2),...,(9,8),(9,9)に対して行えば良い。
### Details ###
##### sample1 #####
```
2
1144#
2233#
```

1. 入力処理
```
import copy

K=int(input())
S=list(input())
T=list(input())

-----
K=2
S=['1', '1', '4', '4', '#']
T=['2', '2', '3', '3', '#']
```
2. 各数字カードの残り枚数を記録
```
rem = [K] * 9
for i in range(9):
  rem[i] -= S.count(str(i+1)) + T.count(str(i+1))
sum_rem = sum(rem)

-----
rem=[0, 0, 0, 0, 2, 2, 2, 2, 2]
sum_rem=10
```

3. 全パターン列挙
```
temp = [i for i in range(1,10)]
import itertools
Comb = list(itertools.product(temp, temp))

-----
Comb=[(1, 1), (1, 2), (1, 3), (1, 4), (1, 5), (1, 6), (1, 7), (1, 8), (1, 9), (2, 1), (2, 2), (2, 3), (2, 4), (2, 5), (2, 6), (2, 7), (2, 8), (2, 9), (3, 1), (3, 2), (3, 3), (3, 4), (3, 5), (3, 6), (3, 7), (3, 8), (3, 9), (4, 1), (4, 2), (4, 3), (4, 4), (4, 5), (4, 6), (4, 7), (4, 8), (4, 9), (5, 1), (5, 2), (5, 3), (5, 4), (5, 5), (5, 6), (5, 7), (5, 8), (5, 9), (6, 1), (6, 2), (6, 3), (6, 4), (6, 5), (6, 6), (6, 7), (6, 8), (6, 9), (7, 1), (7, 2), (7, 3), (7, 4), (7, 5), (7, 6), (7, 7), (7, 8), (7, 9), (8, 1), (8, 2), (8, 3), (8, 4), (8, 5), (8, 6), (8, 7), (8, 8), (8, 9), (9, 1), (9, 2), (9, 3), (9, 4), (9, 5), (9, 6), (9, 7), (9, 8), (9, 9)]
```

4. 各パターンに対して判定を行い、確率の和を計算  
関数scoreは数字5枚の手札に対するスコアを計算するもので、関数probabilityは関数scoreを用いつつ、高橋君が勝つパターンとなる確率を返す。
それをCombで表現した全ての組み合わせに対して行い、最終的な確率を計算する。
```
def score(Hand):
  score = 0
  for i in range(1,10):
    score += pow(10, Hand.count(str(i))) * i
  return score

def probability(c):
  if c.count(c[0]) > rem[int(c[0])-1] or c.count(c[1]) > rem[int(c[1])-1]:
    return 0
  copyS = copy.deepcopy(S)
  copyT = copy.deepcopy(T)
  copyS.append(str(c[0]))
  copyT.append(str(c[1]))
  if score(copyS) > score(copyT):
    return (((rem[int(c[0])-1]) * (rem[int(c[1])-1] - (c[0] == c[1])) / (sum_rem * (sum_rem - 1))))
  else:
    return 0
  
ans = 0
for c in Comb:
  ans += probability(c)
print(ans)
```
### Impression ###
割と方針が立てやすい問題だったが、実装の際に細かい苦戦が多く、時間を多く費やしてしまった。

