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
[tex: {行数、及び列数は高々6であるので、各行、各列が塗られるか否かの全探索をしても高々4000回程のループで済む(行数が6とすれば各行が塗られるか否かは2^(6)通り
であり、各列に関しても同様なので、2^(6)\times 2^(6))。よってbit全探索を用いれば楽。} ]
$$
\begin{eqnarray*}
4a &=& ((a+a)+a)+a \\
   &=& (a+a)+(a+a)
\end{eqnarray*}
$$
