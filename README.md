# dsa-practice

## 1. Maximize bankruptcy

```cpp
#include <iostream>
#include <vector>

//profit
int maximize_bankruptcy(const std::vector<int>& stock_prices) {
    if (stock_prices.size() <= 1) {
        return 0;
    }

    int max_profit = 0;
    int min_price = stock_prices[0];

    for (size_t i = 1; i < stock_prices.size(); ++i) {
        max_profit = std::max(max_profit, abs(stock_prices[i] - min_price));
        min_price = std::min(min_price, stock_prices[i]);
    }

    return max_profit;
}

int main() {
    std::vector<int> stock_prices = {3, 2, 4, 2, 1, 5};
    int result = maximize_bankruptcy(stock_prices);
    std::cout << "The largest profit possible: " << result << std::endl;

    return 0;
}
```

### 2. Left rotation game

```cpp
#include <bits/stdc++.h>

using namespace std;


vector<int> rotate(vector<int> &nums, vector<int> &r){
    int maxNum=INT_MIN,index=-1;
    for(int i=0;i<nums.size();i++){
        if(nums[i]>maxNum){
            maxNum=nums[i];
            index=i;
        }
    }
    vector<int> ans(r.size(),-1);
    for(int i=0;i<r.size();i++){
        int newIndex=(index-(r[i]%nums.size()));
        ans[i]=(newIndex>=0?newIndex:(nums.size()+newIndex));
    }
    return ans;
}

int main() {
    vector<int> nums={1,2,3,4};
    vector<int> r={1,2,4};
    vector<int> ans=rotate(nums,r);
    for(int i;i<ans.size();i++){
        cout<<r[i]<<"::"<<ans[i]<<endl;
    }
    return 0;
}
```
