---
layout: post
label: til
title: "Binary Heap"
---

<p>
  
  <span class="issue-label" style="background-color: #4018ba">Algorithms</span>
  
  <span class="issue-label" style="background-color: #ADE619">java</span>
  
</p>
```java
package dev.namtx.datastructures.binaryheap;

import java.util.ArrayList;

public class BinaryHeap {
    ArrayList<Integer> harr;
    int capacity;
    int heapSize;

    public BinaryHeap(int cap) {
        heapSize = 0;
        capacity = cap;
        harr = new ArrayList<>();
    }

    public void insertKey(int k) throws OverflowException {
        if (heapSize == capacity) {
            throw new OverflowException();
        }

        heapSize++;
        int i = heapSize - 1;
        harr.add(i, k);

        while (i != 0 && harr.get(parent(i)) > harr.get(i)) {
            int tmp = harr.get(parent(i));
            harr.set(parent(i), harr.get(i));
            harr.set(i, tmp);

            i = parent(i);
        }
    }

    public void MinHeapify(int i) {
        int left = i * 2 + 1;
        int right = i * 2 + 2;
        int smallest = i;

        if (left < heapSize && harr.get(left) < harr.get(smallest)) {
            smallest = left;
        }

        if (right < heapSize && harr.get(right) < harr.get(smallest)) {
            smallest = right;
        }

        if (smallest != i) {
            int tmp = harr.get(i);
            harr.set(i, harr.get(smallest));
            harr.set(smallest, tmp);

            MinHeapify(smallest);
        }
    }

    public int extractMin() {
        if (heapSize <= 0) {
            return Integer.MAX_VALUE;
        }
        if (heapSize == 1) {
            heapSize--;
            return harr.get(0);
        }

        int root = harr.get(0);
        harr.set(0, harr.get(heapSize -1));
        heapSize--;
        harr.remove(heapSize);
        MinHeapify(0);

        return root;
    }

    public void decreaseKey(int i, int val) {
        harr.set(i, val);

        while (i != 0 && harr.get(i) < harr.get(parent(i))) {
            int tmp = harr.get(i);
            harr.set(i, harr.get(parent(i)));
            harr.set(parent(i), tmp);

            i = parent(i);
        }
    }

    static int parent(int i) {
        return (i - 1) / 2;
    }

    class OverflowException extends Exception {
    }
}

class Main {
    public static void main(String[] args) throws BinaryHeap.OverflowException {
        BinaryHeap binaryHeap = new BinaryHeap(4);
        binaryHeap.insertKey(1);
        binaryHeap.insertKey(5);
        binaryHeap.insertKey(2);
        binaryHeap.insertKey(4);

        System.out.println(binaryHeap.extractMin());

        binaryHeap.decreaseKey(2, -1);
        System.out.println(binaryHeap);

    }
}
```

