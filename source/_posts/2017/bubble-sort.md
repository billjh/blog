---
title: Bubble Sort
date: 2017-02-17 12:47:43
tags:
- algorithms
- sorting
- golang
---

A stable quadratic sorting algorithms. [Best: O(n), Average: O(n<sup>2</sup>), Worst: O(n<sup>2</sup>), Space: O(1)]

<!-- more -->

![](https://upload.wikimedia.org/wikipedia/commons/c/c8/Bubble-sort-example-300px.gif)

### Idea

The idea of the bubble sort is to iteratively step through the array, compare pairs of adjacent items and swap them if needed. The largest item will quickly "bubble" to the end during one iteration. In an optimized implementation, it only takes _O(n)_ scans for an already sorted array of _n_ items. However, the performance is very slow for average cases.

### Implementation

Below is an optimized implementation of bubble sort in golang. Each iteration in outer loop will keep track of the large items those have been already in their final positions. So the next iteration will skip those items.

{% codeblock lang:go bubble_sort.go https://github.com/billjh/algo/blob/master/sorting/bubble_sort.go source %}
package sorting

func bubbleSort(arr []int) {
	n := len(arr)
	for n > 0 {
		newn := 0
		for i := 1; i < n; i++ {
			if arr[i-1] > arr[i] {
				arr[i], arr[i-1] = arr[i-1], arr[i]
				newn = i
			}
		}
		n = newn
	}
}
{% endcodeblock %}

_References:_
1. https://en.wikipedia.org/wiki/Bubble_sort
2. https://github.com/billjh/algo/tree/master/sorting
