
## **로직 설명**
-백트래킹할시에는 파이썬이 상당히 불리한 언어라고 한다 (pypy3로 제출하지 않으면 통과되지 않는다)
-queen은 체스에서 상하좌우 대각선 방향 모두 이동가능한 기물
-queen을 n개 놓기 위해서는 n개의 행에 무조건 한개씩 있어야 하는 기본 가정으로 가로는 검사하지 않고 내려간다
-다른 공격 방향에 다른 queen의 유무를 검사하기 위해 세로 방향 n개의 False로 초기화 된 배열, '/'과 '\' 대각선 모양의 2n-1개의 False로 초기화된 배열을 생성해줍니다
-i(행)가 n이 되면 모든 행에 한개씩 퀸을 놓았다는 뜻이므로 ans에 1을 추가해주고 return
-'/' 모양을 검사하기 위해서는 x+y의 값이 같으면 같은 대각선상에 있다는 얘기이다
-'\' 모양을 검사하기 위해서는 x-y의 값이 같으면 같은 대각선상에 있다는 얘기이다 하지만 음수가 되서는 안되기 때문에 n-1을 더해줍니다
-한번 방문한 곳을 True로 바꾼 후에 dfs 재귀를 호출하고 다시 False로 바꾸어 가지치기를 하고 다시 탐색 할 수 있게끔 합니다

```python
import sys

sys.setrecursionlimit(10000)

n = int(sys.stdin.readline())
ans = 0

a = [False] * n # | 세로
b = [False] * (2*n-1)  # '/' 모양, x+y
c = [False] * (2*n-1)  # '\' 모양  x-y+n-1


def dfs(i):  # 열
    global ans
    if i == n:
        ans += 1
        return
    for j in range(n): # 행
        if a[j] == False and b[i+j] == False and c[i-j+n-1] == False:
            a[j] = b[i+j] = c[i-j+n-1] = True
            dfs(i+1)
            a[j] = b[i+j] = c[i-j+n-1] = False


dfs(0)
print(ans)









