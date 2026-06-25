# 动态规划 专题
保证代码全部AC
# 5. 最长回文子串
## dp 解法
```cpp
class Solution {
public:
    bool dp[1005][1005];
    string longestPalindrome(string s) {
        if(s.empty() )return "";
        string ans=s.substr(0,1);
        for(int i=0;i<s.size();++i)dp[i][i]=true;
        for(int len=2;len<=s.size();++len){
            for(int i=0;i<=s.size()-len;++i){
                int j=i+len-1;
                //[i][j]
                if(s[i]==s[j]){
                    if(len==2)dp[i][j]=true;
                    else dp[i][j]=dp[i+1][j-1];
                }else{
                    dp[i][j]=false;
                }
                if(dp[i][j]&&ans.size()<j-i+1){
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
    }
    string longestPalindrome(string s) {
        if(s.empty() )return  "";
        ans=s.substr(0,1);
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
    int dp[104];
    int rob(vector<int>& nums) {
        int maxx=0;
        for(int i=0;i<nums.size();++i){
            if(i==0)dp[i]=nums[i];
            else dp[i]=max( (i-2>=0?dp[i-2]:0)+nums[i],dp[i-1]);
            maxx=max(maxx,dp[i]);
        }
        return maxx;
    }
};
```
## 更好 解法
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int prepre=0;
        int pre=0;
        int maxx=0;
        for(int i=0;i<nums.size();++i){
            int cur=max(prepre+nums[i],pre);
            maxx=max({maxx,cur,pre,prepre});
            prepre=pre;
            pre=cur;
        }
        return maxx;
    }
};
```
# LCR 090. 打家劫舍 II
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
