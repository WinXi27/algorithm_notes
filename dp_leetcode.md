# 动态规划 专题
保证代码全部AC
# 5. 最长回文子串
## dp 解法
```cpp
class Solution {
public:
    bool dp[1005][1005];
    string longestPalindrome(string s) {
        for(int i=0;i<s.size();++i)dp[i][i]=true;
        string ans=s.substr(0,1);
        for(int len=2;len<=s.size();++len){
            for(int i=0;i<=s.size()-len;++i){
                int j=i+len-1;
                if(s[i]==s[j]){
                    if(len==2)dp[i][j]=true;
                    else dp[i][j]=dp[i+1][j-1];
                }else dp[i][j]=false;
                if(dp[i][j]&&ans.size()<len){
                    ans=s.substr(i,len);
                }
            }
        }
        return ans;
    }
};
```
## 中心拓展 解法
```cpp
class Solution {
public:
    string ans;
    void expand_around_center(string s,int i,int j){
        while(i>=0&&j<s.size()&&s[i]==s[j])i--,j++;
        if(j-i-1>ans.size() )ans=s.substr(i+1,j-i-1);
        return ;
    }
    string longestPalindrome(string s) {
        if(s.empty() )return "";
        for(int i=0;i<s.size();++i){
            expand_around_center(s,i,i);
            expand_around_center(s,i,i+1);
        }
        return ans;
    }
};
```
# LCR 089. 打家劫舍
## dp 解法
```cpp
class Solution {
public:
    int dp[105];//dp[i]定义为前i个[0,i]范围上的最大值
    int rob(vector<int>& nums) {
        for(int i=0;i<nums.size();++i){
            if(i==0)dp[i]=nums[i];
            else dp[i]=max(dp[i-1],(i-2>=0?dp[i-2]:0)+nums[i]);
        }
        return dp[nums.size()-1];
    }
};
```
## dp+空间压缩 解法
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int prepre=0;
        int pre=0;
        for(int i=0;i<nums.size();++i){ 
            int cur=max(pre,prepre+nums[i]);
            prepre=pre;
            pre=cur;
        }
        return pre;
    }
};
```
# LCR 090. 打家劫舍 II
## dp 解法
```cpp
class Solution {
public:
    int head[1004];
    int tail[1004];
    int rob(vector<int>& nums) {
        if(nums.empty() )return 0;
        int maxx=nums[0];
        for(int i=0;i<nums.size()-1;++i){
            if(i==0)head[i]=nums[i];
            else head[i]=max( (i-2>=0?head[i-2]:0)+nums[i],head[i-1]);
            maxx=max(maxx,head[i]);
        }
        for(int i=1;i<nums.size();++i){
            if(i==1)tail[i]=nums[i];
            else{
                tail[i]=max( (i-2>=1?tail[i-2]:0)+nums[i],tail[i-1]);
            }
            maxx=max(tail[i],maxx);
        }
        return maxx;
    }
};
```
## dp+状态压缩 解法
```cpp
class Solution {
public:
    int rob1(vector<int>&nums,int begin,int end){
        int prepre=0;
        int pre=0;
        for(int i=begin;i<end;++i){
            int cur=max(prepre+nums[i],pre);
            prepre=pre;
            pre=cur;
        }return pre;
    }
    int rob(vector<int>& nums) {
        if(nums.empty() )return 0;
        return max({nums[0],rob1(nums,0,nums.size()-1),rob1(nums,1,nums.size() ) });
    }
};
```
# 300. 最长递增子序列
```cpp
class Solution {
public:
    int dp[2555];
    int lengthOfLIS(vector<int>& nums) {
        if(nums.empty() )return 0;
        int maxx=0;
        for(int i=0;i<nums.size();++i){
            dp[i]=1;
            for(int j=0;j<i;++j){
                if(nums[j]<nums[i])dp[i]=max(dp[i],dp[j]+1);
            }
            maxx=max(maxx,dp[i]);
        }
        return maxx;
    }
};
```
# LCR 112. 矩阵中的最长递增路径
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
        if(matrix.empty() )return 0;
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
# 673. 最长递增子序列的个数
```cpp
class Solution {
public:
    int dp[2005];
    int cnt[2005];
    int findNumberOfLIS(vector<int>& nums) {
        if(nums.empty() )return 0;
        int max_cnt=0;
        int maxx=0;
        for(int i=0;i<nums.size();++i){
            dp[i]=1;
            cnt[i]=1;
            for(int j=0;j<i;++j){
                if(nums[j]<nums[i]){
                    if(dp[i]==dp[j]+1){
                        cnt[i]+=cnt[j];
                    }else if(dp[i]<dp[j]+1){
                        cnt[i]=cnt[j];
                        dp[i]=dp[j]+1;
                    }
                }
            }
            if(maxx<dp[i]){
                max_cnt=cnt[i];
                maxx=dp[i];
            }else if(maxx==dp[i]){
                max_cnt+=cnt[i];
            }
        }
        return max_cnt;
    }
};
```
# LCR 095. 最长公共子序列
```cpp
class Solution {
public:
    int dp[1005][1005];
    int longestCommonSubsequence(string text1, string text2) {
        if(text1.empty()||text2.empty() )return 0;
        for(int i=1;i<=text1.size();++i){
            for(int j=1;j<=text2.size();++j){
                //text1 [0,i-1]
                //text2 [0,j-1]
                if(text1[i-1]==text2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
                }
            }
        }
        return dp[text1.size()][text2.size()];
    }
};
```
# 32. 最长有效括号
```cpp
class Solution {
public:
    int dp[30005];//dp[i]定义为[0,i]最长有效括号长度
    int longestValidParentheses(string s) {
        int maxx=0;
        for(int i=0;i<s.size();++i){
            if(s[i]=='(')dp[i]=0;
            else{
                //[    ,i-1,i]
                int len=(i-1>=0?dp[i-1]:0);
                int j=i-1-len;
                if(j>=0&&s[j]=='('){
                    dp[i]=(i-1>=0?dp[i-1]:0)+2+(j-1>=0?dp[j-1]:0);
                }
            }
            maxx=max(maxx,dp[i]);
        }
        return maxx;
    }
};
```
# 718. 最长重复子数组
```cpp
class Solution {
public:
    int dp[1005][1005];
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        if(nums1.empty()||nums2.empty() )return 0;
        int maxx=0;
        for(int i=1;i<=nums1.size();++i){
            for(int j=1;j<=nums2.size();++j){
                if(nums1[i-1]==nums2[j-1]){
                    dp[i][j]=dp[i-1][j-1]+1;
                }else{
                    dp[i][j]=0;
                }
                maxx=max(maxx,dp[i][j]);
            }
        }
        return maxx;
    }
};
```
# 845. 数组中的最长山脉
```cpp
class Solution {
public:
    int peak(vector<int>&arr,int center){
        int l=center;
        int r=center;
        while(l-1>=0&&arr[l-1]<arr[l])l--;
        while(r+1<arr.size()&&arr[r]>arr[r+1])r++;
        return (l!=center&&r!=center&&r-l+1>=3)?r-l+1:0;
    }
    int longestMountain(vector<int>& arr) {
        if(arr.size()<3)return 0;
        int maxx=0;
        for(int i=1;i<=arr.size()-2;++i){
            maxx=max(maxx,peak(arr,i) );
        }
        return maxx;
    }
};
```
# 845. 数组中的最长山脉
## 中心扩展 解法
```cpp
class Solution {
public:
    int expand_around_center(vector<int>&arr,int center){
        int l=center;
        int r=center;
        while(l-1>=0&&arr[l-1]<arr[l])l--;
        while(r+1<arr.size()&&arr[r+1]<arr[r])r++;
        return (r-l+1>=3&&l<center&&center<r)?r-l+1:0;
    }
    int longestMountain(vector<int>& arr) {
        int maxx=0;
        for(int i=1;i<arr.size()-1;++i){
            maxx=max(maxx,expand_around_center(arr,i) );
        }
        return maxx;
    }
};
```
# 1218. 最长定差子序列
## 超时 版本
```cpp
class Solution {
public:
    int dp[100005];
    int longestSubsequence(vector<int>& arr, int dif) {
        if(arr.empty() )return 0;
        int maxx=1;
        dp[0]=1;
        for(int i=1;i<arr.size();++i){
            int j=i-1;
            while(j>=0&&arr[j]+dif!=arr[i])j--;
            if(j>=0)dp[i]=dp[j]+1;
            else dp[i]=1;
            maxx=max(maxx,dp[i]);
        }
        return maxx;
    }
};
```
# 1218. 最长定差子序列
```cpp
class Solution {
public:
    int dp[100005];

    int longestSubsequence(vector<int>& arr, int dif) {
        if(arr.empty() )return 0;
        int maxx=1;
        dp[0]=1;
        unordered_map<int,int>mp;
        mp[arr[0]]=1;
        for(int i=1;i<arr.size();++i){
            int target=arr[i]-dif;
            if(mp.find(target)==mp.end() ){
                dp[i]=1;
            }else{
                dp[i]=mp[target]+1;
            }
            mp[arr[i]]=max(mp[arr[i]],dp[i] );
            maxx=max(maxx,dp[i]);
        }
        return maxx;
    }
};
```
# 121. 买卖股票的最佳时机
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int ans=0;
        int cur_max=0;
        for(int i=prices.size()-1;i>=0;--i){
            cur_max=max(cur_max,prices[i]);
            ans=max(ans,cur_max-prices[i]);
        }
        return ans;
    }
};
```
# 516. 最长回文子序列
```cpp
class Solution {
public:
    int dp[1005][1005];
    int longestPalindromeSubseq(string s) {
        if(s.empty() )return 0;
        int maxx=1;
        for(int i=0;i<s.size();++i)dp[i][i]=1;
        for(int len=2;len<=s.size();++len){
            for(int i=0;i<=s.size()-len;++i){
                int j=i+len-1;
                if(s[i]==s[j]){
                    if(len==2)dp[i][j]=2;
                    else dp[i][j]=dp[i+1][j-1]+2;
                }else{
                    dp[i][j]=max(dp[i+1][j],dp[i][j-1]);
                }
                maxx=max(maxx,dp[i][j]);
            }
        }
        return maxx;
    }
};
```
# 1800. 最大升序子数组和
```cpp
class Solution {
public:
    int maxAscendingSum(vector<int>& nums) {
        int maxx=0;
        for(int i=0;i<nums.size();++i){
            int j=i;
            int temp=nums[i];
            while(j+1<nums.size()&&nums[j]<nums[j+1]){
                j++;
                temp+=nums[j];
            }
            maxx=max(maxx,temp);
            i=j;
        }
        return maxx;
    }
};
```
# 152. 乘积最大子数组
```cpp
class Solution {
public:
    int dp_max[20004];
    int dp_min[20004];
    int maxProduct(vector<int>& nums) {
        if (nums.empty())
            return 0;
        int ans = INT_MIN;
        for (int i = 0; i < nums.size(); ++i) {
            if (i == 0) {
                dp_max[i] = nums[i];
                dp_min[i] = nums[i];
            } else {
                dp_max[i] = max({nums[i], dp_max[i - 1] * nums[i], dp_min[i - 1] * nums[i]});
                dp_min[i] = min({nums[i], dp_min[i - 1] * nums[i], dp_max[i - 1] * nums[i]});
            }
            ans=max({ans,dp_max[i],dp_min[i] });
        }
        return ans;
    }
};
```
# LCR 009. 乘积小于 K 的子数组
```cpp
class Solution {
public:
#define ll long long 
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        if(k<=1)return 0;
        int i=0;
        ll sum=1;
        int cnt=0;
        for(int j=0;j<nums.size();++j){
            sum*=nums[j];
            while(sum>=k){
                sum/=nums[i++];
            }
            int len=j-i+1;
            //[i,j]=j-i+1
            cnt+=j-i+1;
        }
        return cnt;
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
