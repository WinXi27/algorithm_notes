# 1456. 定长子串中元音的最大数目
```cpp
class Solution {
public:
    bool is_valid(char &c){
        return c=='a'||c=='e'||c=='i'||c=='o'||c=='u';
    }
    int maxVowels(string s, int k) {
        int len=k;
        int l=0;
        int maxx=0;
        int cnt=0;
        for(int r=0;r<s.size();++r){
            if(is_valid(s[r]) )cnt++;
            if(r-l+1<len)continue;
            maxx=max(maxx,cnt);
            if(is_valid(s[l]) )cnt--;
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
        int max_sum=INT_MIN;
        int sum=0;
        int l=0;
        int len=k;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];
            if(r-l+1<len)continue;
            max_sum=max(max_sum,sum);
            sum-=nums[l];
            l++;
        }
        return (double)max_sum/len;
    }
};
```
# 1343. 大小为 K 且平均值大于等于阈值的子数组数目
```cpp
class Solution {
public:
    int numOfSubarrays(vector<int>& arr, int k, int threshold) {
        int sum=0;
        int l=0;
        int ans=0;
        for(int r=0;r<arr.size();++r){
            sum+=arr[r];
            if(r-l+1<k)continue;
            if((double)sum/k>=threshold)ans++;
            sum-=arr[l++];
        }
        return ans;
    }
};
```
# 2090. 半径为 k 的子数组平均值
```cpp
class Solution {
public:
    vector<int> getAverages(vector<int>& nums, int k) {
        int len=k*2+1;
        int l=0;
        vector<int>ans(nums.size(),-1);
        long long sum=0;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];
            if(r-l+1<len)continue;
            ans[r-k]=(double)sum/len;
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
        int cntW=0;
        int minn=blocks.size();
        int l=0;
        for(int r=0;r<blocks.size();++r){
            if(blocks[r]=='W')cntW++;
            if(r-l+1<k)continue;
            minn=min(minn,cntW);
            if(blocks[l++]=='W')cntW--;
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
        ll maxx=0;
        ll sum=0;
        unordered_map<int,int>mp;
        int l=0;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];
            mp[nums[r]]++;
            if(r-l+1<k)continue;
            if(mp.size()>=m)maxx=max(maxx,sum);
            sum-=nums[l];
            mp[nums[l]]--;
            if(mp[nums[l]]==0)mp.erase(nums[l]);
            l++;
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
        ll maxx=0;
        ll sum=0;
        int l=0;
        unordered_map<int,int>mp;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];
            mp[nums[r]]++;
            if(r-l+1<k)continue;
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
#define ll long long 
    int maxScore(vector<int>& cardPoints, int k) {
        int len=cardPoints.size()-k;
        ll min_sum=INT_MAX;
        ll sum=0;
        ll Sum=0;
        int l=0;
        for(int r=0;r<cardPoints.size();++r){
            Sum+=cardPoints[r];
            sum+=cardPoints[r];
            if(r-l+1<len)continue;
            min_sum=min(min_sum,sum);
            sum-=cardPoints[l++];
        }
        return len==0?Sum:Sum-min_sum;
    }
};
```
# 1176. 健身计划评估
```cpp
class Solution {
public:
    int dietPlanPerformance(vector<int>& nums, int k, int lower, int upper) {
        int score=0;
        int l=0;
        int sum=0;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];
            if(r-l+1<k)continue;
            if(sum<lower)score--;
            else if(sum>upper)score++;
            sum-=nums[l++];
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
        unordered_map<char,int>mp;
        int l=0;
        for(int r=0;r<s.size();++r){
            mp[s[r]]++;
            if(r-l+1<k)continue;
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
            if(r-l+1<k)continue;
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
        int minn=data.size();
        int l=0;
        int cnt0=0;
        for(int r=0;r<data.size();++r){
            if(data[r]==0)cnt0++;
            if(r-l+1<len)continue;
            minn=min(minn,cnt0);
            if(data[l]==0)cnt0--;
            l++;
        }
        return minn;
    }
};
```
# 2107. 分享 K 个糖果后独特口味的数量
```cpp
class Solution {
public:
    int shareCandies(vector<int>& nums, int k) {
        unordered_map<int,int>mp;
        for(int &it:nums)mp[it]++;
        if(k==0)return mp.size();
        int maxx=0;
        int l=0;
        for(int r=0;r<nums.size();++r){
            mp[nums[r]]--;
            if(mp[nums[r]]==0)mp.erase(nums[r]);
            if(r-l+1<k)continue;
            maxx=max(maxx,(int)mp.size() );
            mp[nums[l++]]++;
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