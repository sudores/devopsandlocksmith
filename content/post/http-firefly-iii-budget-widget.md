---
title: "HTTP Firefly-III Budget Widget"
date: "2023-09-27T22:07:43+03:00"
draft: true
tags: ["Firefly-III", "Life Automation", "Golang", "Android"]
categories: ["Life Automation"]
description: "Gettting firefly-iii budgets data to android widget"
---

<!-- 
TODO:
  -  Add link to JSON Response Widget
  -
-->

# HTTP Firefly-III Budget Widget

## Foreword

Some time ago I've faced with the issue of overspending budgets in
restaurants and food categories. With my girlfriend as main spender.
And found solution was rather easy. It was not to get rid of girlfriend.  
But solution which is now used in testing phase is to get the widget with
budget stats for android where data like budgeting period, budgeted amount
and left to spend fields would be present.

So task seemed really ease at first look. But let's move to architecture
to get understanding of task.

## Architecture

Task as it was said is neither complicated nor hard to get done.
We just have a data source in my case self hosted firefly-iii instance with
some budgets and spending withing those budgets. And the only thing we need to
do is to get data from the firefly-iii to the widget on android home screen.
There're bunch of apps which allow you to make http request and put
response into widget I thought.  
But there're not such apps! You can find app to date a dragon in Siberia.
Where you're single soul for 500 kilometers around. But not an app
to get http response into widget.
Or at least app to get some data from prometheus/grafana to your home screen.
So the only app I've found for that purpose was a [JSON Response Widget]().  
And the problem of this app is that it was not updated for couple years.
And you're just not allowed to install it to your modern android device from
google play.  
Just because google think that it will not work well on your device because
it hasn't update for some time. I've distracted we're not talking about this.

So We've JWR which displays json fields to us And firefly with it's huge
and complicated API.
JWR formats json in some way.  
But we still need to get smaller JSON which will not make our eyes bleeding
when displayed by JWR.  
And here we've couple ways. The first one is to setup firefly-iii prometheus
exporter and get data from prometheus through small integration app.  
Which will give us architecture like following
```
+-----------------------------------------------------------------+
| +-------------+     +----------+     +------------+     +-----+ |
| | Firefly-iii | --> | Exporter | --> | Prometheus | --> | App | |
| +-------------+     +----------+     +------------+     +-----+ |
|                                              +-----+       |    |
|                                              | JRW | <-----/    |
|                                              +-----+            |
+-----------------------------------------------------------------+
```

This way has much more flexibility and will allow us to extend ours widget
to other metrics if only we want. And possibly I'll implement it so further.  
But instead we can just connect to firefly-iii directly and get data from
there. And the second was chosen by me as easier in implementation and
having less impact to the servers which are actually overloaded now.

And currently I've the following. Less complicated and rather
straight forward.

```
+-----------------------------------------+
| +-------------+     +-----+     +-----+ |
| | Firefly-iii | --> | App | --> | JRW | |
| +-------------+     +-----+     +-----+ |
+-----------------------------------------+
```

So as for now the 
