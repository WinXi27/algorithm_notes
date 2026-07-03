# 下面是基础题
# 1456. 定长子串中元音的最大数目
```cpp
class Solution {
public:
    int maxVowels(string s, int k) {
        int l=0;
        int maxx=0;
        int cnt=0;
        for(int r=0;r<s.size();++r){
            if(s[r]=='a'||s[r]=='e'||s[r]=='i'||s[r]=='o'||s[r]=='u')
                cnt++;
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            maxx=max(maxx,cnt);
            if(s[l]=='a'||s[l]=='e'||s[l]=='i'||s[l]=='o'||s[l]=='u')
                cnt--;
            l++;
        }
        return maxx;
    }
};
```
# 643. 子数组最大平均数 I
```cpp
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int l=0;
        int sum=0;
        double maxx=INT_MIN;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            maxx=max(maxx,(double)sum/k);
            sum-=nums[l++];
        }
        return maxx;
    }
};
```
# 1343. 大小为 K 且平均值大于等于阈值的子数组数目
```cpp
class Solution {
public:
    int numOfSubarrays(vector<int>& arr, int k, int threshold) {
        int l=0;
        int cnt=0;
        int sum=0;
        for(int r=0;r<arr.size();++r){
            sum+=arr[r];
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            if((double)sum/k>=threshold)cnt++;
            sum-=arr[l++];
        }
        return cnt;
    }
};
```
# 2090. 半径为 k 的子数组平均值
```cpp
class Solution {
public:
    vector<int> getAverages(vector<int>& nums, int k) {
        vector<int>ans(nums.size(),-1);
        int len=2*k+1;
        int l=0;
        double sum=0;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];
            int cur_len=r-l+1;
            if(cur_len<len)continue;
            ans[l+k]=sum/len;
            sum-=nums[l++];
        }
        return ans;
    }
};
```
# 2379. 得到 K 个黑块的最少涂色次数
```cpp
class Solution {
public:
    int minimumRecolors(string blocks, int k) {
        int l=0;
        int cntW=0;
        int minn=blocks.size();
        for(int r=0;r<blocks.size();++r){
            if(blocks[r]=='W')cntW++;
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            minn=min(minn,cntW);
            if(blocks[l]=='W')cntW--;
            l++;
        }
        return minn;
    }
};
```
# 2841. 几乎唯一子数组的最大和
```cpp
class Solution {
public:
#define ll long long 
    long long maxSum(vector<int>& nums, int m, int k) {
        ll sum=0;
        unordered_map<int,int>mp;
        ll maxx=0;
        int l=0;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];   
            mp[nums[r]]++;
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            if(mp.size()>=m)maxx=max(maxx,sum);
            mp[nums[l]]--;
            if(mp[nums[l]]==0)mp.erase(nums[l]);
            sum-=nums[l++];
        }
        return maxx;
    }
};
```
# 2461. 长度为 K 子数组中的最大和
```cpp
class Solution {
public:
#define ll long long 
    long long maximumSubarraySum(vector<int>& nums, int k) {
        ll sum=0;
        int l=0;
        ll maxx=0;
        unordered_map<int,int>mp;
        for(int r=0;r<nums.size();++r){
            mp[nums[r]]++;
            sum+=nums[r];
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            if(mp.size()==k)maxx=max(maxx,sum);        
            sum-=nums[l];
            mp[nums[l]]--;
            if(mp[nums[l]]==0)mp.erase(nums[l]);
            l++;
        }
        return maxx;
    }
};
```
# 1423. 可获得的最大点数
```cpp
class Solution {
public:
    int maxScore(vector<int>& cardPoints, int k) {
        int len=cardPoints.size()-k;
        int sum=0;
        int s=0;
        for(int &it:cardPoints)s+=it;
        if(len==0)return s;
        int minn=s;
        int l=0;
        for(int r=0;r<cardPoints.size();++r){
            sum+=cardPoints[r];
            int cur_len=r-l+1;
            if(cur_len<len)continue;
            minn=min(minn,sum);
            sum-=cardPoints[l++];
        }
        return s-minn;
    }
};
```
# 1176. 健身计划评估
```cpp
class Solution {
public:
    int dietPlanPerformance(vector<int>& calories, int k, int lower, int upper) {
        int sum=0;
        int l=0;
        int score=0;
        for(int r=0;r<calories.size();++r){
            sum+=calories[r];
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            if(sum<lower)score--;
            else if(sum>upper)score++;
            sum-=calories[l++];
        }
        return score;
    }
};
```
# 1100. 长度为 K 的无重复字符子串
```cpp
class Solution {
public:
    int numKLenSubstrNoRepeats(string s, int k) {
        int cnt=0;
        int l=0;
        unordered_map<int,int>mp;
        for(int r=0;r<s.size();++r){
            mp[s[r]]++;
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            if(mp.size()==k)cnt++;
            mp[s[l]]--;
            if(mp[s[l]]==0)mp.erase(s[l]);
            l++;
        }
        return cnt;
    }
};
```
# 1852. 每个子数组的数字种类数
```cpp
class Solution {
public:
    vector<int> distinctNumbers(vector<int>& nums, int k) {
        vector<int>ans;
        int l=0;
        unordered_map<int,int>mp;
        for(int r=0;r<nums.size();++r){
            mp[nums[r]]++;
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            ans.push_back(mp.size() );
            mp[nums[l]]--;
            if(mp[nums[l]]==0)mp.erase(nums[l]);
            l++;
        }
        return ans;
    }
};
```
# 1151. 最少交换次数来组合所有的 1
```cpp
class Solution {
public:
    int minSwaps(vector<int>& data) {
        int len=0;
        for(int &it:data)if(it==1)len++;
        if(len==0)return 0;
        if(len==data.size() )return 0;
        int l=0;
        int cnt0=0;
        int minn=INT_MAX;
        for(int r=0;r<data.size();++r){
            if(data[r]==0)cnt0++;
            int cur_len=r-l+1;
            if(cur_len<len)continue;
            minn=min(minn,cnt0);
            if(data[l++]==0)cnt0--;
        }
        return minn;
    }
};
```
# 2107. 分享 K 个糖果后独特口味的数量
```cpp
class Solution {
public:
    int shareCandies(vector<int>& candies, int k) {
        unordered_map<int,int>mp;
        for(int &it:candies)mp[it]++;
        int maxx=0;
        int l=0;
        if(k==0)return mp.size();
        for(int r=0;r<candies.size();++r){
            mp[candies[r]]--;
            if(mp[candies[r]]==0)mp.erase(candies[r]);
            int cur_len=r-l+1;
            if(cur_len<k)continue;
            maxx=max(maxx,(int)mp.size() );
            mp[candies[l++]]++;
        }
        return maxx;
    }
};
```
# 下面是进阶题目
# 1052. 爱生气的书店老板
```cpp
class Solution {
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) {
        int temp=0;
        for(int i=0;i<customers.size();++i){
            if(grumpy[i]==0)temp+=customers[i];
        }
        int l=0;
        int sum=0;
        int maxx=0;
        for(int r=0;r<customers.size();++r){
            if(grumpy[r]==1)sum+=customers[r];
            int cur_len=r-l+1;
            if(cur_len<minutes)continue;
            maxx=max(maxx,sum);
            if(grumpy[l]==1)sum-=customers[l];
            l++;
        }
        return temp+maxx;
    }
};
```
# 
```cpp

```
# 
```cpp

```