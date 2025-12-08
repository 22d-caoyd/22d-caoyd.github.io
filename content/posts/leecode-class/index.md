---
title: "leecode-class2"
date: 2025-12-04
tags: ["leecode","左神","时间复杂度","归并排序"]
categories: ["今日所学","算法"]
draft: false
math: true
---

### 认识O（NlogN）排序

#### 二分法
- 代码实现
```cpp
static int process(int[]arr,int L,int R){
    if(L==R){
        return arr[L];
    }
    int mid=L+(R-L)>>1;//求中点，防止溢出，右移比除2快
    int left_value=process(arr,L,mid);
    int right_value=process(arr,mid+1,R);
    return Math.max(left_value,right_value);
}
```
- master公式求解时间复杂度
![master](20251204-153133.png)
    - 此程序下：a=2 b=2 d=0也即 T(n)=2*T(n/2)+O(1)
    - 只要满足子程序调用规模相等就可以直接求解时间复杂度
![O(time)](20251204-154647.png)

#### 归并排序
- 通俗：先让左侧排序，再右侧排序，再归并在一起；相当于两个有序的序列合并，谁小谁拷贝进解果
- 代码实现
```cpp
static void process(int[]arr,int L,int R){
    if(L==R){
        return ;
    }
    int mid=L+(R-L)>>1;//求中点，防止溢出，右移比除2快
    process(arr,L,mid);
    process(arr,mid+1,right);
    merge(arr,L,mid,R);
}
static void merge(int[] arr,int L,int mid,int R){
    int[] help=new int[R-L+1];
    int i=0;
    int p1=L;
    int p2=mid+1;
    while(p1<=mid && p2<=right){
        help[i++]= arr[p1]<=arr[p2] ? arr[p1++] :arr[p2++];
    }
    while(p1<=mid){
       help[i++]=arr[p1++]; 
    }
    while(p2<=right){
        help[i++]=arr[p2++];
    }
    for(int i=0;i<=help.length(),i++) arr[L+i]=help[i];
}
```
 - master公式中：a=2 b=2 d=1     log2(2)=1
 - 时间复杂度：O(NlogN)
 - 运用外排序：放在另一个数组再拷贝回来

#### O($N^2$)为什么差？（冒泡、比较）
- 浪费大量比较行为，才放置好一个数 

#### 小和问题
- 暴力解法:O($N^2$)
- merge法
  - [1,3,4,2,5]
  - 左侧有多少数比现在的数小->求右边有多少数比现在的数大 
  - 代码实现
```cpp
static int smallsum(int[] arr){
    if(arr==null||arr.length()<2)
    return 0;
return process(arr,0,arr.length()-1);
}

static int process(int[]arr,int L,int R){
    if(L==R){
        return 0;
    }
    int mid=L+(R-L)>>1;//求中点，防止溢出，右移比除2快
    return process(arr,L,mid)+process(arr,mid+1,right)+merge(arr,L,mid,R);//左边产生右边产生，merge的时候也产生
}
//既排序又求小和，一定要排序
static int merge(int[] arr,int L,int mid,int R){
    int[] help=new int[R-L+1];
    int i=0;
    int p1=L;
    int p2=mid+1;
    int res=0;
    while(p1<=mid && p2<=right){
        res+= arr[p1]<arr[p2] ? (r-p2+1)*arr[p1] : 0;//只有左小于右时求小和；个数*当前数
        help[i++]= arr[p1]<arr[p2] ? arr[p1++] :arr[p2++];//等于是要先拷贝右组
    }
    while(p1<=mid){
       help[i++]=arr[p1++]; 
    }
    while(p2<=right){
        help[i++]=arr[p2++];
    }
    for(int i=0;i<=help.length(),i++) arr[L+i]=help[i];
    return res;
}
```

#### 荷兰国旗问题
- 数字分区：给定数组与数字，以该数字分区左边是小于区，右边是大于区
- 升级：小于、等于、大于区严格分开
![荷兰国旗](20251204-174919.png)

#### 快排
- 每次拿该序列的最后的一个元素作为依据排序
- 快排1.0
![qsort1.0](20251204-175202.png)
- 快排2.0
![qsort2.0](20251204-175300.png)
    - 快排2.0比1.0稍快一些，因为一次搞定一批数
- 最坏时间复杂度：O(&N^2&);产生原因：子递归划分太偏，退化成&N^2&
- 快排3.0
  - 随机选取数作为划分值，让好情况&坏情况变为概率事件
  - 代码实现
```cpp
static void quickSort(int[] arr,int L,int R){
    if(L<R>){
        swap(arr,L+(int)(Math.random()*(R-L+1)),R);//Math.random(),生成一个 [0.0, 1.0) 之间的随机浮点数
        int[] p=partition(arr,L,R);
        quickSort(arr,L,p[0]-1);
        quickSort(arr,p[1]+1,R);
    }
}
//L:当前要比较的指针 R:pivot位置
static int[] partition(int[] arr,int L,int R){
    int less=L-1;//小于区的右边界，L一直是当前指针
    int more=R;//大于区的左边界
    while(L<more){
        if(arr[L]<arr[R]){
            swap(arr,++less,L++);//让less区的下一个元素和当前值交换，并扩充less区
        }
        else if(arr[L]>arr[R]){
            swap(arr,--more,L);//注意这里L指针不要自增，因为换过以后的L所指向值还没有比较
        }
        else{
            L++;//值相等时，当前指针自增就行，因为分区是小于区右边界，大于区左边界，中间就是相等区
        }
    }
    swap(arr,more,R);//把基准值放到中间
    return new int[] {less+1,more};//等于区范围是 [less + 1, more]，因为 pivot 最后被放到了 more，所以包含
}
```
