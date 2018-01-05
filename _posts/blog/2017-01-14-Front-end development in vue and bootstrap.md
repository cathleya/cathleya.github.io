---
layout: post
title: "Front-end development in vue and bootstrap"
modified:
categories: blog
excerpt:
tags: [Front-end,vue,bootstrap]
image:
  feature:
date: 2017-01-14T10:12:30-05:03
modified: 2017-01-21T15:09:29-04:20
---


Today I've made some notes in studying vue and bootstrap.


### To initialize a vue project firstly 

* initialize project

    `cnpm install vue`
    `cnpm install --global vue-cli`
    `vue init webpack axis_project`
    `cd axis_project`
    `npm install`
    `npm run dev`

* To generate sever package

    `npm run build`
    
* To disposal of the problem of empty page in sever package

    `cd config`
    `vi index.js -> build: {assetsSubDirectory: 'static',
    assetsPublicPath: './',}`
    
    -------------------------------
    
    `cd build`
    `vi utils.js`
    
    -------------------------------
    
    `if (options.extract) {
        return ExtractTextPlugin.extract({
            use: loaders,
            fallback: 'vue-style-loader'
        })
    } else {
      return ['vue-style-loader'].concat(loaders)
    }`
  
    * ->
    -------------------------------

    `if (options.extract) {
        return ExtractTextPlugin.extract({
            use: loaders,
            fallback: 'vue-style-loader',
            publicPath: '../../'
        })
    } else {
        return ['vue-style-loader'].concat(loaders)
    }`


	
	

### To install jquery and bootstrap in vue project

* install jquery

1.

    `npm install jquery --save-dev`
    
2.
    
    `cd build/webpack.base.conf.js`
    
    _Additional code_ 
    
    `var webpack = require('webpack')`
    
    below 'module.exports' 
    
    `plugins: [
        new webpack.ProvidePlugin({
            $: "jquery",
            jQuery: "jquery"
        })
    ],`
    
3.

`cd src/main.js`

_Additional code_ 

`import $ from 'jquery'`



* install bootstrap

    1.         `npm install bootstrap --save-dev`
    
    
    2.          `cd src/main.js`

                `import 'bootstrap/dist/css/bootstrap.min.css'
                import 'bootstrap/dist/js/bootstrap.min'`


-------

[TOC]








