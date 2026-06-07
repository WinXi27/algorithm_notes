# dfs模板
```cpp
// leetCode200岛屿数量
class Solution {
public:
    int d[5]={0,-1,0,1,0};
    void dfs(vector<vector<char>>&grid,int i,int j){
        grid[i][j]='0';
        for(int k=0;k<4;++k){
            int x=i+d[k];
            int y=j+d[k+1];
            if(x>=0&&y>=0&&x<grid.size()&&y<grid[0].size()&&grid[x][y]=='1'){
                dfs(grid,x,y);
            }
        }
        return ;
    }
    int numIslands(vector<vector<char>>& grid) {
        if(grid.empty() )return 0;
        int cnt=0;
        for(int i=0;i<grid.size();++i){
            for(int j=0;j<grid[i].size();++j){
                if(grid[i][j]=='1'){
                    dfs(grid,i,j);
                    cnt++;
                }
            }
        }
        return cnt;
    }
};
```