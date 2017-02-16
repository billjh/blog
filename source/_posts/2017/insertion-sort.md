---
title: Insertion Sort
date: 2017-02-16 14:45:51
tags:
- algorithms
- sorting
- golang
---

A stable and adaptive quadratic sorting algorithm. [Best: O(n), Average: O(n<sup>2</sup>), Worst:O(n<sup>2</sup>), Space: O(1)]

<!-- more -->

![](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)

### Idea

The idea of insertion sort is to start with _a sorted array_ of the first item on the left, and move one unsorted item to correct position by comparing with the sorted items, and repeat until all items are sorted.

There are two noteworthy advantages of insertion sort. First, insertion sort is _stable_ in nature. The relative order of items with equal _key_ remains unchanged before and after sorting. Second, insertion sort is very efficient on almost already sorted arrays.

### First Implementation in Go

Below is my first implementation. It's concise since I can perform the swaps without temporary variable, thanks to golang's parallel assignment.

{% codeblock lang:go insertion_sort.go https://github.com/billjh/algo/blob/master/sorting/insertion_sort.go source %}
package insertion

func sort(arr []int) {
	for i := 1; i < len(arr); i++ {
		for j := i; j > 0 && arr[j] < arr[j-1]; j-- {
			arr[j], arr[j-1] = arr[j-1], arr[j]
		}
	}
}
{% endcodeblock %}

### Optimization

The above implementation leaves rooms for performance tuning. In the outer loop, to move an item with _k_ swaps takes _2k_ operations. It can be cut down to half by shifting each item to its neighbor with only _k_ operations.

{% codeblock mark:5-10 lang:go insertion_sort_optimized.go https://github.com/billjh/algo/blob/master/sorting/insertion_sort_optimized.go source %}
package insertion

func sortOpt(arr []int) {
	for i := 1; i < len(arr); i++ {
		unsorted := arr[i]
		j := i - 1
		for ; j >= 0 && unsorted < arr[j]; j-- {
			arr[j+1] = arr[j]
		}
		arr[j+1] = unsorted
	}
}
{% endcodeblock %}

With the help of go benchmark tool, we see a constant performance growth of around 2x after optimization.

{% codeblock mark:2-4 %}
► go test -bench=.
BenchmarkOptimizedInsertionSort100-8            10000000               120 ns/op
BenchmarkOptimizedInsertionSort1000-8            1000000              1075 ns/op
BenchmarkOptimizedInsertionSort10000-8            100000             10584 ns/op
BenchmarkInsertionSort100-8                     10000000               204 ns/op
BenchmarkInsertionSort1000-8                     1000000              1916 ns/op
BenchmarkInsertionSort10000-8                     100000             19044 ns/op
PASS
ok      github.com/billjh/algo/sorting  10.389s
{% endcodeblock %}


_References:_
1. https://en.wikipedia.org/wiki/Insertion_sort
2. https://github.com/billjh/algo/tree/master/sorting
3. https://betterexplained.com/articles/sorting-algorithms/#Insertion_Sort_Best_ON_WorstON2
4. https://golang.org/pkg/testing/#hdr-Benchmarks
