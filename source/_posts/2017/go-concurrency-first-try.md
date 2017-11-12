---
title: Go Concurrency First Try
date: 2017-11-11 22:18:02
tags:
- golang
---

> Get three distinct messages from api.github.com/zen. Extra points for concurrency.

<!-- more -->

# Background

I heard about this interesting interview question from my friend last week. 

At first glance, I found it not hard to come up with a solution that calls the API repeatedly until we get three distinct responses. Just need to take care of error handling and duplication. So I tried with Golang:

{% codeblock lang:go zen.go %}
package main

import "net/http"
import "io/ioutil"
import "fmt"

func zen() (string, error) {
	client := &http.Client{}
	res, err := client.Get("https://api.github.com/zen")
	if err != nil {
		return "", err
	}
	defer res.Body.Close()

	if res.StatusCode == http.StatusOK {
		body, _ := ioutil.ReadAll(res.Body)
		return string(body), nil
	}
	return "", fmt.Errorf("Status is not 200")
}

func main() {
	words := make(map[string]string)

	for len(words) < 3 {
		word, err := zen()
		if err != nil {
			continue
		}
		if _, ok := words[word]; ok {
			continue
		}
		words[word] = word
		fmt.Println(word)
	}
}
{% endcodeblock %}

On my terminal I ran `go run zen.go` and it printed three lines of words one-by-one like

> Approachable is better than simple.
Encourage flow.
Anything added dilutes everything else.

<img class="no-box-shadow" src="zen.png" />

My piece of code simply works. Next is to take it to the next level: making it concurrent. 

<img class="no-box-shadow" src="gophercomplex1.jpg">

# Concurrency in Go

In order to write concurrent Go program, one needs to understand 
- Communicating Sequential Processes ([wiki](https://en.wikipedia.org/wiki/Communicating_sequential_processes)) model which Go's ideas about concurrency are [based on](https://golang.org/doc/faq#csp), 
- concurrency primitives **goroutine** and **channel**, and
- the concept of [share memory by communicating](https://blog.golang.org/share-memory-by-communicating) instead of communicate by sharing memory.

## Goroutine

A [goroutine](https://golang.org/doc/effective_go.html#goroutines) is like a lightweight thread with only [2 KB](https://golang.org/doc/go1.4#runtime) overhead. It is practical to create hundreds of thousands of goroutines ([quote](https://golang.org/doc/faq#goroutines)). Upon creating goroutines in your program, Go runtime will take care of scheduling and multiplexing onto system threads. 

## Channel

[Channels](https://golang.org/doc/effective_go.html#channels) connect goroutines like pipes. Or message queues. Each channel can have multiple senders and receivers.

Channels can be buffered. Unbuffered channel will block senders until a receiver has received the value. Buffered channel can store messages up to the buffer size specified.

# Solution with Concurrency

With those in mind, I came up with this solution using goroutines as [workers pool](https://gobyexample.com/worker-pools) and channels as queues to store jobs and immediate results. 

{% codeblock lang:go zen_concurrent.go %}
package main

import (
	"fmt"
	"io/ioutil"
	"net/http"
	"sync"
)

func worker(in <-chan bool, out chan<- string, putback chan<- bool) {
	for job := range in {
		client := &http.Client{}
		res, err := client.Get("https://api.github.com/zen")
		if err != nil {
			putback <- job
			break
		}
		if res.StatusCode == http.StatusOK {
			body, _ := ioutil.ReadAll(res.Body)
			out <- string(body)
		} else {
			putback <- job
		}
		res.Body.Close()
	}
}

func main() {
	count := 3

	wg := sync.WaitGroup{}
	jobs := make(chan bool, count)
	resp := make(chan string)
	messages := make(map[string]string)

	for i := 0; i < count; i++ {
		jobs <- true
		wg.Add(1)
	}

	for i := 0; i < count; i++ {
		go worker(jobs, resp, jobs)
	}

	go func() {
		for res := range resp {
			if _, ok := messages[res]; ok {
				jobs <- true
			} else {
				fmt.Println(res)
				messages[res] = res
				wg.Done()
			}
		}
	}()

	wg.Wait()
}
{% endcodeblock %}

I created two channels, buffered `jobs` and unbuffered `resp`.

A `worker` is goroutine that both receive and send jobs (when anything failed) to the `jobs` channel, and send response to `resp` channel. I created three workers for parallelism in this case.

Another goroutine I created checks for duplication of responses and print distinct messages to console. On duplication, it will send a new job to `jobs` and later workers will try get a different response. 

I also used `sync.WaitGroup` to make `main` goroutine wait until all jobs are done.

On my terminal I ran `go run zen_concurrent.go` and three lines appeared almost at the same time:

> Speak like a human.
It's not fully shipped until it's fast.
Practicality beats purity.

<img class="no-box-shadow" src="zen_concurrent.png">

I didn't measure but total running time was reduced to around one third, thanks to parallel workers. 

My first try with Go concurrency was successful and fun.