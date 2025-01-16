---
title: "Basics for Software Interview"
---

<details>
<summary><strong>Table of Contents</strong></summary>

<ol style="padding-left: 20px; margin: 0;">
  <li style="margin-bottom: 0px;">
    <a href="#binary-search-algorithm-to-find-the-smallest-index-of-an-element" style="text-decoration:none; color:#1a73e8;">
      <strong>Binary Search Algorithm to Find the Smallest Index of an Element</strong>
    </a>
  </li>
  <ul style="margin: 0 0 0px 10px; padding: 0;">
    <li style="margin-bottom: 0px;">
      <a href="#implementation" style="text-decoration:none; color:#4285f4;">Implementation</a>
    </li>
  </ul>
  <li style="margin-bottom: 0px;">
    <a href="#quicksort" style="text-decoration:none; color:#1a73e8;">
      <strong>QuickSort</strong>
    </a>
  </li>
  <ul style="margin: 0 0 0px 10px; padding: 0;">
    <li style="margin-bottom: 0px;">
      <a href="#implementation-1" style="text-decoration:none; color:#4285f4;">Implementation</a>
    </li>
  </ul>

  <li style="margin-bottom: 0px;">
    <a href="#mergesort" style="text-decoration:none; color:#1a73e8;">
      <strong>MergeSort</strong>
    </a>
  </li>
  <ul style="margin: 0 0 0px 10px; padding: 0;">
    <li style="margin-bottom: 0px;">
      <a href="#implementation-2" style="text-decoration:none; color:#4285f4;">Implementation</a>
    </li>
  </ul>

  <li style="margin-bottom: 0px;">
    <a href="#heapsort" style="text-decoration:none; color:#1a73e8;">
      <strong>HeapSort</strong>
    </a>
  </li>
  <ul style="margin: 0 0 0px 10px; padding: 0;">
    <li style="margin-bottom: 0px;">
      <a href="#implementation-3" style="text-decoration:none; color:#4285f4;">Implementation</a>
    </li>
  </ul>

  <li style="margin-bottom: 0px;">
    <a href="#merge-overlapping-intervals" style="text-decoration:none; color:#1a73e8;">
      <strong>Merge Overlapping Intervals</strong>
    </a>
  </li>
  <ul style="margin: 0 0 0px 10px; padding: 0;">
    <li style="margin-bottom: 0px;">
      <a href="#implementation-4" style="text-decoration:none; color:#4285f4;">Implementation</a>
    </li>
  </ul>

</ol>

</details>




# Binary Search Algorithm to Find the Smallest Index of an Element

Binary search is a highly efficient algorithm used to find the position of a target value within a sorted array. The algorithm works by repeatedly dividing the search interval in half. If the value of the search key is less than the item in the middle of the interval, the algorithm narrows the interval to the lower half. Otherwise, it narrows it to the upper half. This process continues until the target value is found or the interval is empty.

In this article, we'll implement a binary search function that not only finds the target value but also ensures that we return the smallest index of that value in the array if it appears multiple times.

## Implementation

Below is the C++ code for the binary search function:

```cpp
int binarysearch(int arr[], int start, int end, int x){
    int result = -1;
    while(start <= end){
        int mid = start + (end - start) / 2;
        if(arr[mid] == x){
            result = mid; // Update the result
            end = mid - 1; // Continue search for smallest index of x
        }
        if(arr[mid] < x)
            start = mid + 1; // Ignore left half
        else
            end = mid - 1; // Ignore right half
    }
    return result;
}

// Initial Call
int index = binarysearch(arr, 0, arr.size() - 1, x);

```
# QuickSort

## Implementation
Below is the C++ code for the QuickSort function:
```cpp
int partition(vector<int> &A, int low, int high){
    int i = low;
    int j = high + 1;
    while(true){
        while(A[++i] < A[low]){
            if(i == high) break;
        }
        while(A[--j] > A[low]){
            if(j == low) break;
        }
        if(i >= j) break;
        exch(A, i, j);
    }
    exch(A, low, j);
    return j;
}

void QuickSort(vector<int> &A, int low, int high){
    if(low < high){
        int partitionPos = partition(A, low, high);
        QuickSort(A, low, partitionPos - 1);
        QuickSort(A, partitionPos + 1, high);
    }
}

// Initial call
Random-shuffle(A)
QuickSort(A, 0, A.size() - 1)

```


# MergeSort

## Implementation

```cpp
void Merge(vector<int> &A, int low, int mid, int high){
    vector<int> arr;
    int i = low;
    int j = mid + 1;
    while(i <= mid && j <= high){
        if(A[i] <= A[j]) arr.push_back(A[i++]); // Determines stability of the algorithm. With equals operator it is stable.
        else arr.push_back(A[j++]);
    }
    while(i <= mid) arr.push_back(A[i++]);
    while(j <= high) arr.push_back(A[j++]);
    //Remerge
    for(int k=0;k<arr.size();k++) A[low++] = arr[k];
}

void MergeSort(vector<int> &A, int low, int high){
    if(low < high){
        int mid = low + (high - low)/2;
        MergeSort(A, low, mid);
        MergeSort(A, mid+1, high);
        Merge(A, low, mid, high);
    }
}

//Initial call
MergeSort(A, 0, A.size()-1);
```

# HeapSort (Using Max-Heap)

Max-heap is a binary tree in which each node is greater than its children or equal to its left child. **A binary heap is typically represented as an array**.

## Implementation

```cpp
void exch(vector<int>& A, int a, int b){
    int temp = A[a];
    A[a] = A[b];
    A[b] = temp;
}

bool _less(vector<int> &A, int a, int b){
    return A[a] < A[b];
}


void swim(vector<int> &heap, int pos){
    while(pos > 0 && _less(heap, pos/2, pos)){
        exch(heap, pos/2, pos);
        pos = pos/2;
    }
}

void sink(vector<int> &heap, int k){
    while(2*k + 1 < heap.size()){
        int swapWith = 2*k; //Left Child
        if(swapWith < heap.size() && _less(heap, swapWith, swapWith + 1)) swapWith++; //If left child is smaller than right child then swap with right child.
        if(!_less(heap, k, swapWith)) break;
        exch(heap, k, swapWith);
        k = swapWith;
    }
}

void insert(vector<int> &heap, int x){
    heap.push_back(x);
    swim(heap, heap.size()-1); //Swim the last element up.
}

int delMax(vector<int> &heap){
    int maxElement = heap[0];
    exch(heap, 0, heap.size()-1);
    heap.pop_back();
    sink(heap, 0);
    return maxElement;
}

void constructHeap(vector<int> &heap, vector<int> &A){
    for(int i=0; i<A.size(); i++) insert(heap, A[i]);
}

void HeapSort(vector<int>& A, int low, int high){
    //Build a max-heap: Insert elements one by one and keep swimming them up.

    vector<int> heap;
    constructHeap(heap, A);
    //Remerge i.e Pop elements from the top of heap and keep adding it to
    // the original array.
    for(int i=high;i>=low;i--) A[i] = delMax(heap);
}
//Initial call
HeapSort(A, 0, A.size() - 1);
```



# Merge Overlapping Intervals

This approach merges overlapping intervals **in-place**, ensuring that the output contains only mutually exclusive intervals. As intervals are merged, they are kept at the start of the original array. By the end of the process, the new size of the array represents the number of mutually exclusive intervals.

## Approach

**Input**: A list of intervals `[startTime, endTime]`.

### Steps
1. **Sort** the intervals by their start times.
2. Initialize `newIntervalSize` to 0. This will keep track of the size of the resultant array of merged intervals.
3. Use a `currentInterval` to hold the interval currently being merged.
4. Iterate through the sorted intervals:
   - If the current interval does not overlap with the `currentInterval`, save the `currentInterval` to the result, and update `currentInterval` to the current interval.
   - Otherwise, merge the intervals by updating the end time of `currentInterval` to the maximum of the two overlapping intervals.
5. After the loop, add the last `currentInterval` to the result.
6. Resize the array to `newIntervalSize` to discard any remaining elements.

---

## Implementation

```cpp
#include <vector>
#include <algorithm>

void mergeIntervals(std::vector<std::vector<int>>& intervals) {
    // Step 1: Sort intervals by start time
    std::sort(intervals.begin(), intervals.end());

    // Step 2: Initialize variables
    int newIntervalSize = 0;
    std::vector<int> currentInterval = intervals[0];

    // Step 3: Loop through intervals to merge
    for (int i = 1; i < intervals.size(); ++i) {
        if (intervals[i][0] > currentInterval[1]) {
            // No overlap: Save the current interval and update it
            intervals[newIntervalSize] = currentInterval;
            newIntervalSize++;
            currentInterval = intervals[i];
        } else {
            // Overlap: Merge intervals by extending the end time
            currentInterval[1] = std::max(currentInterval[1], intervals[i][1]);
        }
    }

    // Step 4: Save the last interval
    intervals[newIntervalSize] = currentInterval;
    newIntervalSize++;

    // Step 5: Resize the intervals array to only include merged intervals
    intervals.resize(newIntervalSize);
}


```
