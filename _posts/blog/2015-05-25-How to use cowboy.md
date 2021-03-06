---
layout: post
title: "How to use cowboy"
modified:
categories: blog
excerpt:
tags: [Erlang,Cowboy]
image:
  feature:
date: 2015-06-19T08:08:50-04:00
modified: 2016-06-19T14:19:19-04:00
---



## How to use cowboy?


###	3.cowboy (restful_handler.erl)


```

-module(restful_handler).

-export([init/2]).
-export([allowed_methods/2]).
-export([content_types_provided/2]).
-export([content_types_accepted/2]).
-export([delete_completed/2]).
-export([delete_resource/2]).

-export([hello_to_html/2]).
-export([form_urlencoded_post/2]).
-export([form_data_post/2]).
-export([json_post/2]).

init(Req, Opts) ->
  {cowboy_rest, Req, Opts}.

allowed_methods(Req, State) ->
  Methods = [
    <<"HEAD">>,
    <<"GET">>,
    <<"POST">>,
    <<"PATCH">>,
    <<"DELETE">>,
    <<"OPTIONS">>
  ],
  {Methods, Req, State}.

content_types_provided(Req, State) ->
  Handlers = [
    {<<"text/html">>, hello_to_html},
    {<<"application/x-www-form-urlencoded">>, form_urlencoded_post},
    {<<"multipart/form-data">>, form_data_post},
    {<<"application/json">>, json_post}
  ],
  {Handlers, Req, State}.

content_types_accepted(Req, State) ->
  Handlers = [
    {<<"application/x-www-form-urlencoded">>, form_urlencoded_post},
    {<<"multipart/form-data">>, form_data_post},
    {<<"application/json">>, json_post}
  ],
  {Handlers, Req, State}.

delete_completed(Req, State) ->
  io:format("will delete resource~n"),
  {true, Req, State}.

delete_resource(Req, State) ->
  io:format("delete resource finish~n"),
  {true, Req, State}.

hello_to_html(Req, State) ->
  Body = <<"html">>,
  {Body, Req, State}.

form_urlencoded_post(Req, State) ->
  {ok, PostVals, _Req2} = cowboy_req:body_qs(Req),
  PostVal1 = proplists:get_value(<<"aaa">>, PostVals),
  PostVal2 = proplists:get_value(<<"bbb">>, PostVals),
  io:format("form_urlencoded_post~p~p~n",[PostVal1,PostVal2]),

  Body = string:concat(
    erlang:bitstring_to_list(PostVal1),
    erlang:bitstring_to_list(PostVal2)
  ),

  NewReq = cowboy_req:set_resp_body(erlang:list_to_bitstring(Body),Req),

  {true, NewReq, State}.

form_data_post(Req, State) ->
  {ok, PostVals, _Req2} = cowboy_req:body_qs(Req),
  PostVal = proplists:get_value(<<"aaa">>, PostVals),
  io:format("form_data_post~p~n",[PostVal]),
  NewReq = cowboy_req:set_resp_body(PostVal,Req),
  {true, NewReq, State}.

json_post(Req, State) ->

  {ok, PostVals, _Req2} = cowboy_req:body_qs(Req),
  
  PostVal = proplists:get_value(<<"aaa">>, PostVals),
  io:format("json_post~p~n",[PostVal]),

  NewReq = cowboy_req:set_resp_body(PostVal,Req),
  {true, NewReq, State}.
  

```


-------

[TOC]


