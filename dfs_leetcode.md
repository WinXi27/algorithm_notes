# 本文章解答leetCode的DFS(深度优先搜索)所有题目
按照leetcode上的顺序排版
保证代码都AC
# 79.单词搜索
```cpp
class Solution {
public:
    int r=0;
    int c=0;
    int d[5]={0,-1,0,1,0};
    int visit[10][10];
    bool dfs(int i,int j,int idx,vector<vector<char>>&board,string word){
        if(idx==word.size() )return true;
        if(i<0||j<0||i>=r||j>=c||board[i][j]!=word[idx]||visit[i][j])return false;
        visit[i][j]=true;
        for(int k=0;k<4;++k){
            int x=i+d[k];
            int y=j+d[k+1];
            if(dfs(x,y,idx+1,board,word) )return true;
        }
        visit[i][j]=false;
        return false;
    }
    bool exist(vector<vector<char>>& board, string word) {
        r=board.size();
        c=board[0].size();
        for(int i=0;i<r;++i){
            for(int j=0;j<c;++j){
                if(dfs(i,j,0,board,word)==true)return true;
            }
        }
        return false;
    }
};
```
# 94.二叉树中序遍历
## dfs解法
```cpp
class Solution {
public:
    void dfs(vector<int>&ans,TreeNode*root){
        if(root==nullptr)return ;
        dfs(ans,root->left);
        ans.push_back(root->val);
        dfs(ans,root->right);
        return ;
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int>ans;
        dfs(ans,root);
        return ans;
    }
};
```
## stack解法1
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (root == nullptr)
            return {};
        stack<TreeNode*> st;
        vector<int> ans;
        st.push(root);
        while (!st.empty()) {
            if (root != nullptr) {
                while (root->left) {
                    st.push(root->left);
                    root = root->left;
                }
            }
            TreeNode* cur = st.top();
            st.pop();
            ans.push_back(cur->val);
            if (cur->right)
                st.push(cur->right), root = cur->right;
        }
        return ans;
    }
};
```
## stack解法2
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*>st;
        vector<int>ans;
        while(root||!st.empty() ){
            while(root){
                st.push(root);
                root=root->left;
            }
            TreeNode*cur=st.top();
            st.pop();
            ans.push_back(cur->val);
            if(cur->right)root=cur->right;
        }
        return ans;
    }
};
```
# 98.验证完全二叉树
```cpp
class Solution {
public:
#define ll long long 
    bool dfs(TreeNode*root,ll lower,ll upper){
        if(root==nullptr)return true;
        if(!(lower<root->val&&root->val<upper) )return false;
        return dfs(root->left,lower,root->val)&&dfs(root->right,root->val,upper);
    }
    bool isValidBST(TreeNode* root) {
        return dfs(root,(ll)INT_MIN-1,(ll)INT_MAX+1);
    }
};
```
# 99.恢复二叉搜索树()
```cpp

```
# 100.相同的树
```cpp
class Solution {
public:
    bool dfs(TreeNode*p,TreeNode*q){
        if(p==nullptr&&q==nullptr)return true;
        if(p==nullptr||q==nullptr)return false;
        return p->val==q->val&&dfs(p->left,q->left)&&dfs(p->right,q->right);
    }
    bool isSameTree(TreeNode* p, TreeNode* q) {
        return dfs(p,q);
    }
};
```
# 101.对称二叉树
```cpp
class Solution {
public:
    bool dfs(TreeNode*left,TreeNode*right){
        if(left==nullptr&&right==nullptr)return true;
        if(left==nullptr||right==nullptr)return false;
        return left->val==right->val&&dfs(left->left,right->right)&&dfs(left->right,right->left);
    }
    bool isSymmetric(TreeNode* root) {
        if(root==nullptr)return true;
        return dfs(root->left,root->right);
    }
};
```
# 104.二叉树的最大深度
## bfs解法
```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(root==nullptr)return 0;
        queue<TreeNode*>que;
        que.push(root);
        int level=0;
        while(!que.empty() ){
            level++;
            int size=que.size();
            while(size--){
                TreeNode*cur=que.front();
                que.pop();
                if(cur->left)que.push(cur->left);
                if(cur->right)que.push(cur->right);
            }
        }
        return level;
    }
};
```
## dfs解法
```cpp
class Solution {
public:
    int dfs(TreeNode*root){
        if(root==nullptr)return 0;
        return max(1+dfs(root->left),1+dfs(root->right) );
    }
    int maxDepth(TreeNode* root) {
        return dfs(root);
    }
};
``
# 110.平衡二叉树
```cpp
class Solution {
public:
    int get_height(TreeNode*root){
        if(root==nullptr)return 0;
        int left_height=get_height(root->left);
        int right_height=get_height(root->right);
        if(left_height==-1||right_height==-1||abs(right_height-left_height)>1)return -1;
        return 1+max(left_height,right_height);
    }
    bool isBalanced(TreeNode* root) {
        return get_height(root)!=-1;
    }
};
```
# 111.二叉树的最小深度
## bfs解法
```cpp
class Solution {
public:
    int minDepth(TreeNode* root) {
        if(root==nullptr)return 0;
        queue<TreeNode*>que;
        que.push(root);
        int level=0;
        while(!que.empty() ){
            level++;
            int size=que.size();
            while(size--){
                TreeNode*cur=que.front();
                que.pop();
                if(cur->left)que.push(cur->left);
                if(cur->right)que.push(cur->right);
                if(!cur->left&&!cur->right)return level;
            }
        }
        return level;
    }
};
```
## dfs解法
```cpp
class Solution {
public:
#define ll long long 
    ll dfs(TreeNode*root){
        if(root==nullptr)return INT_MAX;
        if(!root->left&&!root->right)return 1;
        return 1+min(dfs(root->left),dfs(root->right) );
    }
    int minDepth(TreeNode* root) {
        if(root==nullptr)return 0;
        return dfs(root);
    }
};
```
# 112.路径总和
```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if(root==nullptr)return false;
        queue<TreeNode*>que;
        queue<int>sum;
        que.push(root);
        sum.push(root->val);
        int level=0;
        while(!que.empty() ){
            level++;
            int size=que.size();
            while(size--){
                TreeNode*cur=que.front();
                int cur_sum=sum.front();
                if(!cur->left&&!cur->right&&cur_sum==targetSum)return true;
                sum.pop();
                que.pop();
                if(cur->left){
                    que.push(cur->left);
                    sum.push(cur->left->val+cur_sum);
                }
                if(cur->right){
                    que.push(cur->right);
                    sum.push(cur->right->val+cur_sum);
                }
            }
        }
        return false;
    }
};
```
# 113.路径总和 II
```cpp
class Solution {
public:
    vector<vector<int>>ans;
    vector<int>temp;
    int target=0;
    int sum=0;
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        temp.push_back(root->val);
        sum+=root->val;
        if(!root->left&&!root->right&&sum==target){
            ans.push_back(temp);
        }
        dfs(root->left);
        dfs(root->right);
        temp.pop_back();        
        sum-=root->val;
        return ;
    }
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if(root==nullptr)return {};
        target=targetSum;
        dfs(root);
        return ans;
    }
};
```
# 114.二叉树展开为链表
```cpp
class Solution {
public:
    void pre(TreeNode*root,vector<TreeNode*>&nums){
        if(root==nullptr)return ;
        nums.push_back(root);
        pre(root->left,nums);
        pre(root->right,nums);
        return ;
    }
    void flatten(TreeNode* root) {
        vector<TreeNode*>nums;
        pre(root,nums);
        TreeNode*head=root;
        for(int i=1;i<nums.size();++i){
            head->left=nullptr;
            head->right=nums[i];
            head=head->right;
        }
        return ;
    }
};
```
# 116.填充每个节点的下一个右侧节点指针
```cpp
class Solution {
public:
    Node* connect(Node* root) {
        if(root==nullptr)return root;
        queue<Node*>que;
        que.push(root);
        while(!que.empty() ){
            int size=que.size();
            while(size--){
                Node*cur=que.front();
                que.pop();
                if(size==0){
                    cur->next=nullptr;
                }else{
                    Node*Next=que.front();
                    cur->next=Next;
                }
                if(cur->left)que.push(cur->left);
                if(cur->right)que.push(cur->right);
            }
        }
        return root;
    }
};
```
# 117.填充每个节点的下一个右侧节点指针 II
```cpp
class Solution {
public:
    Node* connect(Node* root) {
        if(root==nullptr)return root;
        queue<Node*>que;
        que.push(root);
        while(!que.empty() ){
            int size=que.size();
            while(size--){
                Node*cur=que.front();
                que.pop();
                if(size==0){
                    cur->next=nullptr;
                }else{
                    Node*Next=que.front();
                    cur->next=Next;
                }
                if(cur->left)que.push(cur->left);
                if(cur->right)que.push(cur->right);
            }
        }
        return root;
    }
};
```
# 124.二叉树中的最大路径和()
```cpp

```
# 129.求根节点到叶节点数字之和
```cpp
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        queue<TreeNode*>que;
        queue<int>sum;
        que.push(root);
        sum.push(root->val);
        int level=0;
        int ans=0;
        while(!que.empty() ){
            int size=que.size();
            level++;
            while(size--){
                TreeNode*cur=que.front();
                int cur_sum=sum.front();
                que.pop();
                sum.pop();
                if(!cur->left&&!cur->right){
                    ans+=cur_sum;
                }
                if(cur->left){
                    que.push(cur->left);
                    sum.push(cur->left->val+10*cur_sum);
                }
                if(cur->right){
                    que.push(cur->right);
                    sum.push(cur->right->val+10*cur_sum);
                }
            }
        }
        return ans;
    }
};
```
# 
```cpp

```
# 
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```


# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```


# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```


# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```


# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```


# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```

# 79.单词搜索
```cpp

```


