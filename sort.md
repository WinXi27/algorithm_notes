# 选择排序
```cpp
//每一次把第一个元素当作最小值下标,找到后面比他还要小的值下标,交换
void select_sort(vector<int>&nums){
    for(int i=0;i<nums.size();++i){
        int min_idx=i;
        for(int j=i+1;j<nums.size();++j){
            if(nums[min_idx]>nums[j])min_idx=j;
        }
        swap(nums[min_idx],nums[i]);
    }return ;
}
```
# 冒泡排序
```cpp
void bubble_sort(vector<int>&nums){
    for(int i=0;i<nums.size()-1;++i){
        for(int j=0;j<nums.size()-i-1;++j){
            if(nums[j]>nums[j+1])swap(nums[j],nums[j+1]);
        }
    }return ;
}
```
# 归并排序
```cpp
void merge_sort(vector<int>&nums,int l,int r){
    if(l>=r)return ;
    int mid=l+(r-l)/2;
    //[l,mid],[mid+1,r]
    merge_sort(nums,l,mid);
    merge_sort(nums,mid+1,r);
    vector<int>temp;
    int p=l;
    int q=mid+1;
    while(p<=mid&&q<=r){
        if(nums[p]<nums[q]){
            temp.push_back(nums[p++]);
        }else{
            temp.push_back(nums[q++]);
        }
    }
    while(p<=mid)temp.push_back(nums[p++]);
    while(q<=r)temp.push_back(nums[q++]);
    int idx=0;
    for(int i=l;i<=r;++i){
        nums[i]=temp[idx++];
    }
    return ;
}
```
# 堆排序
```cpp
void down(vector<int>&nums,int i,int size){
    //      i
    //i*2+1  i*2+2
    while(i<size ){
        int maxx=i;
        if(i*2+1<size&&nums[maxx]<nums[i*2+1]){
            maxx=i*2+1;
        }
        if(i*2+2<size&&nums[maxx]<nums[i*2+2]){
            maxx=i*2+2;
        }
        if(maxx!=i){
            swap(nums[maxx],nums[i]);
            i=maxx;
        }else break;
    }return ;
}
void heap_sort(vector<int>&nums){
    for(int i=nums.size()/2-1;i>=0;--i){
        down(nums,i,nums.size() );
    }
    for(int i=nums.size()-1;i>0;--i){
        swap(nums[i],nums[0]);
        down(nums,0,i);
    }return ;
}

```
