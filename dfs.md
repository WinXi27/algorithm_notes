 # 本文章解答leetCode的DFS(深度优先搜索)大部分题目
基本按照leetcode上的顺序排版
**保证代码都AC,先看目录**
题目带()是还没写,之后会补上
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
# 130.被围绕的区域
## dfs解法
```cpp
class Solution {
public:
    int r=0;
    int c=0;
    int d[5]={0,-1,0,1,0};
    void dfs(vector<vector<char>>&board,int i,int j){
        if(i<0||j<0||i>=r||j>=c||board[i][j]!='O')return ;
        board[i][j]='A';
        for(int k=0;k<4;++k){
            int x=i+d[k];
            int y=j+d[k+1];
            dfs(board,x,y);
        }return ;
    }
    void solve(vector<vector<char>>& board) {
        r=board.size();
        c=board[0].size();
        for(int i=0;i<r;++i){
            for(int j=0;j<c;++j){
                if(i==0||j==0||i==r-1||j==c-1){
                    if(board[i][j]=='O')
                        dfs(board,i,j);
                }
            }
        }
        for(int i=0;i<r;++i){
            for(int j=0;j<c;++j){
                if(board[i][j]=='O')board[i][j]='X';
                else if(board[i][j]=='A')board[i][j]='O';
            }
        }
        return ;    
    }
};
```
# 133.克隆图()
```cpp

```
# 144.二叉树的前序遍历
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if(root==nullptr)return {};
        stack<TreeNode*>st;
        st.push(root);
        vector<int>ans;
        while(!st.empty() ){
            TreeNode*cur=st.top();
            st.pop();
            ans.push_back(cur->val);
            if(cur->right)st.push(cur->right);
            if(cur->left)st.push(cur->left);
        }
        return ans;
    }
};
```
# 145.二叉树的后序遍历
```cpp
class Solution {
public:
    //后序遍历: 左 右 中
    //先序遍历: 中 左 右--->翻转--->中 右 左--->
    vector<int> postorderTraversal(TreeNode* root) {
        if(root==nullptr)return {};
        vector<int>ans;
        stack<TreeNode*>st;
        stack<int>temp;
        st.push(root);
        while(!st.empty() ){
            TreeNode*cur=st.top();
            st.pop();
            temp.push(cur->val);
            if(cur->left)st.push(cur->left);
            if(cur->right)st.push(cur->right);
        }
        while(!temp.empty() ){
            ans.push_back(temp.top() );
            temp.pop();
        }
        return ans;
    }
};
```
# 156.上下翻转二叉树()
```cpp

```
# 199.二叉树的右视图
```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        if(root==nullptr)return {};
        queue<TreeNode*>que;
        que.push(root);
        vector<int>ans;
        while(!que.empty() ){
            int size=que.size();
            while(size--){
                TreeNode*cur=que.front();
                que.pop();
                if(size==0){
                    ans.push_back(cur->val);
                }
                if(cur->left)que.push(cur->left);
                if(cur->right)que.push(cur->right);
            }
        }
        return ans;
    }
};
```
# 200.岛屿数量
```cpp
class Solution {
public:
    int r=0;
    int c=0;
    int d[5]={0,-1,0,1,0};
    void dfs(vector<vector<char>>&grid,int i,int j){
        if(i<0||j<0||i>=r||j>=c||grid[i][j]!='1')return ;
        grid[i][j]='0';
        for(int k=0;k<4;++k){
            int x=i+d[k];
            int y=j+d[k+1];
            dfs(grid,x,y);
        }
        return ;
    }
    int numIslands(vector<vector<char>>& grid) {
        r=grid.size();
        c=grid[0].size();
        int cnt=0;
        for(int i=0;i<r;++i){
            for(int j=0;j<c;++j){
                if(grid[i][j]=='1'){
                    cnt++;
                    dfs(grid,i,j);
                }
            }
        }    
        return cnt;
    }
};
```
# 207.课程表
```cpp
class Solution {
public:
    unordered_map<int,vector<int>>graph;
    bool canFinish(int n, vector<vector<int>>& pre) {
        //[0,n-1]
        vector<int>in(n,0);
        for(auto &it:pre){
            int from=it[1];
            int to=it[0];
            graph[from].push_back(to);
            in[to]++;
        }
        queue<int>que;
        int ans=0;
        for(int i=0;i<n;++i)if(in[i]==0)que.push(i);
        while(!que.empty() ){
            int cur=que.front();
            que.pop();
            ans++;
            for(auto &it:graph[cur]){
                in[it]--;
                if(in[it]==0)que.push(it);
            }
        }
        return ans==n;
    }
};
```
# 210.课程表 II
```cpp
class Solution {
public:
    unordered_map<int,vector<int>>graph;
    vector<int>in;
    vector<int> findOrder(int n, vector<vector<int>>& pre) {
        vector<int>ans;
        in.resize(n,0);
        for(auto &it:pre){
            int from=it[1];
            int to=it[0];
            graph[from].push_back(to);
            in[to]++;
        }
        queue<int>que;
        for(int i=0;i<n;++i)if(in[i]==0)que.push(i);
        while(!que.empty() ){
            int cur=que.front();
            que.pop();
            ans.push_back(cur);
            for(auto&it:graph[cur]){
                in[it]--;
                if(in[it]==0)que.push(it);
            }
        }
        return ans.size()==n?ans:vector<int>{};
    }
};
```
# 211.添加与搜索单词 - 数据结构设计()
```cpp

```
# 226.翻转二叉树
## dfs解法1
```cpp
class Solution {
public:
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        dfs(root->left);
        dfs(root->right);
        TreeNode*temp=root->left;
        root->left=root->right;
        root->right=temp;
        return ;
    }
    TreeNode* invertTree(TreeNode* root) {
        dfs(root);
        return root;
    }
};
```
## dfs解法2
```cpp
class Solution {
public:
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        TreeNode*temp=root->left;
        root->left=root->right;
        root->right=temp;
        dfs(root->left);
        dfs(root->right);
        return ;
    }
    TreeNode* invertTree(TreeNode* root) {
        dfs(root);
        return root;
    }
};
```
# 230.二叉搜索树中第 K 小的元素
## dfs解法1
```cpp
class Solution {
public:
    unordered_map<TreeNode*,int>cnt;
    int dfs(TreeNode*root){
        if(root==nullptr)return 0;
        int ans=1+dfs(root->left)+dfs(root->right);
        cnt[root]=ans;
        return ans;
    }
    int kthSmallest(TreeNode* root, int k) {
        dfs(root);
        cnt[nullptr]=0;
        while(root){
            int left_cnt=dfs(root->left);
            int right_cnt=dfs(root->right);
            //[left_cnt][][right_cnt];
            if(left_cnt+1==k)return root->val;
            else if(left_cnt>=k){
                root=root->left;
            }else{
                root=root->right;
                k-=(left_cnt+1);
            }
        }
        return 0;
    }
};
```
## dfs解法2
```cpp
class Solution {
public:
    vector<int>ans;
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        dfs(root->left);
        ans.push_back(root->val);
        dfs(root->right);
        return ;
    }
    int kthSmallest(TreeNode* root, int k) {
        dfs(root);
        return ans[k-1];
    }
};
```
## stack解法3
```cpp
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        stack<TreeNode*>st;
        int cnt=0;
        while(root||!st.empty() ){
            while(root){
                st.push(root);
                root=root->left;
            }
            TreeNode*cur=st.top();
            st.pop();
            cnt++;
            if(cnt==k)return cur->val;
            if(cur->right)root=cur->right;
        }
        return 0;
    }
};
```
# 235.二叉搜索树的最近公共祖先
## dfs解法1
```cpp
class Solution {
public:
    TreeNode*dfs(TreeNode*root,TreeNode*p,TreeNode*q){
        if(root==nullptr)return nullptr;
        //root p q 
        if(root->val<p->val&&root->val<q->val)return dfs(root->right,p,q);
        //p q root
        else if(root->val>q->val&&root->val>p->val)return dfs(root->left,p,q);
        //p root q
        return root;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return  dfs(root,p,q);
    }
};
```
## dfs解法2
```cpp
class Solution {
public:
    TreeNode*dfs(TreeNode*root,TreeNode*p,TreeNode*q){
        if(root==nullptr)return nullptr;
        //p q root
        if(q->val<root->val)return dfs(root->left,p,q);
        //root p q
        if(root->val<p->val)return dfs(root->right,p,q);
        return root;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(p->val>q->val)swap(p,q);
        return dfs(root,p,q);
    }
};
```
# 236.二叉树的最近公共祖先
```cpp
class Solution {
public:
    TreeNode*dfs(TreeNode*root,TreeNode*p,TreeNode*q){
        if(root==nullptr)return nullptr;
        if(root==p||root==q)return root;
        TreeNode*left=dfs(root->left,p,q);
        TreeNode*right=dfs(root->right,p,q);
        if(left&&right)return root;
        if(left||right)return left?left:right;
        return nullptr;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        return dfs(root,p,q);
    }
};
```
# 250.统计同值子树()
```cpp

```
# 257.而擦函数的所有路径
```cpp
class Solution {
public:
    vector<string>ans;
    string temp;
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        temp+=("->"+to_string(root->val) );
        if(!root->left&&!root->right){
            ans.push_back(temp);
        }
        dfs(root->left);
        dfs(root->right);
        int k=2+to_string(root->val).size();
        while(k--)temp.pop_back();
        return ;
    }
    vector<string> binaryTreePaths(TreeNode* root) {
        if(root==nullptr)return {};
        if(!root->left&&!root->right){
            return vector<string>{to_string(root->val)};
        }
        temp+=to_string(root->val);
        dfs(root->left);
        dfs(root->right);
        temp.pop_back();
        return ans;    
    }
};
```
# 261.以图判树
```cpp
class Solution {
public:
    int fa[2005];
    int cnt=0;
    void build(int n){
        for(int i=0;i<n;++i)fa[i]=i;
        cnt=n;
    }
    bool is_same(int n1,int n2){
        return find(n1)==find(n2);
    }
    int find(int n){
        queue<int>que;
        while(fa[n]!=n){
            que.push(n);
            n=fa[n];
        }
        while(!que.empty() ){
            fa[que.front()]=n;
            que.pop();
        }return n;
    }
    void union_ele(int n1,int n2){
        int fa1=find(n1);
        int fa2=find(n2);
        if(fa1==fa2)return ;
        fa[fa1]=fa2;
        cnt--;
    }
    bool validTree(int n, vector<vector<int>>& edges) {
        //[0,n-1]
        build(n);
        for(auto &it:edges){
            int n1=it[0];
            int n2=it[1];
            if(is_same(n1,n2) )return false;
            union_ele(n1,n2);
        }
        return cnt==1;
    }
};
```
# 269.火星词典
## 这题都要被绕懵了,好多边界,哈哈哈
```cpp
class Solution {
public:
    unordered_map<char,vector<char>>graph;
    unordered_map<char,int>in;
    bool visit[26][26];
    bool fun(string s1,string s2){
        if(s1==s2)return true;
        int i=0;
        int j=0;
        while(i<s1.size()&&j<s2.size() ){
            if(s1[i]==s2[j])i++,j++;
            else{
                if(visit[s1[i]-'a'][s2[j]-'a']==false){
                    graph[s1[i]].push_back(s2[j]);
                    in[s2[j]]++;
                    visit[s1[i]-'a'][s2[j]-'a']=true;
                }
                return true;
            }
        }
        if(j==s2.size() )return false;
        return true;
    }
    bool have[26];
    string alienOrder(vector<string>& words) {
        int cnt=0;
        for(int i=0;i<words.size();++i){
            for(char &c:words[i]){
                if(have[c-'a']==false){
                    cnt++;
                    have[c-'a']=true;
                }                
            }
            for(int j=i+1;j<words.size();++j){
                if(fun(words[i],words[j])==false)
                    return "";
            }
        }
        queue<char>que;
        for(char c='a';c<='z';++c){
            if(have[c-'a']&&in[c]==0)que.push(c);
        }
        string ans;
        while(!que.empty() ){
            char cur=que.front();
            que.pop();
            ans.push_back(cur);
            for(char &c:graph[cur]){
                in[c]--;
                if(in[c]==0)que.push(c);
            }
        }
        return ans.size()==cnt?ans:"";
    }
};
```
# 270.最接近的二叉搜索树值
```cpp
class Solution {
public:
    int ans=0;
    double min_dif=INT_MAX;
    double target;
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        double cur_dif=abs(root->val-target);
        if(cur_dif<min_dif){
            min_dif=cur_dif;
            ans=root->val;
        }else if(cur_dif==min_dif){
            ans=min(ans,root->val);
        }
        dfs(root->left);
        dfs(root->right);
        return ;
    }
    int closestValue(TreeNode* root, double target1) {
        target=target1;
        dfs(root);
        return ans;
    }
};
```
# 272.最接近的二叉搜索树值 II
```cpp
class Solution {
public:
    double target=0;
    vector<int>arr;
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        arr.push_back(root->val);
        dfs(root->left);
        dfs(root->right);
        return ;
    }
    vector<int> closestKValues(TreeNode* root, double target1, int k) {
        target=target1;
        vector<int>ans;
        dfs(root);
        sort(arr.begin(),arr.end(),[&](const int&s1,const int&s2){
            return abs(target-s1)<abs(target-s2);
        });
        for(int i=0;i<k;++i)ans.push_back(arr[i]);
        return ans;
    }
};
```
# 285.二叉搜索树中的中序后继
```cpp
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        stack<TreeNode*>st;
        bool flag=false;
        while(root||!st.empty() ){
            while(root){
                st.push(root);
                root=root->left;
            }
            TreeNode*cur=st.top();
            st.pop();
            if(flag)return cur;
            if(cur==p)flag=true;
            root=cur->right;
        }
        return nullptr;
    }
};
```
# 297.二叉树的序列化与反序列化()
```cpp

```
# 298.二叉树最长连续序列()
```cpp

```
# 302.包含全部黑色像素的最小矩形()
```cpp

```
# 310.最小高度树()
```cpp

```
# 314.二叉树的垂直遍历
```cpp
class Solution {
public:
    unordered_map<int,vector<int>>arr;
    vector<vector<int>> verticalOrder(TreeNode* root) {
        if(root==nullptr)return {};
        queue<TreeNode*>que;
        queue<int>flag;
        que.push(root);
        flag.push(0);
        int minn=INT_MAX;
        int maxx=INT_MIN;
        while(!que.empty() ){
            int size=que.size();
            while(size--){
                TreeNode*cur=que.front();
                int cur_flag=flag.front();
                minn=min(minn,cur_flag);
                maxx=max(maxx,cur_flag);
                que.pop();
                flag.pop();
                arr[cur_flag].push_back(cur->val);
                if(cur->left){
                    que.push(cur->left);
                    flag.push(cur_flag-1);
                }
                if(cur->right){
                    que.push(cur->right);
                    flag.push(cur_flag+1);
                }
            }
        } 
        vector<vector<int>>ans;
        for(int i=minn;i<=maxx;++i){
            ans.push_back(arr[i]);
        }
        return ans;
    }
};
```
# 323.无向图中连通分量的数目
```cpp
class Solution {
public:
    int fa[2005];
    int set_cnt=0;
    void build(int n){
        for(int i=0;i<n;++i)fa[i]=i;
        set_cnt=n;
    }
    int find(int n){
        queue<int>que;
        while(n!=fa[n]){
            que.push(n);
            n=fa[n];
        }
        while(!que.empty() ){
            fa[que.front()]=n;
            que.pop();
        }
        return n;
    }
    void union_ele(int n1,int n2){
        int fa1=find(n1);
        int fa2=find(n2);
        if(fa1==fa2)return ;
        fa[fa1]=fa2;
        set_cnt--;
    }
    int countComponents(int n, vector<vector<int>>& edges) {
        //[0,n]
        build(n);
        for(auto &it:edges){
            int n1=it[0];
            int n2=it[1];
            union_ele(n1,n2);
        }
        return set_cnt;
    }
};
```
# 329.矩阵中的最长递增路径
```cpp
class Solution {
public:
    int r=0;
    int c=0;
    int d[5]={0,-1,0,1,0};
    int dp[205][205];
    int dfs(vector<vector<int>>&matrix,int i,int j){
        if(dp[i][j]!=0)return dp[i][j];
        int ans=1;
        for(int k=0;k<4;++k){
            int x=i+d[k];
            int y=j+d[k+1];
            if(x>=0&&y>=0&&x<r&&y<c&&matrix[x][y]>matrix[i][j]){
                ans=max(ans,1+dfs(matrix,x,y) );
            }
        }
        dp[i][j]=ans;
        return ans;
    }
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        r=matrix.size();
        c=matrix[0].size();
        int maxx=0;
        for(int i=0;i<r;++i){
            for(int j=0;j<c;++j){
                maxx=max(maxx,dfs(matrix,i,j) );
            }
        }
        return maxx;
    }
};
```
# 332.重新安排行程()
```cpp

```
# 333.最大二叉搜索子树()
```cpp

```
# 337.打家劫舍 III
```cpp
class Solution {
public:
    unordered_map<TreeNode*,int>mp;
    int dfs(TreeNode*root){
        if(root==nullptr)return 0;
        if(mp[root]!=0)return mp[root]; 
        int temp1=dfs(root->left)+dfs(root->right);
        int temp2=root->val
                +(root->left?dfs(root->left->left):0)
                +(root->left?dfs(root->left->right):0)
                +(root->right?dfs(root->right->left):0)
                +(root->right?dfs(root->right->right):0);
        int ans=max(temp1,temp2);
        mp[root]=ans;
        return ans;
    }
    int rob(TreeNode* root) {
        mp[nullptr]=0;
        return dfs(root);
    }
};
```
# 339.嵌套列表加权和()
```cpp

```

# 341.扁平化嵌套列表迭代器()
```cpp

```
# 364.嵌套列表加权和 II()
```cpp

```
# 365.水壶问题()
```cpp

```
# 366.寻找二叉树的叶子节点
```cpp
class Solution {
public:
    vector<vector<int>>ans;
    vector<int>temp;
    unordered_map<TreeNode*,int>mp;
    unordered_map<int,vector<int>>arr;
    int minn=INT_MAX;
    int maxx=INT_MIN;
    int dfs(TreeNode*root){
        if(root==nullptr)return 0;
        if(mp[root]!=0)return mp[root];
        int left_height=dfs(root->left);
        int right_height=dfs(root->right);
        int ans=1+max(left_height,right_height);
        mp[root]=ans;
        arr[ans].push_back(root->val);
        maxx=max(maxx,ans);
        minn=min(minn,ans);
        return ans;
    }
    vector<vector<int>> findLeaves(TreeNode* root) {
        dfs(root);
        for(int height=minn;height<=maxx;++height){
            ans.push_back(arr[height]);
        }
        return ans;
    }
};
```
# 385.迷你语法分析器()
```cpp

```
# 386.字典序排数()
```cpp

```
# 388.文件的最长绝对路径()
```cpp

```
# 399.除法求值()
```cpp

```
# 404.左叶子之和
## bfs解法
```cpp
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        queue<TreeNode*>que;
        queue<bool>flag;
        int sum=0;
        que.push(root);
        flag.push(false);
        while(!que.empty() ){
            int size=que.size();
            while(size--){
                TreeNode*cur=que.front();
                que.pop();
                if(flag.front() )sum+=cur->val;
                flag.pop();
                if(cur->left){
                    que.push(cur->left);
                    if(!cur->left->left&&!cur->left->right){
                        flag.push(true);
                    }else flag.push(false);
                }
                if(cur->right){
                    que.push(cur->right);
                    flag.push(false);
                }
            }
        }
        return sum;
    }
};
```
## dfs解法
```cpp
class Solution {
public:
    int sum=0;
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        dfs(root->left);
        dfs(root->right);
        if(root->left&&!root->left->left&&!root->left->right)
            sum+=root->left->val;
        return ;
    }
    int sumOfLeftLeaves(TreeNode* root) {
        dfs(root);
        return sum;
    }
};
```
# 417.太平洋大西洋水流问题()
```cpp

```
# 419.棋盘上的战舰
```cpp
class Solution {
public:
    int r = 0;
    int c = 0;
    void dfs(vector<vector<char>>&board,int i,int j){
        for(int k=i+1;k<r&&board[k][j]=='X';++k)board[k][j]='.';
        for(int k=j+1;k<c&&board[i][k]=='X';++k)board[i][k]='.';
        board[i][j]='.';
    }
    int countBattleships(vector<vector<char>>& board) {
        r = board.size();
        c = board[0].size();
        int cnt=0;
        for (int i = 0; i < r; ++i) {
            for (int j = 0; j < c; ++j) {
                if (board[i][j] == 'X') {
                    dfs(board,i,j);
                    cnt++;
                }
            }
        }
        return cnt;
    }
};
```
# 463.岛屿的周长
```cpp
class Solution {
public:
    int r=0;
    int c=0;
    bool visit[105][105];
    int d[5]={0,-1,0,1,0};
    int dfs(vector<vector<int>>&grid,int i,int j){
        if(i<0||j<0||i>=r||j>=c)return 1;
        if(visit[i][j])return 0;
        if(grid[i][j]!=1)return 1;
        int ans=0;
        visit[i][j]=true;
        for(int k=0;k<4;++k){
            int x=i+d[k];
            int y=j+d[k+1];
            ans+=dfs(grid,x,y);
        }return ans;
    }
    int islandPerimeter(vector<vector<int>>& grid) {
        r=grid.size();
        c=grid[0].size();
        for(int i=0;i<r;++i){
            for(int j=0;j<c;++j){
                if(grid[i][j]==1){
                    return dfs(grid,i,j);
                }
            }
        }    
        return 0;
    }
};
```
# 472.连接词()
```cpp

```
# 490.迷宫()
```cpp

```
# 499.迷宫 III()
```cpp

```
# 501.二叉搜索树中的众数
```cpp
class Solution {
public:
    unordered_map<int,int>cnt;
    vector<int>ans;
    int max_cnt=0;
    void dfs(TreeNode*root){
        if(root==nullptr)return ;
        cnt[root->val]++;
        if(cnt[root->val]==max_cnt){
            ans.push_back(root->val);
        }else if(cnt[root->val]>max_cnt){
            ans.clear();
            max_cnt=cnt[root->val];
            ans.push_back(root->val);
        }
        dfs(root->left);
        dfs(root->right);
        return ;
    }
    vector<int> findMode(TreeNode* root) {
        dfs(root);
        return ans;
    }
};
```
# 508.出现次数最多的子树元素和
```cpp
class Solution {
public:
    int max_cnt=0;
    vector<int>ans;
    unordered_map<int,int>cnt;
    int dfs(TreeNode*root){
        if(root==nullptr)return 0;
        int sum=root->val+dfs(root->left)+dfs(root->right);
        cnt[sum]++;
        if(cnt[sum]>max_cnt){
            ans.clear();
            ans.push_back(sum);
            max_cnt=cnt[sum];
        }else if(cnt[sum]==max_cnt){
            ans.push_back(sum);
        }
        return sum;
    }
    vector<int> findFrequentTreeSum(TreeNode* root) {
        dfs(root);     
        return ans;    
    }
};
```
# 513.找树左下角的值
```cpp
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        if(root==nullptr)return 0;
        queue<TreeNode*>que;
        que.push(root);
        int level=0;
        int ans=0;
        while(!que.empty() ){
            level++;
            int size=que.size();
            ans=que.front()->val;
            while(size--){
                TreeNode*cur=que.front();
                que.pop();
                if(cur->left)que.push(cur->left);
                if(cur->right)que.push(cur->right);
            }
        }
        return ans;
    }
};
```
# 515.在每个树行中找最大值
```cpp
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        if(root==nullptr)return {};
        vector<int>ans;
        queue<TreeNode*>que;
        que.push(root);
        while(!que.empty() ){
            int size=que.size();
            int maxx=INT_MIN;
            while(size--){
                TreeNode*cur=que.front();
                que.pop();
                maxx=max(maxx,cur->val);
                if(cur->left)que.push(cur->left);
                if(cur->right)que.push(cur->right);
            }
            ans.push_back(maxx);
        }
        return ans;
    }
};
```
# 530.二叉搜索树的最小绝对差
```cpp
class Solution {
public:
    int getMinimumDifference(TreeNode* root) {
        if(root==nullptr)return 0;
        queue<TreeNode*>que;
        que.push(root);
        vector<int>arr;
        while(!que.empty() ){
            int size=que.size();
            while(size--){
                TreeNode*cur=que.front();
                que.pop();
                arr.push_back(cur->val);
                if(cur->left){
                    que.push(cur->left);
                }
                if(cur->right){
                    que.push(cur->right);
                }
            }
        }
        int minn=INT_MAX;
        sort(arr.begin(),arr.end() );
        for(int i=0;i<arr.size()-1;++i){
            minn=min(minn,abs(arr[i]-arr[i+1]) );
        }
        return minn;
    }
};
```
# 543.二叉树的直径
```cpp
class Solution {
public:
    int maxx=0;
    int dfs(TreeNode*root){
        if(root==nullptr)return 0;
        int left=dfs(root->left);
        int right=dfs(root->right);
        int ans=max(left,right)+1;
        maxx=max(maxx,right+left+1-1);
        return ans;
    }
    int diameterOfBinaryTree(TreeNode* root) {
        dfs(root);
        return maxx;
    }
};
```
# 545.二叉树的边界()
```cpp

```
# 547.省份数量
```cpp
class Solution {
public:
    int fa[205];
    int set_cnt=0;
    void build(int n){
        for(int i=1;i<=n;++i)fa[i]=i;
        set_cnt=n;
    }
    int find(int n){
        queue<int>que;
        while(fa[n]!=n){
            que.push(n);
            n=fa[n];
        }
        while(!que.empty() ){
            fa[que.front()]=n;
            que.pop();
        }return n;
    }
    void union_ele(int n1,int n2){
        int fa1=find(n1);
        int fa2=find(n2);
        if(fa1==fa2)return ;
        fa[fa1]=fa2;
        set_cnt--;
    }
    int findCircleNum(vector<vector<int>>& is) {
        int n=is.size();
        //[1,n]
        build(n);
        for(int i=0;i<n;++i){ 
            for(int j=0;j<n;++j){ 
                if(is[i][j])union_ele(i+1,j+1); 
            } 
        }   
        return set_cnt;
    } 
};
```
# 559.N 叉树的最大深度
## bfs解法
```cpp
class Solution {
public:
    int maxDepth(Node* root) {
        if(root==nullptr)return 0;
        queue<Node*>que;
        que.push(root);
        int level=0;
        while(!que.empty() ){
            level++;
            int size=que.size();
            while(size--){
                Node*cur=que.front();
                que.pop();
                for(auto &it:cur->children){
                    que.push(it);
                }
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
    int dfs(Node*root){
        if(root==nullptr)return 0;
        int ans=1;
        for(auto &it:root->children){
            ans=max(ans,1+dfs(it) );
        }
        return ans;
    }  
    int maxDepth(Node* root) { 
        return dfs(root); 
    } 
}; 
```
