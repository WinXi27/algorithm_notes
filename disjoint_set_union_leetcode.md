# leetcode的并查集专题_题目_解法
**代码保证都AC**
# disjoint_set_union模板
```cpp
//并查集是一个集合,初始所有元素自己单独一个集合
vector<int>fa;//fa[i]表示i这个节点的代表节点
vector<int>sz;
void build(int size){
    fa.resize(size,0);
    sz.resize(size,1);
    for(int i=0;i<size;++i)fa[i]=i;
    //初始化时把所有元素的箭头指向自己
}
int find(int x){//find(x)是查找x的代表节点,同时做扁平化操作
    queue<int>que;
    while(fa[x]!=x){
        que.push(x);
        x=fa[x];
    }
    while(!que.empty() ){
        fa[que.front()]=x;
        que.pop();
    }
    return x;
}
void union_element(int x,int y){//合并x和所在的集合
    int fx=find(x);
    int fy=find(y);
    if(fx==fy)return ;//本来就在一个集合中,不用合并了
    fa[fx]=fy;
    sz[fy]+=sz[fx];
    return ;
}
bool is_same(int x,int y){//x和y是否在一个集合
    return find(x)==find(y);
}
```
# 128.最长连续序列
```cpp
class Solution {
public:
    unordered_map<int,int>fa;
    unordered_map<int,int>size;
    int maxx=1;
    int find(int n){
        queue<int>que;
        while(fa[n]!=n){
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
        size[fa2]+=size[fa1];
        maxx=max(maxx,size[fa2]);
    }
    int longestConsecutive(vector<int>& nums) {
        if(nums.empty() )return 0;
        unordered_set<int>st;
        for(int i=0;i<nums.size();++i){
            if(st.find(nums[i])!=st.end() )continue;
            st.insert(nums[i]);
            fa[nums[i]]=nums[i];
            size[nums[i]]=1;
            if(st.find(nums[i]-1)!=st.end() ){
                union_ele(nums[i],nums[i]-1);
            }
            if(st.find(nums[i]+1)!=st.end() ){
                union_ele(nums[i],nums[i]+1);
            }
        }
        return maxx;
    }
};
```
# 261.以图判树
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
        set_cnt--;
        fa[fa1]=fa2;
    }
    bool validTree(int n, vector<vector<int>>& edges) {
        //[0,n-1]    
        build(n);
        for(auto &it:edges){
            int n1=it[0];
            int n2=it[1];
            if(find(n1)!=find(n2) ){
                union_ele(n1,n2);
            }else return false;
        }
        return set_cnt==1;
    }
};
```
# 305.岛屿数量 II
```cpp
class Solution {
public:
    int r=0;
    int c=0;
    int fa[10005];
    unordered_map<int,bool>flag;
    int d[5]={0,-1,0,1,0};
    int set_cnt=0;
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
    vector<int> numIslands2(int m, int n, vector<vector<int>>& positions) {
        r=m;
        c=n;
        //[1,row*col]
        vector<int>ans;
        for(auto &it:positions){
            int i=it[0];
            int j=it[1];
            if(flag[i*c+j+1]){
                ans.push_back(set_cnt);
                continue;
            }
            flag[i*c+j+1]=true;
            set_cnt++;
            fa[i*c+j+1]=i*c+j+1;
            for(int k=0;k<4;++k){
                int x=i+d[k];
                int y=j+d[k+1];
                if(x>=0&&y>=0&&x<r&&y<c&&flag[x*c+y+1]){
                    union_ele(x*c+y+1,i*c+j+1);
                }
            }
            ans.push_back(set_cnt);
        }
        return ans; 
    }
};
```
## 很好的题目,再做一遍
```cpp
class Solution {
public:
    int fa[10005];
    int row=0;
    int col=0;
    int d[5]={0,-1,0,1,0};
    unordered_map<int,bool>flag;
    int set_cnt=0;
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
    vector<int> numIslands2(int m, int n, vector<vector<int>>& positions) {
        //下标从1开始
        row=m;
        col=n;
        vector<int>ans;
        for(auto &it:positions){
            int i=it[0];
            int j=it[1];
            int temp=i*col+j+1;
            if(flag[temp]){
                ans.push_back(set_cnt);
                continue;
            }
            fa[temp]=temp;
            set_cnt++;
            flag[temp]=true;
            for(int k=0;k<4;++k){
                int x=i+d[k];
                int y=j+d[k+1];
                if(x>=0&&y>=0&&x<row&&y<col&&flag[x*col+y+1]){
                    union_ele(temp,x*col+y+1);
                }
            }
            ans.push_back(set_cnt);
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
        //[0,n-1]
        for(int i=0;i<n;++i)fa[i]=i;
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
    int countComponents(int n, vector<vector<int>>& edges) {
        //[0,n-1]
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
# 547.省份数量
```cpp
class Solution {
public:
    int fa[205];
    int set_cnt=0;
    void build(int n){
        //[0,n-1]
        for(int i=0;i<n;++i)fa[i]=i;
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
        build(n);
        for(int i=0;i<n;++i){
            for(int j=0;j<n;++j){
                if(is[i][j]){
                    union_ele(i,j);
                }
            }
        }
        return set_cnt;
    }
};
```
# 684.冗余连接
```cpp
class Solution {
public:
    int fa[10005];
    int set_cnt=0;
    void build(int n){//[1,n]
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
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        if(edges.empty() )return {};
        int n=0;//[1,n]
        for(auto &it:edges){
            n=max({n,it[0],it[1]});
        }
        build(n);
        for(auto &it:edges){
            int n1=it[0];
            int n2=it[1];
            //n1---n2;
            if(find(n1)==find(n2) ){
                return {n1,n2};
            }else{
                union_ele(n1,n2);
            }
        }
        return {};
    }
};
```
# 695.岛屿的最大面积
```cpp
class Solution {
public:
    int r=0;
    int c=0;
    int maxx=0;
    int fa[2600];
    int sz[2600];
    int d[5]={0,-1,0,1,0};
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
        sz[fa2]+=sz[fa1];
        maxx=max(maxx,sz[fa2]);
    }
    bool visit[55][55];
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        if(grid.empty() )return 0;
        r=grid.size();
        c=grid[0].size();
        for(int i=0;i<r;++i){
            for(int j=0;j<c;++j){
                if(grid[i][j]==1){
                    maxx=max(maxx,1);
                    int temp=i*c+j+1;
                    fa[temp]=temp;
                    sz[temp]=1;
                    visit[i][j]=true;
                    for(int k=0;k<4;++k){
                        int x=i+d[k];
                        int y=j+d[k+1];
                        if(x>=0&&y>=0&&x<r&&y<c&&visit[x][y]){
                            union_ele(temp,x*c+y+1);
                        }
                    }
                }
            }
        }
        return maxx;
    }
};
```
# 827.最大人工岛
```cpp
class Solution {
public:
    unordered_map<int,int>cnt;
    int row=0;
    int col=0;
    int maxx=0;
    int d[5]={0,-1,0,1,0};
    bool visit[505][505];
    int dfs(vector<vector<int>>& grid,int i,int j,int idx){
        visit[i][j]=true;
        grid[i][j]=idx;
        int ans=1;
        for(int k=0;k<4;++k){
            int x=i+d[k];
            int y=j+d[k+1];
            if(x>=0&&y>=0&&x<row&&y<col&&grid[x][y]==1&&visit[x][y]==false){
                ans+=dfs(grid,x,y,idx);
            }
        }
        maxx=max(maxx,ans);
        cnt[idx]=ans;
        return ans;
    }
    int largestIsland(vector<vector<int>>& grid) {
        row=grid.size();
        col=grid[0].size();
        int idx=2;
        for(int i=0;i<row;++i){
            for(int j=0;j<col;++j){
                if(grid[i][j]==1){
                    dfs(grid,i,j,idx);
                    idx++;
                }
            }
        }
        for(int i=0;i<row;++i){
            for(int j=0;j<col;++j){
                if(grid[i][j]==0){
                    unordered_set<int>st;
                    int ans=1;
                    for(int k=0;k<4;++k){
                        int x=i+d[k];
                        int y=j+d[k+1];
                        if(x>=0&&y>=0&&x<row&&y<col&&grid[x][y]!=0&&st.find(grid[x][y])==st.end() ){
                            st.insert(grid[x][y]);
                            ans+=cnt[grid[x][y]];
                        }
                    }
                    maxx=max(maxx,ans);
                }
            }
        }
        return maxx;
    }
};
```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```
#
```cpp

```








