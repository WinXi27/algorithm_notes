# disjoint_set_union
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







