---
layout: post
label: til
title: "HeapSort"
---

<p>
  
  <span class="issue-label" style="background-color: #4018ba">Algorithms</span>
  
  <span class="issue-label" style="background-color: #ADE619">java</span>
  
</p>
```java
public class HeapSort {
    public void sort(int[] arr) {
        int n = arr.length;

        // build heap
        for (int i = n/2 -1 ; i >=0; i--) {
            heapify(arr, n, i);
        }

        printArray(arr);
    }

    void heapify(int[] arr, int n, int i) {
        int largest = i;
        int l = 2*i+1;
        int r = 2*i+2;

        if (l < n && arr[l] > arr[largest]) {
            largest = l;
        }

        if (r < n && arr[r] > arr[largest]) {
            largest = r;
        }

        if (largest != i) {
            int swap = arr[i];
            arr[i] = arr[largest];
            arr[largest] = swap;

            heapify(arr, n, largest);
        }
    }

    void printArray(int[] arr) {
        for (int j : arr) {
            System.out.println(j);
        }
    }
}
```

