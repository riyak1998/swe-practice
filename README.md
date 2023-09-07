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

## 2. Left rotation game

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

## 3. Largest number after concatenation 

```cpp

//for int
bool myCompare(int x,int y){
    int t1=x,t2=y;
    
    int countx=0,county=0;
    while(x>0){
        countx++;
        x/=10;
    }
    while(y>0){
        county++;
        y/=10;
    }
    
    int xy=t1,yx=t2;
    //generate xy
    while(county--){
        xy*=10;
    }
    xy+=t2;
    
    //generate yx
    while(countx--){
        yx*=10;
    }
    yx+=t1;
    
    return xy>yx;
}

///for strings
static bool myCompare(int &a,int &b){
    string x = to_string(a)+to_string(b);
    string y = to_string(b)+to_string(a);
    
    return x>y;
}

string Solution::largestNumber(const vector<int> &A) {
    if(A.size()==0)
        return "0";
        
    int cnt=0;

    for(int i=0;i<A.size();i++){
        if(A[i]==0){
            cnt++;
        }
    }

    if(cnt==A.size()){
        return "0";
    }
    
    vector<int> arr=A;
    sort(arr.begin(),arr.end(),myCompare);
        
    string ans;
    for(int x:arr){
        ans+=to_string(x);
    }
    return ans;
}
```

## 4. Left shifts, then right shifts

```cpp
#include <bits/stdc++.h>

using namespace std;

void reverse(string &s, int i, int j){
    while(i<j){
        swap(s[i],s[j]);
        i++,j--;
    }
}

void rotations(string &s, int &leftShift, int &rightShift){
    int l=s.length();
    leftShift=leftShift %l;
    
    reverse(s,0,leftShift-1);
    reverse(s,leftShift,l-1);
    reverse(s,0,l-1);
    
    //convert rightshift in terms of left shift to use the same algo for rotations
    rightShift=(rightShift%l);
    rightShift=(l-rightShift)%l;
    
    reverse(s,0,rightShift-1);
    reverse(s,rightShift,l-1);
    reverse(s,0,l-1);
    
}

int main() {
    string s="abcd";
    int leftShift=1;
    int rightShift=2;
    rotations(s, leftShift, rightShift);
    cout<<s;
    return 0;
}
```

## 5. Repeating string finder


