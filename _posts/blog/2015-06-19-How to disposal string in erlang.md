---
layout: post
title: "How to disposal string in erlang"
modified:
categories: blog
excerpt:
tags: [Erlang,String]
image:
  feature:
date: 2015-06-19T08:08:50-04:00
---



## How to disposal string in erlang ?


####    the length of a string
string:len("abcdef").	result: 6

####    determine whether the 2 strings are exactly equal
string:equal("abc","abc").	result: true

####    merge string
string:concat("abc","def").	result: "abcdef"	
####    The positon of ehe first occurence of a character in a string
string:chr("abdcdef",$d).	result: 3	
string:rchr("abdcdef",$d).	result: 5

#### The position of the first occurence of a string in a string
string:str("hehe haha haha","haha"). result: 6	
string:rstr("hehe haha haha","haha"). result: 11

#### Substring
string:substr("Hello World",4). result: "lo World"	
string:substr("Hello World",4,5). result: "lo Wo"	
#### Split string
string:tokens("asdhfgjjdttfg","df"). result: ["as","h","gjj","tt","g"]

#### join strings in a special character
string:join(["aaa","bbb","ccc"],"@"). result: "aaa@bbb@ccc"	
string:chars($a,5).	result: "aaaaa"
string:copies("as",5).	result: "asasasasas"
string:words("aaa bbb ccc").	result: 3

#### splict in b,for number
string:words("abcbchdbjfb"，$b). result: 4	
#### split in character b,take third
string:sub_words("abcbchdbjfb",3,$b). result:  "chd"	
#### remove spaces on both sides of the string 
string:strip("    aaa  ").	result:  "aaa"

#### removve . on both sides of the string 
string:strip("...aaa..",both,$.). result:  "aaa"	
#### intercept the first 10 characters，instead of space if it is not enough ,like (string:right)and(string:centre)
string:left("hahaha",10).	result:  "hahaha    "	
#### intercept the first 10 characters，instead of ! if it is not enough,like(string:right)(string:centre)
string:left("hahaha",10,$!).	result:  "hahaha!!!!"	
string:to_integer("123sa23"). result:  {123,"sa23"}

####  convert lower case
srring:to_lower("asFDds").	result:  "asfdds"	
#### convert upper case
srring:to_upper("asFDds").	result:  "ASFDDS"	



-------

[TOC]


