---
title: Selection Sort
date: 2017-02-17 11:50:54
tags:
- algorithms
- sorting
- golang
---

A space-efficient quadratic sorting algorithms. [Time: Θ(n<sup>2</sup>), Space: O(1)]

<!-- more -->

### Idea

The idea of selection sort is to iteratively select the smallest (or largest) item from the unsorted _n_ items and swap to the first position. It takes _O(k)_ swaps and _Θ(k<sup>2</sup>)_ scans and comparisons for an array of _k_ items.

### Implementation

Below is an implementation of selection sort in golang.

{% codeblock lang:go selection_sort.go https://github.com/billjh/algo/blob/master/sorting/selection_sort.go source%}
package sorting

func SelectionSort(arr []int) {
	for i := 0; i < len(arr)-1; i++ {
		iMin := i
		for j := i + 1; j < len(arr); j++ {
			if arr[j] < arr[iMin] {
				iMin = j
			}
		}
		if iMin != i {
			arr[i], arr[iMin] = arr[iMin], arr[i]
		}
	}
}
{% endcodeblock %}

_References:_
1. https://en.wikipedia.org/wiki/Selection_sort
2. https://github.com/billjh/algo/tree/master/sorting
