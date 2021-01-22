## 문제

N-Queen 문제는 크기가 N × N인 체스판 위에 퀸 N개를 서로 공격할 수 없게 놓는 문제이다.

N이 주어졌을 때, 퀸을 놓는 방법의 수를 구하는 프로그램을 작성하시오.

## 입력

첫째 줄에 N이 주어진다. (1 ≤ N < 15)

## 출력

첫째 줄에 퀸 N개를 서로 공격할 수 없게 놓는 경우의 수를 출력한다.

## 예제 입력 1

```
8

```

## 예제 출력 1

```
92

```

## 

---

## 풀이 과정

- 퀸을 놓을 수 없는 위치일 경우에 백트랙킹하여 이전 퀸이 놓인 위치에서부터 다시 탐색해야하기 때문에 DFS 그래프 탐색을 사용하여 풀었습니다.
- (1,1) (1,2)~~~~(1,N) 중에 퀸을 놓은 위치를 시작점으로 하여 DFS 탐색을 합니다.
- (1,1) 에 놓았을 경우, 다음 2행 ? 열에 놓아야 괜찮을지를 알아야합니다.
- 일단 2행 1열에 놓은 상태로 여기 놔도 괜찮은지를 검사해줍니다.
- 2행 1열은 놓을 수 없는 곳이므로 2행 2열을 검사합니다.
    - 이 때, 2행 1열에 놓은 상태로 검사를 했으므로 다시 아무것도 놓여지지 않은 상태로 되돌려놔야 합니다.
- 2행 2열도 놓을 수 없으므로 2행 3열을 검사합니다 → 이번에는 놓을 수 있는 곳이므로 2행 3열에서 DFS함수를 재귀호출하여 다음행에서부터 다시 탐색을 시작합니다.
- 3행 1열에는 1행 1열에 놓인 퀸 때문에 놓을 수 없습니다.
- 이런식으로 쭉 과정을 반복하다 보면 N개의 퀸을 다 놓을 수 있는 경우를 찾게 됩니다.
- 경우를 찾게될 때마다 최종 결과를 1씩 증가시켜주면 됩니다.

- 퀸을 놓을 수 있는지 검사하는 bool possible 함수를 만들었습니다.
- 행방향과 열방향 그리고 대각선 방향을 검사해주면 됩니다.

```cpp
//여기 퀸을 놔도 괜찮을까?
bool possible(int row){
    for(int i=1;i<row;i++){
        if(arr[i]==arr[row]){
            return false;
        }
        if(abs(arr[i]-arr[row])==abs(i-row)){
            return false;
        }
    }
    return true;
}
```

---

## 전체 소스 코드

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <cmath>
#include <cstring>
#include <cstdio>
#include <stack>
#include <queue>
#include <map>
#include <functional>
using namespace std;
int n;
int answer = 0;
int arr[15];

//여기 퀸을 놔도 괜찮을까?
bool possible(int row){
    for(int i=1;i<row;i++){
        if(arr[i]==arr[row]){
            return false;
        }
        if(abs(arr[i]-arr[row])==abs(i-row)){
            return false;
        }
    }
    return true;
}

void dfs(int row){
    if(row==n){ //검사 끝난시점
        answer++;
    }else{
        for(int i=1;i<=n;i++){
            //8x8이면 1부터 8까지 검사
            arr[row+1] = i;//(2,1), (2,2), (2,3) , (2,4) ....중 한군데에 퀸 놓아보면서 루프 돌면서 일단 퀸을 놓아봄
            if(possible(row+1)==true){
                dfs(row+1); //만약 퀸 놓을 수 있는 곳이면 재귀
            }else{
                arr[row+1]=0;//퀸 경로에 잇으니깐 다시 되돌려
            }            
        }
    }
    arr[row]=0; //실패했으니깐 퀸 없애기

    
}
int main(){
    cin.tie(0);
    cout.tie(0);
    ios::sync_with_stdio(false);
    cin>>n;
    for(int i=1;i<=n;i++){
        arr[1] = i; 
        dfs(1);
        // 1행 1열에 퀸을 놓았을 경우
        // 1행 2열에 놓았을 경우~~~
    }
    cout<<answer;
    
    return 0;

}
```