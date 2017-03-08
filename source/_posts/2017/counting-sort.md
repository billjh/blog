---
title: Counting Sort
date: 2017-03-08 23:18:50
tags:
- algorithms
- sorting
- golang
---

A stable and linear-time sorting algorithm for items with keys within a limited size of collection. [Time: O(n+k), Space: O(n+k)]

<!-- more -->

### Idea

The idea of counting sort is to count the numbers of items according to each distinct key, then decide each item's position using prefix sum. The time complexity for counting sort is O(n+k) where n is the number of items and k is the number of possible keys for each item.

### Implementation

Below is an implementation of counting sort in golang.

{% codeblock lang:go counting_sort.go https://github.com/billjh/algo/blob/master/sorting/couting_sort.go source %}
package sorting

// CountingSort Time: O(n+k), Space: O(n+k)
func CountingSort(arr []int, k int) {
	c := make([]int, k)
	for i := 0; i < len(arr); i++ {
		c[arr[i]]++
	}
	// pre-fix sum
	for i, sum := 0, 0; i < k; i++ {
		// tmp := c[i]
		// c[i] = sum
		// sum += tmp
		sum, c[i] = sum+c[i], sum
	}
	sorted := make([]int, len(arr))
	for _, n := range arr {
		sorted[c[n]] = n
		c[n]++
	}
	copy(arr, sorted)
}
{% endcodeblock %}

_References:_
1. https://en.wikipedia.org/wiki/Counting_sort
2. https://github.com/billjh/algo/tree/master/sorting
