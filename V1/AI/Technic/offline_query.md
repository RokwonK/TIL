# Offline Query (오프라인 쿼리)

쿼리 즉, 문제 자체에서 질문을 하면 그 상황에서의 답을 내야할 때가 있다.  
그렇다면 쿼리순서대로 답을 내야하는데 그럴경우 놀라운 시간복잡도가 나올 수 있다. 

예를 들어, 어느 그래프가 있다고하자

1. 1을 입력하면 다음 입력되는 정점이 도달할 수 있는 정점을(사이클이 있다면 있다고 표시)
2. 2를 입력하면 다음 입력되는 정점으로 가는 길을 끊는다

3. 2가 입력될때마다 그래프의 간선을 끊으므로 다음 1이 입력될때 그래프를 다시 확인해봐야한다  

4. 2와 1이 번갈아 가며 입력될시 그래프를 거의 N번 확인해야하는 꼴이 되므로 놀라운 시간복잡도가 나온다.

이때 <span style="font-size:20px">**오프라인 쿼리**</span>를 사용한다.  


쿼리를 미리 다 받아서 마지막 쿼리가 끝났을 때의 그래프를 만들어 놓고
쿼리를 꺼꾸로 확인한다(마지막 쿼리부터 확인)  

반대로 생각하는 것이다!  

2가 입력되면 끊는 것이 아닌 이어지게 만드는 것 (거꾸로 니까!)  
(위와 같은 문제는 **유니온 파인드**로 정점마다의 가장 끝 정점을 저장해 놓으면 되는 것! )

그렇다면 놀랍게도 그래프는 마지막 쿼리가 끝났을 때의 모습에서 간선만 추가하면 되므로 시간복잡도를 대폭 줄일 수 있다!  
<div style="color:red; font-weight:bold">
말 그대로 오프라인 쿼리는 쿼리를 역순으로 생각한다는 뜻!</br>
</div>

- Code
```cpp
#include<iostream>
#include<vector>

using namespace std;

int union_find[300002];
pair<int,int> query[300002];
int arr[300002];
bool can_not_use[300002];

int find(int x) {
    if (union_find[x] == x)
        return x;
    
    return union_find[x] = find(union_find[x]);
}

void merge(int x, int y) {
    x = find(x);
    y = find(y);
    
    if (x != y)
        union_find[x] = y;
    else {
        // 싸이클이 있다
        union_find[x] = 0;
        union_find[y] = 0;
    }
    
}

int main(void) {
    
    ios_base :: sync_with_stdio(0);
    cin.tie(0);
    
    int N,M;
    cin >> N;
    
    vector<int> ans;
    
    for (int i = 1 ; i <= N; i++) {
        cin >> arr[i];
        union_find[i] = i;
    }
    
    
    cin >> M;
    // 쿼리르 받으면서 끊어놓을 곳을 can_not_use에 true로 만들어놓음
    for (int i = 0; i < M; i++) {
        int a, b;
        cin >> a >> b;
        
        query[i].first = a;
        query[i].second = b;
        
        
        if (a == 2) can_not_use[b] = true;
    }
    
    // 미리 끊어놓을 곳을 곳을 다 끝어서 만듬
    for (int i = 1; i <= N; i++)
        if ( can_not_use[i] == false && arr[i] != 0)
            merge(i, arr[i]);
    

    //쿼리를 뒤에서 부터 확인함
    // 2가 입려되는 곳에서는 이어줌!
    for (int i = M-1; i >= 0; i--) {
        int value = query[i].second;
        
        if (query[i].first == 1) {
            int part_ans = find(value);
            
            if (part_ans == 0)
                ans.push_back(0);
            else
                ans.push_back(part_ans);
        }
        else {
            if (arr[value] != 0)
                merge(value, arr[value]);
        }
            
    }
    
    // 답이 거꿀로 저장되었으므로 거꾸로 출력함
    for (int i = ans.size() - 1; i >= 0; i--) {
        if (ans[i] == 0)
            cout << "CIKLUS" << '\n';
        else
            cout << ans[i] << '\n';
    }
    
    return 0;
}

```