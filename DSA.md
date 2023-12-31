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

## 10. 2-D matrix breaker

```cpp
```

## 11. Keyboard: Generate max A's with select, copy, and paste

```cpp
class Solution{
public:
    long long int optimalKeys(int N){
        // code here
        
        if(N<=6)
            return (long long)N;
        vector<long long> ans(N+1,0);
        for(int i=0;i<=6;i++)
            ans[i]=i;
            
        for(int i=7;i<=N;i++){
            // long long maxx=INT_MIN;
            for(int b=i-3;b>=1;b--){
                long long curr=(i-b-1)*ans[b];
                ans[i]=max(curr,ans[i]);
            }
            // ans[i]=maxx;
        }
        return ans[N];
        
    }
};
```
## 12. Cut and maximize segments

```cpp
{
    public:
    //Function to find the maximum number of cuts
    int func(int n, int x, int y, int z, vector<int> &memo){
        if(n<0)
            return INT_MIN;
        if(n==0)
            return 0;
        if(memo[n]!=-1){
            return memo[n];
        }
        
        int n1=1+func(n-x,x,y,z,memo);
        int n2=1+func(n-y,x,y,z,memo);
        int n3=1+func(n-z,x,y,z,memo);
        
        memo[n]=max(n1,max(n2,n3));
        return memo[n];
    }
    
    int maximizeTheCuts(int n, int x, int y, int z)
    {
        vector<int> memo(n+1,-1);
        memo[0]=0;
        return max(func(n,x,y,z,memo),0);
    }
};
```

## 13. LRU cache (dll+map)

```cpp
class LRUCache {
public:
    // dll class
    class Node{
        public:
            int key;
            int value;
            Node* prev;
            Node* next;
            Node(int key, int value){
                this->key=key;
                this->value=value;
                this->prev=NULL;
                this->next=NULL;
            }
    };


    Node* head= new Node(-1,-1);
    Node* tail= new Node(-1,-1);

    unordered_map<int,Node*> mp;
    int cap;

    LRUCache(int capacity) {
        cap=capacity;
        head->next=tail;
        tail->prev=head;
    }

    void deleteNode(Node* pos){
        Node* prevPos=pos->prev;
        Node* nextPos=pos->next;
        prevPos->next=nextPos;
        nextPos->prev=prevPos;
    }

    void addNode(Node* pos){
        Node* temp=head->next;

        pos->next=temp;
        pos->prev=head;

        head->next=pos;
        temp->prev=pos;
    }
    
    int get(int key) {
        if(mp.find(key)!=mp.end()){

            //1. get the key->value(node->value), and erase from the map
            //2. erase the node from the dll
            //3. add the node in the front of dll
            //4. update the map of key with new position of the node

            Node* pos=mp[key];
            int ans= pos->value;
            mp.erase(key);
            deleteNode(pos);
            addNode(pos);
            mp.insert({key,head->next});
            return ans;
        }
        else return -1;
    }
    
    void put(int key, int value) {
        //0. if key already present in map, delete from map, insert the node in the first position of dll, add again in map with new node pointer
        //1. if capacity is full, then delete the last node from the dll and the map
        //2. add new node in the front of dll
        //3. add the key and node posiion in the map

        if(mp.find(key)!=mp.end()){
            Node* pos=mp[key];
            mp.erase(key);
            deleteNode(pos);
        }
        else{
            if(mp.size()==cap){
                Node* lastNode=tail->prev;
                mp.erase(lastNode->key);
                deleteNode(lastNode);
            }
        }
        addNode(new Node(key,value));
        mp.insert({key,head->next});
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```

## 14. Mid of linked list

```cpp
Node* getMiddle(Node *head)
{
     struct Node *slow = head;
     struct Node *fast = head;
 
     if (head)
     {
         while (fast != NULL && fast->next != NULL)
         {
             fast = fast->next->next;
             slow = slow->next;
         }
     }
     return slow;
}
```

## 15. Flatten a Binary tree to Linked list (preorder)

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:

    pair<TreeNode*,TreeNode*> convert(TreeNode* root){
        if(root==NULL)
            return {root,root};


        TreeNode* head = root; 
        TreeNode* tail = root;


        pair<TreeNode*, TreeNode*> leftList=convert(root->left);
        pair<TreeNode*, TreeNode*> rightList=convert(root->right);

        //if left subtree and respective linked list exist
        if(leftList.second){
            leftList.second->right=rightList.first;
            head->right=leftList.first;
            tail=leftList.second;
        }

        head->left=NULL;

        if(rightList.first){
            tail=rightList.second;
        }

        return {head,tail};
    }
    void flatten(TreeNode* root) {
        if(root==NULL)
            return;
        convert(root);
        return;
    }
};
```

## 16. Binary tree-> doubly linked list (inorder)

```cpp
class Solution
{
    public: 
    //Function to convert binary tree to doubly linked list and return it.
    
    pair<Node*,Node*> convert(Node* root){
        if(root==NULL)
            return {root,root};
        if(!root->left && !root->right) 
            return {root,root};
        
        Node* head=root;
        Node* tail=root;
        
        pair<Node*,Node*> leftList=convert(root->left);
        pair<Node*,Node*> rightList=convert(root->right);
        
        if(leftList.first){
            leftList.second->right=head;
            head->left=leftList.second;
            head=leftList.first;
        }
        if(rightList.first){
            root->right=rightList.first;
            rightList.first->left=root;
            tail=rightList.second;
        }
        
        return {head,tail};
    }
    
    Node * bToDLL(Node *root)
    {
        if(root==NULL)
            return root;
        // your code here
        pair<Node*,Node*> newRoot=convert(root);
        return newRoot.first;
    }
};
```

#### simple recursion

```cpp
class Solution
{
    public: 
    //Function to convert binary tree to doubly linked list and return it.
    Node* prev=NULL;
    Node * bToDLL(Node *root)
    {
        // your code here
        if(root==NULL)
            return root;
        Node* head=bToDLL(root->left);
        
        if(prev==NULL){
            head=root;
        }
        else{
            root->left=prev;
            prev->right=root;
        }
        prev=root;
        bToDLL(root->right);
        return head;
    }
};
```

## 17. Linked list has cycle?

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *slow=head, *fast=head;
        while(fast && fast->next){
            fast=fast->next->next;
            slow=slow->next;
            if(fast==slow)
                return true;
        }
        return false;
    }
};
```

## 18. Where does the cycle starts from in Linkedlist?

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(head==NULL || head->next==NULL)
            return NULL;
        ListNode *slow=head, *fast=head;

        while(fast && fast->next){
            slow=slow->next;
            fast=fast->next->next;
            if(slow==fast)
                break;
        }
        if (!(fast && fast->next)) 
            return NULL;

        slow=head;
        while(slow!=fast){
            slow=slow->next;
            fast=fast->next;
        }
        return fast;
    }
};
```

## 19. Find duplicate in an array of [1,n-1] (O(n),O(1) using slow and fast)

```cpp
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int slow=0,fast=0;
        do{
            slow=nums[slow];
            fast=nums[nums[fast]];
        }while(slow!=fast);

        slow=0;
        while(slow!=fast){
            slow=nums[slow];
            fast=nums[fast];
        }
        return slow;
    }
};
```

## 20. Intersection point of two linked-list

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        if(!headA || !headB) return NULL;
        int n=0,m=0;
        ListNode* temp=headA;
        while(temp){
            n++;
            temp=temp->next;
        }
        ListNode* temp2=headB;
        while(temp2){
            m++;
            temp2=temp2->next;
        }

        if(n>m){
            while(n>m){
                headA=headA->next;
                n--;
            }
        }
        else if(n<m){
            while(n<m){
                headB=headB->next;
                m--;
            }
        }
        while(headA && headB){
            if(headA==headB)
                return headA;
            headA=headA->next;
            headB=headB->next;
        }
        return NULL;

    }
};
```

## 21. Reverse linked list

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev=NULL;
        while(head!=NULL){
            ListNode* t=head->next;
            head->next=prev;
            prev=head;
            head=t;
        }
        return prev;
    }
};
```

## 22. Palindrome linked list

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(head == NULL || head->next == NULL){
            return head;
        }

        ListNode* rhead=NULL;
        ListNode* ptr=head;
        while(ptr!=NULL){
            ListNode* temp = new ListNode(ptr->val);
            temp->next=rhead;
            rhead=temp;
            ptr=ptr->next;
        }
        while(rhead && head){
            if(head->val!=rhead->val)
                return false;
            rhead=rhead->next;
            head=head->next;
        }
        return true;
    }
};
```

## 23. balanced binary tree

```cpp
class Solution{
    public:
    //Function to check whether a binary tree is balanced or not.
    pair<int,bool> solve(Node* root){
        if(root==NULL)
            return {0,true};
        pair<int,bool> l=solve(root->left);
        pair<int,bool> r=solve(root->right);
        int d=abs(l.first-r.first);
        if(l.second && r.second && (d==0 ||d==1)){
            return {(max(l.first,r.first)+1),true};
        }
        return {(max(l.first,r.first)+1),false};
    }
    bool isBalanced(Node *root)
    {
        //  Your Code here
        return solve(root).second;
    }
};
```

## 24. sorted LL to BST (vector extra space)

```cpp
class Solution{
  public:
    
    
    TNode* solve(vector<int> &v, int start, int end){
        if(start>end)
            return NULL;
        int mid=start+(end-start)/2;
        if((end-start)%2!=0)
            mid=mid+1;
        TNode* root=new TNode(v[mid]);
        root->left=solve(v,start,mid-1);
        root->right=solve(v,mid+1,end);
        return root;
    }
    
    TNode* sortedListToBST(LNode *head) {
        //code here
        vector<int> v;
        while(head){
            v.push_back(head->data);
            head=head->next;
        }
        TNode* root=solve(v,0,v.size()-1);
        return root;
    }
};
```

## 25. sorted LL to BST(no space)

```cpp
class Solution{
  public:
    
    
    TNode* solve(LNode **node_ref,int n){
        if(n<=0)
            return NULL;
        TNode* l=solve(node_ref,n/2);
        TNode* root=new TNode((*node_ref)->data);
        root->left=l;
        (*node_ref)=(*node_ref)->next;
        root->right=solve(node_ref,(n-n/2-1));
        return root;
    }
    
    TNode* sortedListToBST(LNode *root) {
        //code here
        int n=0;
        LNode* head=root;
        while(head){
            n++;
            head=head->next;
        }
        
        TNode* rooth=solve(&root,n);
        return rooth;
    }
};
```

## 26. Diameter of a BT

```cpp
class Solution {
  public:
    // Function to return the diameter of a Binary Tree.
    pair<int,int> diameterFast(Node* root){
        if(root==NULL){
            return {0,0};
        }
        
        pair<int,int> left=diameterFast(root->left);
        pair<int,int> right=diameterFast(root->right);
        
        int op1=left.first;
        int op2=right.first;
        //including the root
        int op3=left.second+right.second+1;
        
        pair<int,int>ans;
        //max diameter
        ans.first=max(op1,max(op2,op3));
        //max height for the current subtree
        ans.second=max(left.second,right.second)+1;
        return ans;
    }
    int diameter(Node* root) {
        // Your code here
        return diameterFast(root).first;
    }
};
```

## 27. Check if BST

```cpp
bool validate(Node*root,int min,int max){
        if(root==NULL){
            return true ;
        }
        if(root->data>min && root->data<max){
            bool left=validate(root->left,min,root->data);
            bool right=validate(root->right,root->data,max);
        return(left && right);
    }
    else{
        return false;
    }
 }
    
    bool isBST(Node* root) 
    {
      return validate(root,INT_MIN,INT_MAX)  ;
}
```

## 28. Kth largest element in BST (reverse inorder)

```cpp
class Solution
{
    public:
    int solve(Node* root, int &k){
        if(root==NULL)
            return INT_MIN;
        int r=solve(root->right,k);
        if(k==0)
            return r;
        k-=1;
        if(k==0)
            return root->data;
        return solve(root->left,k);
    }
    int kthLargest(Node *root, int k)
    {
        //Your code here
        if(root==NULL)
            return INT_MIN;
        return solve(root,k);
    }
};
```

## 29. Inorder successor

```cpp
class Solution{
  public:
    // returns the inorder successor of the Node x in BST (rooted at 'root')
    Node * inOrderSuccessor(Node *root, Node *x)
    {
        //Your code here
        Node* suc=new Node(INT_MIN);
        if(root == NULL)
            return suc;
        Node* curr= root;
        while(curr != NULL){
            if(curr->data > x->data){
                suc = curr;
                curr= curr->left;
            }else{
                if(curr->right==NULL && suc->data<x->data)
                    return NULL;
                curr= curr->right;
                
            }
        }
        return suc;
    }
};
```

## 30. LCA

```cpp
class Solution
{
    public:
    //Function to return the lowest common ancestor in a Binary Tree.
    Node* lca(Node* root ,int p ,int q )
    {
       if(!root) return NULL;
        if(root->data == p || root->data == q) return root;

        Node* lst = lca(root->left, p, q);
        Node* rst = lca(root->right, p, q);
        if(!lst) return rst; 
        if(!rst) return lst; 

        return root;
    }
};
```

## 31. postfix-prefix stack

```cpp
bool isOperand(char c)  
{  
    if((c>='a' && c<='z') || (c>='A' && c<='Z'))  
    {  
        return true;  
    }  
    else  
    {  
        return false;  
    }  
}  
// Converting postfix to prefix expression in C++  
string postfixtoprefix(string postfix)  
{  
    stack<string> st;
    string ex="";
    for(int i=0;i<postfix.length();i++){
        if(isOperand(postfix[i])){
            string op(1, postfix[i]);
            st.push(op);
        }
        else{
            string o2=st.top();
            st.pop();
            string o1=st.top();
            st.pop();
            ex=postfix[i]+o1+o2;
            st.push(ex);
            ex.clear();
        }
    }
    return st.top();
}
```


