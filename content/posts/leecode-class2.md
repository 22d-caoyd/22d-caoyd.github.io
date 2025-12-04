---
title: "leecode-class2.md"
date: 2025-12-04
tags: ["leecode","左神", "时间复杂度"]
categories: ["今日所学","算法"]
draft: false
---
### 认识O（NlogN）排序

```cpp
static int proc ess(int[]arr,int L,int R){
    if(L==R){
        return arr[L];
    }
    int mid=L+(R-L)>>1;//求中点，防止溢出，右移比除快
    int left_value=process(arr,L,mid);
    int right_value=process(arr,mid+1,R);
    return Math.max(left_value,right_value);
}
```



