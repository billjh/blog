---
title: Merge Sort
date: 2017-02-17 18:41:56
tags:
- algorithms
- sorting
- golang
---

An efficient and stable divide-and-conquer sorting algorithm. [Best: O(n), Average: O(nlgn), Worst: O(nlgn), Space: O(n)]

<!-- more -->

![](https://upload.wikimedia.org/wikipedia/commons/c/cc/Merge-sort-example-300px.gif)

### Idea

The idea of merge sort uses a classic divide-and-conquer approach. By recursively dividing the array into smaller sub-arrays, the sub-arrays will finally all become sorted (at most 1 item). Next, those sub-arrays are merged back together in a sorted manner until all becomes one. It is an efficient and stable sorting algorithm.

### Implementation

Below is an implementation of merge sort in golang.

{% codeblock lang:go merge_sort.go https://github.com/billjh/algo/blob/master/sorting/merge_sort.go source %}
package sorting

func MergeSort(arr []int) {
	copy(arr, mergeSort(arr))
}

func mergeSort(arr []int) []int {
	if len(arr) <= 1 {
		return arr
	}
	middle := len(arr) / 2
	left := mergeSort(arr[:middle])
	right := mergeSort(arr[middle:])
	return merge(left, right)
}

func merge(left, right []int) []int {
	r := make([]int, len(left)+len(right))
	for i := 0; ; i++ {
		if len(left) > 0 && len(right) > 0 {
			// pick one smaller item when two arrays are both non-empty
			if left[0] > right[0] {
				r[i] = right[0]
				right = right[1:]
			} else {
				r[i] = left[0]
				left = left[1:]
			}
		} else if len(left) > 0 {
			copy(r[i:], left)
			break
		} else if len(right) > 0 {
			copy(r[i:], right)
			break
		}
	}
	return r
}
{% endcodeblock %}

_References:_
1. https://en.wikipedia.org/wiki/Merge_sort
2. https://github.com/billjh/algo/tree/master/sorting
