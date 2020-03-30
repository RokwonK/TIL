# DP(Bottom-up) - 역추적

Bottom-up 방식으로 구현
1. 만들어 놓은 매트릭스(dp 매트릭스) 외 각 본연의 값을 저장해 놓는 매트릭스를 만든다.
2. 첫 시작 값부터 시작해서 최대값에서 현재의 값을 빼면서 접근!

```cpp
#include<iostream>
#include<string.h>
#include<vector>

using namespace std;

int N, M;
int arr[301][21];
//dp
int dp[301][21];

//본연의 값을 저장해 놓을 배열
int invest_company_money[301][21];

int solve(int remain_money, int company_num){

    if (company_num > M) 
        return 0;

    int &value = dp[remain_money][company_num];

    if (value != -1)
        return value;

    value = 0;
    for(int i = 0; i <= remain_money; i++) {
        int money = solve(remain_money-i, company_num+1)+arr[i][company_num];

        //그냥 min값으로 넣지 말고 if문으로 비교하여 값 뿐만 아니라 본연의 값도 저장
        if (money > value) {
            value = money;
            invest_company_money[remain_money][company_num] = i;
        }
    }

    return value;
}

int main(void) {

    ios_base :: sync_with_stdio(0);
    cin.tie(0);

    memset(dp, -1, sizeof(dp));

    cin >> N >> M;
    int a;

    
    for (int i = 1; i <= N; i++) {
        cin >> a;
        for (int j = 1; j <= M; j++)
            cin >> arr[a][j];
    }
    solve(N,1);

    // 시작인 N, 1에서 시작하여
    // 그 값을 뺀 배열에 접근...
    // 반복하여 접근한다.
    for (int i = 1; i <= M; i++) {
        cout << invest_company_money[N][i] << ' ';
        N -= invest_company_money[N][i];
    }

    return 0;
}
```

