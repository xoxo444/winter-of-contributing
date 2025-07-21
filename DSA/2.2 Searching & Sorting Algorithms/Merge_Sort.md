# Merge Sort

### Introduction 📖

Merge Sort is one of the most popular sorting algorithms that is based on the principle of "__Divide and Conquer Algorithm__".

Here, the problem is divided into multiple sub-problems. Now, Each sub-problem is solved individually. Finally, all the sub-problems are combined to form a final solution.

> In Divide and Conquer Technique, we divide a problem into subproblems. When the solution to each subproblem is ready, we thus, 'combine' the results from the subproblems to solve the main problem.

### Intuition behind using Merge Sort 💡

The idea is simple, we divide the array into 2 halves recursively, perform merge sort on left half and right half independently. Then we merge the two sorted left and right halves. 

<img src="https://cdn-images-1.medium.com/max/600/1*ZwWt6m6bMRZTL_-8r4zOOQ.gif" style="height: 40%;">

__Pseudo Code:__

```
MergeSort(int arr[], int start, int end) {
  if start>end :
      1. Find Middle point to divide array in 2 halves :
          mid= (start+end)/2;
      2. Call MergeSort on first half :
          MergeSort(arr,start,mid);
      3. Call MergeSort on second half :
          MergeSort(arr,mid+1,end);
      4. Merge both the sorted halves :
          Merge(arr,start,mid,end);
}
```

![image](https://user-images.githubusercontent.com/58984074/135335459-a2d87b9c-19c4-4bf4-b176-df37a531a2b0.png)


#### Merge Function: ⚙️

We are given two sorted halves and we, need to combine them into one sorted array. This can be approached easily by thinking of two pointer approach. 

- Create an auxilary array of size (end-start+1) to store the new sorted array
- Intialize two pointers to head=start and tail=mid+1
- If arr[head]<=arr[tail], then temp_arr[index]=arr[head], head++;
- else temp_arr[index]=arr[tail], tail++;

### Code 💻

```c++
// Merge Function 
void Merge(int arr[], int start, int mid, int end) {
    // Create and Auxillary Array to store new sorted array
    int temp_arr[end-start+1];
    int index=0, head=start, tail=mid+1;
    while(head<=mid and tail<=end) {
        // Checking Which part has smaller element storing in temp_arr
        if(arr[head]<=arr[tail]) {
            temp_arr[index]=arr[head];
            head++;
        } else {
            temp_arr[index]=arr[tail];
            tail++;
        }
        index++;
    }
    // If first half has still elements left 
    while(head<=mid) {
        temp_arr[index++]=arr[head++];
    }
    // If second half has still elements left 
    while(tail<=end) {
        temp_arr[index++]=arr[tail++];
    }
    index=0;
    // Storing back the result in Orignal Array 
    while(start<=end) {
        arr[start]=temp_arr[index++];
        start++;
    }
}
// Merge sort 
void Merge_sort(int arr[], int start, int end) {
    if(start<end) {
        // Find the Middle point to divide the array in two halves
        int mid=(start+end)/2;
        // Perform Merge Sort on First Half from arr[start,mid]
        Merge_sort(arr,start,mid);
        // Perform Merge Sort on Second Half from arr[mid+1,end]
        Merge_sort(arr,mid+1,end);
        // Merge both the sorted Halves 
        Merge(arr,start,mid,end);
    }
}  

// To sort Arr[] of size n, Call Merge_sort(Arr,0,n-1);
```

### ⏰ Time and Space Complexity Analysis :

Since we are first, recursively dividing the arrays and then sorting the two halves, thus it can be expressed as the following recurrence relation:

```
T(n) = 2T(n/2) + θ(n)
T(n) = 2T(n/2) + cn 
Using Master Theorem it can be reduced to  T(n) = aT(n/b) + O(n^k), where a=2 and b=2 and k=1
Therefore, Time complexity T(n)= O(n^k * logn)
                               = O(n^1 * logn)
                               = O(nlogn)
```
Therefore, **Time Complexity** of Merge Sort Algorithm is O(nlogn)

Also, **Space Complexity** of Merge Sort = Space Complexity of Merging + Size of of recursion Stack = O(n) + O(logn) = O(n)

                              
| Time Complexity |  |
| --- | --- |
| Best | O(n*log(n)) |
| Worst | O(n*log(n)) |
| Average | O(n*log(n)) |
| Space Complexity | O(n) |
| Stability | YES |



### Applications of Merge Sort 📝: 

1. Merge Sort is a stable sorting algorithm therefore similar elements would be in the same order in the newly sorted array. 
2. Merge Sort is best choice for sorting Linked Lists, it would require only O(1) extra space. 
3. Inversion Count Problem  
4. Used in [External Sorting](https://en.wikipedia.org/wiki/External_sorting)

#### Some Drawbacks of Merge Sort: 

1. Merge Sort Algorithm requires an extra space of O(n) for the temporary array
2. Worst Case Time Complexity is also O(nlogn), i.e it would perform all operations even for the sorted Array

### References 📙

1. https://www.enjoyalgorithms.com/blog/merge-sort-algorithm
2. https://www.geeksforgeeks.org/merge-sort/
3. https://en.wikipedia.org/wiki/Merge_sort



Here is the complete **Quick Sort write-up** that you can directly copy and paste into your GitHub `README.md` or any markdown file:

---

# Quick Sort

## 📖 Introduction

Quick Sort is a widely used and efficient sorting algorithm based on the **Divide and Conquer** technique. Unlike Merge Sort, Quick Sort works by selecting a pivot element and partitioning the array around the pivot such that elements less than the pivot are on the left, and elements greater are on the right. It then recursively sorts the sub-arrays.


## 💡 Intuition behind Quick Sort

The algorithm selects a pivot and reorders the array so that all elements less than the pivot come before it and all elements greater come after. This step is known as *partitioning*. After partitioning, the pivot is in its final sorted position. This process is recursively applied to the sub-arrays formed by splitting at the pivot.


## 🧾 Pseudo Code

```
QuickSort(arr[], low, high):
  if (low < high):
    1. pi = Partition(arr, low, high)
    2. QuickSort(arr, low, pi - 1)
    3. QuickSort(arr, pi + 1, high)

Partition(arr[], low, high):
  1. Choose the pivot (usually the last element)
  2. Rearrange elements by comparing with pivot
  3. Return index of pivot after partition
```

## 💻 Code (C)

```c
#include <stdio.h>

void swap(int* a, int* b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int partition(int arr[], int low, int high) {
    int pivot = arr[high]; 
    int i = low - 1;      
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(&arr[i], &arr[j]);
        }
    }

    swap(&arr[i + 1], &arr[high]);
    return i + 1;
}

void quicksort(int arr[], int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quicksort(arr, low, pi - 1);
        quicksort(arr, pi + 1, high);
    }
}

void printarray(int arr[], int size) {
    for (int i = 0; i < size; i++)
        printf("%d ", arr[i]);
    printf("\n");
}

int main() {
    int arr[] = {10, 7, 8, 9, 1, 5};
    int n = sizeof(arr) / sizeof(arr[0]);

    printf("Original array: ");
    printarray(arr, n);

    quicksort(arr, 0, n - 1);

    printf("Sorted array:   ");
    printarray(arr, n);

    return 0;
}
```

![image](https://media.geeksforgeeks.org/wp-content/uploads/20240926172924/Heap-Sort-Recursive-Illustration.webp)


## ⏰ Time and Space Complexity

| Case    | Time Complexity |
| ------- | --------------- |
| Best    | O(n log n)      |
| Average | O(n log n)      |
| Worst   | O(n²)           |

* **Space Complexity**: O(log n) (due to recursion stack)
* **In-place**: Yes
* **Stable**: No

## 📝 Applications

1. Quick Sort is used in many libraries as the default sorting algorithm (e.g., Python's TimSort is a hybrid).
2. Efficient for large datasets that fit in memory.
3. Useful in scenarios requiring in-place sorting with minimal space.


## ❗ Drawbacks

1. Worst-case performance is O(n²), though mitigated using randomized pivoting.
2. Not stable: identical elements may not preserve input order.
3. Recursive implementation can lead to stack overflow if not optimized.


## 📚 References

1. [GeeksforGeeks: Quick Sort](https://www.geeksforgeeks.org/quick-sort/)
2. [Wikipedia: Quicksort](https://en.wikipedia.org/wiki/Quicksort)
3. [Programiz: Quick Sort](https://www.programiz.com/dsa/quick-sort)


