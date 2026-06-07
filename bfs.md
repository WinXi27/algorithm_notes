# bfs的代码模板
```cpp
// leetCode102二叉树层序遍历
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        if(root==nullptr)return {};
        vector<vector<int>>ans;
        queue<TreeNode*>que;
        que.push(root);
        while(!que.empty() ){
            int size=que.size();
            vector<int>temp;
            while(size--){
                TreeNode*cur=que.front();
                que.pop();
                temp.push_back(cur->val);
                if(cur->left)que.push(cur->left);
                if(cur->right)que.push(cur->right);
            }
            ans.push_back(temp);
        }
        return ans;
    }
};
```