# 分享丨【算法题单】滑动窗口与双指针（定长/不定长/单序列/双序列/三指针/分组循环）

# 下面是____定长滑动窗口___1.1基础
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
# 下面是____定长滑动窗口___1.2进阶
# 1052. 爱生气的书店老板
```cpp
class Solution {
public:
    int maxSatisfied(vector<int>& customers, vector<int>& grumpy, int minutes) {
        int sum=0;
        int maxx=0;
        for(int i=0;i<customers.size();++i)
            if(grumpy[i]==0)
                sum+=customers[i];
        int l=0;
        maxx=sum;
        for(int r=0;r<customers.size();++r){
            if(grumpy[r]==1)sum+=customers[r];
            if(r-l+1<minutes)continue;
            maxx=max(maxx,sum);
            if(grumpy[l]==1)sum-=customers[l];
            l++;
        }
        return maxx;
    }
};
```
# 3679. 使库存平衡的最少丢弃次数
```cpp
class Solution {
public:
    int minArrivalsToDiscard(vector<int>& arrivals, int w, int m) {
        int l=0;
        int drop_cnt=0;
        unordered_map<int,int>mp;
        unordered_map<int,bool>is_drop;
        for(int r=0;r<arrivals.size();++r){
            if(mp[arrivals[r]]==m){
                is_drop[r]=true;
                drop_cnt++;
            }else mp[arrivals[r]]++;
            if(r-l+1<w)continue;
            if(is_drop[l]==false)mp[arrivals[l]]--;
            l++;
        }
        return drop_cnt;
    }
};
```
# 3439. 重新安排会议得到最多空余时间 I
```cpp
class Solution {
public:
    int maxFreeTime(int eventTime, int k, vector<int>& startTime, vector<int>& endTime) {
        vector<int>nums;
        nums.push_back(startTime[0]-0);
        for(int i=0;i+1<startTime.size();++i){
            nums.push_back(startTime[i+1]-endTime[i]);
        }
        nums.push_back(eventTime-endTime.back() );
        int len=k+1;
        int l=0;
        int maxx=0;
        int sum=0;
        for(int r=0;r<nums.size();++r){
            sum+=nums[r];
            if(r-l+1<k+1)continue;
            maxx=max(maxx,sum);
            sum-=nums[l++];
        }
        return maxx;
    }
};
```
# 3694. 删除子字符串后不同的终点
```cpp
class Solution {
public:
    int distinctPoints(string s, int k) {
        int x=0;
        int y=0;
        int l=0;
        unordered_map<string,int>mp;
        for(int r=0;r<s.size();++r){
            if(s[r]=='U')y--;
            else if(s[r]=='D')y++;
            else if(s[r]=='L')x++;
            else if(s[r]=='R')x--;
            if(r-l+1<k)continue;
            mp[to_string(x)+","+to_string(y)]++;
            if(s[l]=='U')y++;
            else if(s[l]=='D')y--;
            else if(s[l]=='L')x--;
            else if(s[l]=='R')x++;
            l++;
        }
        return mp.size();
    }
};
```
# 2134. 最少交换次数来组合所有的 1 II
```cpp
class Solution {
public:
    int minSwaps(vector<int>& nums) {
        int minn=nums.size();
        int l=0;
        int len=0;
        int cnt0=0;
        for(int &it:nums)if(it==1)len++;
        for(int r=0;r<nums.size();++r){
            if(nums[r]==0)cnt0++;
            if(r-l+1<len)continue;
            minn=min(minn,cnt0);
            if(nums[l]==0)cnt0--;
            l++;
        }
        len=nums.size()-len;
        l=0;
        int cnt1=0;
        for(int r=0;r<nums.size();++r){
            if(nums[r]==1)cnt1++;
            if(r-l+1<len)continue;
            minn=min(minn,cnt1);
            if(nums[l]==1)cnt1--;
            l++;
        }
        return minn;
    }
};
```
# 1652. 拆炸弹
```cpp

```
# 
```cpp

```
# 
```cpp

```