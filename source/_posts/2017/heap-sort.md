---
title: Heap Sort
date: 2017-02-27 21:56:30
tags:
- algorithms
- sorting
- golang
---

An efficient in-place sorting algorithm. [Time: O(nlgn), Space: O(1)]

![](https://upload.wikimedia.org/wikipedia/commons/1/1b/Sorting_heapsort_anim.gif)

<!-- more -->

### Idea

The idea of heap sort consists of two phases. In the first phase, the array is transformed into a binary max heap, a tree where each node of the tree is larger than or equal to its children (if any).

![](https://upload.wikimedia.org/wikipedia/commons/d/d2/Heap-as-array.svg)

The root of a max heap is always the maximum. In the second phase, the array gradually becomes sorted by moving the root to the sorted region and fixing the heap with an algorithm named sift-down. Both phases have a time complexity of O(nlgn).

### Implementation

Below is an implementation of heap sort in golang.

{% codeblock lang:go heap_sort.go https://github.com/billjh/algo/blob/master/sorting/heap_sort.go source %}
package sorting

// HeapSort Best: O(nlgn), Average: O(nlgn), Worst: O(nlgn), Space: O(1)
func HeapSort(arr []int) {
	// heapify the array into a max-heap
	heapify(arr)
	for i := len(arr) - 1; i > 0; i-- {
		// swap the root of the max-heap with the last item
		arr[0], arr[i] = arr[i], arr[0]
		// fix heap
		siftDown(arr, 0, i)
	}
}

func heapify(arr []int) {
	for i := (len(arr) - 1) / 2; i >= 0; i-- {
		siftDown(arr, i, len(arr))
	}
}

func siftDown(heap []int, lo, hi int) {
	root := lo
	for {
		child := root*2 + 1
		if child >= hi {
			break
		}
		if child+1 < hi && heap[child] < heap[child+1] {
			child++
		}
		if heap[root] < heap[child] {
			heap[root], heap[child] = heap[child], heap[root]
			root = child
		} else {
			break
		}

	}
}
{% endcodeblock %}

_References:_
1. https://en.wikipedia.org/wiki/Heapsort
2. <https://en.wikipedia.org/wiki/Heap_(data_structure)>
3. https://en.wikipedia.org/wiki/Binary_heap
4. https://golang.org/src/sort/sort.go
5. https://github.com/billjh/algo/tree/master/sorting
