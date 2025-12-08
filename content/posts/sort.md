---
title: "排序归纳"
date: 2025-12-08
tags: ["leecode","左神","排序",]
categories: ["今日所学","算法"]
draft: false
math: true
---

### 归并排序 (Merge Sort)
核心思路: 分治法 - 递归分解数组,然后合并有序子数组
```cpp
// 核心模块1: 合并两个有序数组
void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;
    
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j])
            temp[k++] = arr[i++];
        else
            temp[k++] = arr[j++];
    }
    
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    
    for (int i = 0; i < k; i++)
        arr[left + i] = temp[i];
}

// 核心模块2: 递归分解
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);        // 左半部分
        mergeSort(arr, mid + 1, right);   // 右半部分
        merge(arr, left, mid, right);     // 合并
    }
}
```
时间复杂度: O(n log n) | 空间复杂度: O(n) | 稳定排序

### 桶排序 (Bucket Sort)
核心思路: 将数据分配到多个桶中,每个桶分别排序,最后合并
```cpp
void bucketSort(vector<float>& arr) {
    int n = arr.size();
    vector<vector<float>> buckets(n);
    
    // 核心模块1: 分配元素到桶
    for (int i = 0; i < n; i++) {
        int index = n * arr[i];  // 映射函数(假设0-1之间)
        buckets[index].push_back(arr[i]);
    }
    
    // 核心模块2: 对每个桶排序
    for (int i = 0; i < n; i++) {
        sort(buckets[i].begin(), buckets[i].end());
    }
    
    // 核心模块3: 合并桶
    int idx = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < buckets[i].size(); j++) {
            arr[idx++] = buckets[i][j];
        }
    }
}
```
时间复杂度: O(n+k) 平均 | 适用: 均匀分布数据 | 稳定排序

### 堆排序 (Heap Sort)
核心思路: 构建最大堆,反复取堆顶元素(最大值)放到末尾
```cpp
// 核心模块1: 向下调整(堆化)
void heapify(vector<int>& arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && arr[left] > arr[largest])
        largest = left;
    if (right < n && arr[right] > arr[largest])
        largest = right;
    
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);  // 递归调整
    }
}

// 核心模块2: 建堆 + 排序
void heapSort(vector<int>& arr) {
    int n = arr.size();
    
    // 建立最大堆(从最后一个非叶子节点开始)
    for (int i = n / 2 - 1; i >= 0; i--)
        heapify(arr, n, i);
    
    // 依次取出堆顶
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);     // 交换堆顶和末尾
        heapify(arr, i, 0);       // 调整剩余堆
    }
}
```
时间复杂度: O(n log n) | 空间复杂度: O(1) | 不稳定

### 快速排序 (Quick Sort)
核心思路: 选择基准元素,分区使左边小于基准、右边大于基准,递归排序
```cpp
// 核心模块1: 分区(Partition)
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];  // 选择基准
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] <= pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

// 核心模块2: 递归排序
void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);  // 分区点
        quickSort(arr, low, pi - 1);         // 左子数组
        quickSort(arr, pi + 1, high);        // 右子数组
    }
}

// 优化版: 三路快排(处理重复元素)
void quickSort3Way(vector<int>& arr, int low, int high) {
    if (low >= high) return;
    
    int lt = low, gt = high, i = low + 1;
    int pivot = arr[low];
    
    while (i <= gt) {
        if (arr[i] < pivot) swap(arr[lt++], arr[i++]);
        else if (arr[i] > pivot) swap(arr[i], arr[gt--]);
        else i++;
    }
    
    quickSort3Way(arr, low, lt - 1);
    quickSort3Way(arr, gt + 1, high);
}
```
### 选择排序
  ![选择排序](20251208-154018.png)
  - 代码实现
```cpp
  void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        // 在未排序部分找最小元素
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        // 将最小元素交换到已排序部分末尾
        if (minIdx != i) {
            swap(arr[i], arr[minIdx]);
        }
    }
}
```

### 冒泡排序
  ![冒泡排序](20251208-154148.png)
  - 代码实现
```cpp
   void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        bool swapped = false;
        for (int j = 0; j < n - 1 - i; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        // 优化：如果某趟没有发生交换，说明已经有序
        if (!swapped) break;
    }
}
```
### 插入排序
  - 代码实现
```cpp
  // 插入排序：将未排序元素逐个插入到已排序部分的正确位置
// 时间复杂度：O(n²)，空间复杂度：O(1)
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        // 将大于key的元素向后移动
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        // 插入key到正确位置
        arr[j + 1] = key;
    }
}
```
