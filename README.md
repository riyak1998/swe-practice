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

```cpp
#include <stdio.h>
#include <string.h>

void shift_string(char *str, int left_shifts, int right_shifts) {
    int k = right_shifts - left_shifts;
    int len = strlen(str);
    char temp[len];

    if (k > 0) {
        // Perform right shift
        for (int i = 0; i < len; i++) {
            temp[(i + k) % len] = str[i];
        }
    } else {
        // Perform left shift
        for (int i = 0; i < len; i++) {
            temp[(i + len + k) % len] = str[i];
        }
    }

    // Copy the shifted string back to the original string
    for (int i = 0; i < len; i++) {
        str[i] = temp[i];
    }
}

int main() {
    char str[] = "abcd";
    int left_shifts = 1;
    int right_shifts = 2;

    printf("Original string: %s\n", str);
    shift_string(str, left_shifts, right_shifts);
    printf("Shifted string: %s\n", str);

    return 0;
}
```

## 5. Repeating string finder

## 6. Search pattern occurences

```cpp
vector<int> search(string B, string A)
        {
            //code hee.
            int lenA=A.length(),lenB=B.length();
            int index=-1;
            vector<int> ans;
            for(int i=0;i<lenA;){
                int j=0;
                if(A[i]==B[j]){
                    int temp=i;
                    while(j<lenB && A[i]==B[j]){
                        i++;
                        j++;
                    }
                    if(j==lenB){
                        index=temp;
                        ans.push_back(index+1);
                    }
                    i=temp+1;
                }
                else{
                    i++;
                }
            }
            if(ans.size()==0)
                return {-1};
            return ans;
        }
```
#### KMP-algo

```cpp
```

## 7. LCS (memoization, tabulation)

```cpp
class Solution
{
    public:
    
    //Function to find the length of the longest common subsequence in two strings.
    int func(string &s1, string &s2, int &n, int &m, int i, int j,vector<vector<int>> &memo){
        if(i==n || j==m){
            return 0;
        }
        if(memo[i][j]!=-1){
            return memo[i][j];
        }
        if(s1[i]==s2[j]){
                return memo[i][j]=1+func(s1,s2,n,m,i+1,j+1,memo);
        }
        return memo[i][j]=max(func(s1,s2,n,m,i+1,j,memo),func(s1,s2,n,m,i,j+1,memo));
    }
    
    int lcs(int n, int m, string s1, string s2)
    {
        // your code here
        vector<vector<int>> memo(n+1,vector<int> (m+1,-1));
        return func(s1,s2,n,m,0,0,memo);
    }
};
```
```cpp
class Solution
{
    public:
    int lcs(int n, int m, string s1, string s2)
    {
        // your code here
        vector<vector<int>> dp(n+1,vector<int> (m+1,0));
        dp[0][0]=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(s1[i-1]==s2[j-1]){
                    dp[i][j]=1+dp[i-1][j-1];
                }
                else dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }
        return dp[n][m];
    }
};
```
## 8. LCS gives the lcs string as well

```cpp
class Solution
{
    public:
    int lcs(int n, int m, string s1, string s2)
    {
        // your code here
        string lcss;
        vector<vector<string>> dp(n+1,vector<string> (m+1,""));
        vector<vector<int>> dp1(n+1,vector<int> (m+1,0));
        dp[0][0]="";
        dp1[0][0]=0;
        for(int i=1;i<=n;i++){
            for(int j=1;j<=m;j++){
                if(s1[i-1]==s2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+s1[i-1];
                    dp1[i][j]=dp1[i-1][j-1]+1;
                }
                else {
                    dp1[i][j]=max(dp1[i-1][j],dp1[i][j-1]);
                    int l1=dp[i-1][j].length();
                    int l2=dp[i][j-1].length();
                    dp[i][j]=l1>=l2?dp[i-1][j]:dp[i][j-1];
                }
            }
        }
        cout<<dp[n][m]<<endl;
        return dp1[n][m];
    }
};
```

## 9. Spiral matrix

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int n=matrix.size();
        int m=matrix[0].size();
        int rowCol[4][2]={{0,1},{1,0},{0,-1},{-1,0}};
        int direction=0;
        int currR=0,currC=0;
        vector<int> ans;
        while(ans.size()<n*m){
            ans.push_back(matrix[currR][currC]);
            int nextR=currR+rowCol[direction][0];
            int nextC=currC+rowCol[direction][1];
            matrix[currR][currC]=INT_MAX;
            if(nextR<0 || nextR>=n || nextC<0 || nextC>=m || matrix[nextR][nextC] == INT_MAX){
                direction=(direction+1)%4;
            }
            currR=currR+rowCol[direction][0];
            currC=currC+rowCol[direction][1];
        }
        return ans;
    }
};
```

