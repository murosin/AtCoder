[問題へのLink (ABC196 [ProblemC])](https://atcoder.jp/contests/abc196/tasks/abc196_c)
### 費やした回答時間：約13分 ###
### Source Code ###
```
N = input()

if int(N) < 10:
  print(0)
  exit()

if len(N) % 2 != 0:
  print(pow(10, int(len(N)/2)) - 1)
  exit()

first, second = int(N[:int(len(N)/2)]), int(N[int(len(N)/2):])
if first <= second:
  print(first)
else:
  print(first - 1)
```

### Idea ###
以下の2つの場合では簡単に答えが出せる。
- Nが1桁の場合

N以下でかつ桁数が偶数となる整数は作れないので、0。
- Nの桁数が奇数の場合

例えばNが5桁の場合、3桁以上の繰り返し(123123等)は作れず、2桁までの数字の繰り返し(11～9999)ならば全て作ることが出来る。

少し考える必要があるのは以下の場合である。
- Nの桁数が偶数の場合

例えばN=1312のとき、1～12までの数字の繰り返し(11～1212)を作ることが出来るが、13の繰り返し(1313)を作ることは出来ない。  
それに対しN=1314のとき、1～13までの数字の繰り返し(11～1313)を作ることが出来る。  
よってNの桁数が偶数2nの場合は、(前半n桁の値)と(後半n桁の値)に分け、(前半n桁の値)<=(後半n桁の値)ならば1～「(前半n桁の値)」、(前半n桁の値)>(後半n桁の値)ならば1～「(前半n桁の値)-1」まで
の繰り返しが可能だと分かる。
### Details ###
##### sample1 #####
```
33
```

1. 入力処理
```
N = input()

-----
N=33
```
2. Nが1桁の場合と桁数が奇数の場合の処理
```
if int(N) < 10:
  print(0)
  exit()

if len(N) % 2 != 0:
  print(pow(10, int(len(N)/2)) - 1)
  exit()
```
3. Nの桁数が偶数の場合の処理
```
first, second = int(N[:int(len(N)/2)]), int(N[int(len(N)/2):])
if first <= second:
  print(first)
else:
  print(first - 1)
```

### Impression ###
解説を見たところ、Nの上限が10^12くらいで、単純に高々10^6回のループで繰り返しの数字が作れるか判定するというやり方でも大丈夫だったぽい。


