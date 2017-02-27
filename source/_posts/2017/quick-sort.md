---
title: Quick Sort
date: 2017-02-27 22:55:44
tags:
- algorithms
- sorting
- golang
---

An efficient in-place divide-and-conquer sorting algorithm. [Best: O(nlgn), Average: O(nlgn), Worst: O(n<sup>2</sup>), Space: O(n)]

<!-- more -->

![](https://upload.wikimedia.org/wikipedia/commons/6/6a/Sorting_quicksort_anim.gif)

### Idea

The idea of quick sort is a divide-and-conquer approach that partitions the array into subarrays with pivots. When an item is picked as pivot, the items are rearranged such that all smaller items are moved to the left of pivot and all larger items are to the right. The array becomes sorted when all subarrays are all sorted (at most 1 item).

### Implementation

Below is an implementation of quick sort in golang.

{% codeblock lang:go quick_sort.go https://github.com/billjh/algo/blob/master/sorting/quick_sort.go source %}
package sorting

// QuickSort Best: O(nlgn), Average: O(nlgn), Worst: O(n^2), Space: O(n)
func QuickSort(arr []int) {
	quickSort(arr, 0, len(arr)-1)
}

func quickSort(arr []int, lo, hi int) {
	if lo < hi {
		pivot := partition(arr, lo, hi)
		quickSort(arr, lo, pivot-1)
		quickSort(arr, pivot+1, hi)
	}
}

// Lomuto partition scheme
func partition(arr []int, lo, hi int) int {
	pivot := arr[hi]
	i := lo
	for j := lo; j < hi; j++ {
		if arr[j] <= pivot {
			arr[i], arr[j] = arr[j], arr[i]
			i++
		}
	}
	arr[i], arr[hi] = arr[hi], arr[i]
	return i
}
{% endcodeblock %}

_References:_
1. https://en.wikipedia.org/wiki/Quicksort
2. https://github.com/billjh/algo/tree/master/sorting
