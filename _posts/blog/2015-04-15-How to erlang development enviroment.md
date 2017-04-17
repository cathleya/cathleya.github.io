---
layout: post
title: "How to configure erlang development enviroment"
modified:
categories: blog
excerpt:
tags: [Erlang,String]
image:
  feature:
date: 2015-06-19T08:08:50-04:00
---



## How to configure erlang development enviroment ?


### 1.install erlang 

```
brew install erlang
``` 
	
or download and install from this [website](http://rudix.org/packages/erlang.html) and you can [learn more](https://www.erlang-solutions.com/blog/erlang-installer-a-better-way-to-use-erlang-on-osx.html)


### 2.How to install cowboy ?

```
$ mkdir hello_erlang $ cd hello_erlang

curl -O https://raw.githubusercontent.com/ninenines/erlang.mk/master/erlang.mk

brew install erlang git homebrew/dupes/make

make -f erlang.mk bootstrap bootstrap-rel

make run

```

you can [learn more]().




-------

[TOC]


