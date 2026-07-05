# 下面是基础题
# 34. 在排序数组中查找元素的第一个和最后一个位置
```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int l=0;//  小于target的数据  l   r 大于等于target的数据
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<target){
                l=mid+1;
            }else{
                r=mid-1;
            }
        }
        if(l>=0&&l<nums.size()&&nums[l]==target){
            int begin=l;
            while(begin+1<nums.size()&&nums[begin]==nums[begin+1])begin++;
            return {l,begin};
        }
        return {-1,-1}; 
    }
};
```
# 35. 搜索插入位置
```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        //小于 l r 大于等于
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<target)l=mid+1;
            else r=mid-1;

        }
        return l;
    }
};
```
# 704. 二分查找
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l=0;
        int r=nums.size()-1;
        //小于 l r 大于等于
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<target)l=mid+1;
            else r=mid-1;
        }
        if(l>=0&&l<nums.size()&&nums[l]==target)return l;
        return -1;
    }
};
```
# 744. 寻找比目标字母大的最小字母
```cpp
class Solution {
public:
    char nextGreatestLetter(vector<char>& letters, char target) {
        //小于等于 l r 大于
        int l=0;
        int r=letters.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(letters[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        if(l>=0&&l<letters.size())return letters[l];
        return letters[0];
    }
};
```
# 2529. 正整数和负整数的最大计数
```cpp
class Solution {
public:
    int maximumCount(vector<int>& nums) {
        //  小于0  l  r  大于等于0
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<0)l=mid+1;
            else r=mid-1;
        }
        int fushu=l;
        while(l<nums.size()&&nums[l]==0)l++;
        return max(fushu,(int)nums.size()-l);
    }
};
```
# 下面是进阶题目
# 1385. 两个数组间的距离值
## 常规 解法
```cpp
class Solution {
public:
    int fun(vector<int>& arr2, int target) {
        int l = 0;
        int r = arr2.size() - 1;
        // 小于等于 l r 大于
        while (l <= r) {
            int mid = l + (r - l) / 2;
            if (arr2[mid] <= target)
                l = mid + 1;
            else
                r = mid - 1;
        }
        return r;
    }
    int findTheDistanceValue(vector<int>& arr1, vector<int>& arr2, int d) {
        sort(arr2.begin(), arr2.end());
        int ans = 0;
        for (int i = 0; i < arr1.size(); ++i) {
            int temp1 = fun(arr2, arr1[i]); // 小于等于
            if (temp1 == -1) {
                if(abs(arr2[temp1+1]-arr1[i])>d)ans++;
            } else {
                if (abs(arr2[temp1] - arr1[i]) > d &&
                    abs(arr2[temp1 + 1] - arr1[i]) > d) {
                    ans++;
                }
            }
        }
        return ans;
    }
};
```
## 添加头尾数据 解法
```cpp
class Solution {
public:
    int findTheDistanceValue(vector<int>& arr1, vector<int>& arr2, int d) {
        int cnt=0;
        int maxx=INT_MIN;
        int minn=INT_MAX;
        for(int &it:arr1)maxx=max(maxx,it),minn=min(minn,it);
        sort(arr2.begin(),arr2.end() );
        arr2.insert(arr2.begin(),minn-d-1);
        arr2.push_back(maxx+d+1);
        for(int i=0;i<arr1.size();++i){
            int idx=lower_bound(arr2.begin(),arr2.end(),arr1[i])-arr2.begin();
            if(arr2[idx]-arr1[i]>d&&arr1[i]-arr2[idx-1]>d)cnt++;
        }
        return cnt;
    }
};
```
# 1170. 比较字符串最小字母出现频次
```cpp
class Solution {
public:
    int fun(string s){
        char minn=s[0];
        int cnt=0;
        for(char &c:s){
            if(c<minn){
                minn=c;
                cnt=1;
            }else if(c==minn)
                cnt++;
        }
        return cnt;
    }
    int find01(vector<int>&nums,int target){
        //小于等于 l r 大于
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            int mid=l+(r-l)/2;
            if(nums[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        return nums.size()-l;
    }
    vector<int> numSmallerByFrequency(vector<string>& queries, vector<string>& words) {
        vector<int>w;
        for(string &s:words){
            w.push_back(fun(s) );
        }
        sort(w.begin(),w.end() );
        vector<int>ans(queries.size() );
        for(int i=0;i<queries.size();++i){
            ans[i]=find01(w,fun(queries[i]) );
        }
        return ans; 
    }
};
```
# 2300. 咒语和药水的成功对数
```cpp
class Solution {
public:
#define ll long long 
    int fun(vector<int>&nums,int target,ll success){
        int l=0;
        int r=nums.size()-1;
        ///小于 l r 大于等于
        while(l<=r){
            int mid=l+(r-l)/2;
            if((ll)nums[mid]*target<success){
                l=mid+1;
            }else r=mid-1;
        }
        return nums.size()-l;
    }
    vector<int> successfulPairs(vector<int>& spells, vector<int>& potions, long long success) {
        vector<int>ans(spells.size() );
        sort(potions.begin(),potions.end() );
        for(int i=0;i<spells.size();++i){
            ans[i]=fun(potions,spells[i],success);
        }
        return ans;
    }
};
```
# 2389. 和有限的最长子序列
```cpp
class Solution {
public:
    int fun(vector<int>&nums,int target){
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            //小于等于 l r 大于
            int mid=l+(r-l)/2;
            if(nums[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        return r+1;
    }
    vector<int> answerQueries(vector<int>& nums, vector<int>& queries) {
        sort(nums.begin(),nums.end() );
        vector<int>ans(queries.size() );
        for(int i=1;i<nums.size();++i){
            nums[i]+=nums[i-1];
        }
        for(int i=0;i<queries.size();++i){
            int target=queries[i];
            ans[i]=fun(nums,target);
        }
        return ans;
    }
};
```
# 2080. 区间内查询数字的频率
```cpp
class RangeFreqQuery {
public:
    unordered_map<int,vector<int>>idx;
    RangeFreqQuery(vector<int>& arr) {
        for(int i=0;i<arr.size();++i){
            idx[arr[i]].push_back(i);
        }
    }
    int fun01(vector<int>&nums,int target){
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            //小于 l r 大于等于
            int mid=l+(r-l)/2;
            if(nums[mid]<target)l=mid+1;
            else r=mid-1;
        }
        return l;
    }
    int fun02(vector<int>&nums,int target){
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            //小于等于 l r 大于
            int mid=l+(r-l)/2;
            if(nums[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        return r;
    }
    int query(int left, int right, int value) {
        if(idx.find(value)==idx.end() )return 0;
        vector<int>&nums=idx[value];
        int begin=fun01(nums,left);//大于等于
        int end=fun02(nums,right);//小于等于
        return end-begin+1;
    }
};
```
# 3488. 距离最小相等元素查询
```cpp
class Solution {
public:
    unordered_map<int,vector<int>>idx;
    vector<int> solveQueries(vector<int>& nums, vector<int>& queries) { 
        for(int i=0;i<nums.size();++i){ 
            idx[nums[i]].push_back(i); 
        }  
        for(auto &[_,pos]:idx){ 
            int temp=pos[0]; 
            pos.insert(pos.begin(),pos.back()-nums.size() ); 
            pos.push_back(temp+nums.size() ); 
        } 
        vector<int>ans(queries.size(),0); 
        for(int i=0;i<queries.size();++i){ 
            int target=nums[queries[i]]; 
            if(idx.find(target)==idx.end()||idx[target].size()==3){
                ans[i]=-1;
                continue;
            }
            vector<int>&nums=idx[target]; 
            int begin=(int)(lower_bound(nums.begin(),nums.end(),queries[i])-nums.begin() );
            //  第一个等于的数据 
            ans[i]=min( queries[i]-nums[begin-1],nums[begin+1]-queries[i] );
        }   
        return ans;   
    }
};
```
# 2563. 统计公平数对的数目
```cpp
class Solution {
public:
#define ll  long long 
    int fun01(vector<int>&nums,int begin,int end,int target){
        //大于等于target的数据下标
        int l=begin;
        int r=end;
        while(l<=r){
            //小于 l r 大于=
            int mid=l+(r-l)/2;
            if(nums[mid]<target)l=mid+1;
            else r=mid-1;
        }
        return l;
    }
    int fun02(vector<int>&nums,int begin,int end,int target){
        //小于等于target的数据下标
        int l=begin;
        int r=end;
        while(l<=r){
            //小于= l r 大于
            int mid=l+(r-l)/2;
            if(nums[mid]<=target)l=mid+1;
            else r=mid-1;
        }
        return r;
    }
    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        ll cnt=0;
        sort(nums.begin(),nums.end() );
        for(int i=0;i<nums.size();++i){
            //[i+1,nums.size()-1]
            int temp1=lower-nums[i];
            int temp2=upper-nums[i];
            cnt+=(fun02(nums,i+1,nums.size()-1,temp2)-fun01(nums,i+1,nums.size()-1,temp1)+1 );
        }
        return cnt;
    }
};
```
# 1146. 快照数组
```cpp
class SnapshotArray {
public:
    unordered_map<int,vector<pair<int,int>> >mp;
    int cur_snap_id=0;
    SnapshotArray(int length) {
        for(int i=0;i<length;++i)mp[i].push_back({cur_snap_id,0});
    }
    void set(int index, int val) {
        mp[index].push_back({cur_snap_id,val});
    }
    int snap() {
        cur_snap_id++;
        return cur_snap_id-1;
    }
    int get(int index, int snap_id) {
        vector<pair<int,int>>&nums=mp[index];
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            //小于等于 l r 大于
            int mid=l+(r-l)/2;
            if(nums[mid].first<=snap_id)l=mid+1;
            else r=mid-1;
        }
        return nums[r].second;
    }
};
```
# 981. 基于时间的键值存储
```cpp
class TimeMap {
public:
    unordered_map<string,vector<pair<string,int>>>idx;
    TimeMap() {
    }
    void set(string key, string value, int timestamp) {
        idx[key].push_back({value,timestamp});
    }
    string get(string key, int timestamp) {
        vector<pair<string,int>>&nums=idx[key];
        int l=0;
        int r=nums.size()-1;
        while(l<=r){
            //小于等于 l r 大于
            int mid=l+(r-l)/2;
            if(nums[mid].second<=timestamp)
                l=mid+1;
            else r=mid-1; 
        }
        return r!=-1?nums[r].first:"";
    }
};
```
# 658. 找到 K 个最接近的元素
```cpp
class Solution {
public:
    vector<int> findClosestElements(vector<int>& nums, int k, int x) {
        sort(nums.begin(),nums.end() );
        int r=upper_bound(nums.begin(),nums.end(),x)-nums.begin() ;//大于等于
        if(r==0){
            return vector<int>(nums.begin(),nums.begin()+k);
        }else if(r==nums.size() ){
            return vector<int>(nums.begin()+(int)nums.size()-k,nums.end() );
        }else{
            int l=r-1;
            vector<int>ans;
            while(l>=0&&r<nums.size()&&ans.size()<k ){
                if(x-nums[l]<=nums[r]-x)ans.push_back(nums[l--]);
                else ans.push_back(nums[r++]);
            } 
            while(ans.size()<k&&l>=0){
                ans.push_back(nums[l--]);
            }
            while(ans.size()<k&&r<nums.size() ){
                ans.push_back(nums[r++]);
            }
            sort(ans.begin(),ans.end() );
            return ans;
        }
        return {};
    }
};
```
# 1818. 绝对差值和
```cpp
class Solution {
public:
#define ll long long 
const ll mod=1e9+7;
    int minAbsoluteSumDiff(vector<int>& nums1, vector<int>& nums2) {
        if(nums1==nums2)return 0;
        vector<int>nums=nums1;
        sort(nums.begin(),nums.end() );
        ll sum=0;
        for(int i=0;i<nums1.size();++i)
            sum=(sum+llabs(nums1[i]-nums2[i])+mod)%mod;
        ll maxx=0;
        for(int i=0;i<nums.size();++i){
            int r=lower_bound(nums.begin(),nums.end(),nums2[i])-nums.begin();
            ll dif=abs(nums1[i]-nums2[i]);
            if(r==0){
                maxx=max(maxx,dif-(nums[r]-nums2[i]) );
            }else if(r==nums.size() ){
                maxx=max(maxx,dif-(nums2[i]-nums[r-1]) );
            }else maxx=max({maxx,dif-nums2[i]+nums[r-1],dif-nums[r]+nums2[i] });
        }
        return (sum-maxx+mod)%mod;
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