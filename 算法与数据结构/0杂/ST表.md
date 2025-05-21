## 作用
O(1)效率查询区间最大或最小值

## 构成
1. 准备工作
```C++
   int N=2e5+5,ma_level=22;//数组最大长度 和 最大跳跃距离次方(2^ma_level)
   int mi_st[N][level+1]//存储ST表
```
**注意：mi_st[i][j]表示i~i+2^level-1**

2. 建表<br>
```C++
   void build()
   {
    // 初始化ST表第0层（单个元素）
    for (int i = 1; i <= n; i++){
        cin >> mi_st[i][0];        
    }
    // 逐层构建ST表（level表示2^level长度的区间）
    for (int level = 1; level <= 21; level++){
        for (int st = 1; st + (1 << level) - 1 <= n; st++){
            int mid = st + (1 << (level - 1)); // 计算中间分割点 注意：level-1
            mi_st[st][level] = min(mi_st[st][level - 1], mi_st[mid][level - 1]);//合并
        }
      }
   }
```
-  问：为什么遍历到st+(1 << level)-1? <br>
   答：因为"(1 << level)-1"由ma_st[mid][level-1]去搞定了。通过中间点mid <br>
-  问：合并过程原理？<br>答：mi_st[st][level - 1]左半区间：[st, st+2^(level-1)-1]为[st][level]\(以st为起点的长度为2^level的区间)的左边一半(注意level-1),mid则又是左半的起点，后面就不必多说了，同理。
3. 查询
```C++
void querry(int l, int r){
    int len = r - l + 1;   // 区间长度
    int level = log2(len); // 计算最大的level使得2^level <= len
    mi_sum = min(mi_st[l][level],mi_st[r - (1 << level) + 1][level]);
    // 前半部分：[l, l+2^level-1]
    // 后半部分：[r-2^level+1, r]
}
```
- 问：为什么这个能够刚好覆盖所查询的区间？<br>
  答：mi_st[r-(1<< level)+1]\[level]意为：(r−2^level+1)+(2^level−1)=r