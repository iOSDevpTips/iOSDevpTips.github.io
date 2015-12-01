---
layout: post
title: "Alamofire源码详解"
category:  Swift
---

# [Alamofire](https://github.com/Alamofire/Alamofire)

## Alamofire.swift
声明了一个URLStringConvertible协议，使URL支持[RFC 2396](http://tools.ietf.org/html/rfc2396)。
然后扩展了String，NSURL，NSURLComponents，NSURLRequest，使其支持URLStringConvertible协议。
又声明了URLRequestConvertible协议，扩展到NSURLRequest中。

实现了request，upload，download方法。

## Request.swift
### TaskDelegate
Request类内部定义了一个TaskDelegate类。提供一个队列用来执行Task完成之后的所有操作。
传入NSURLSessionTask来初始化，deinit的时候取消队列中的所有操作

用闭包方式来重载taskWillPerformHTTPRedirection，taskDidReceiveChallenge，taskNeedNewBodyStream，taskDidCompleteWithError这几个方法，SDK是用Delegate实现的，改成闭包方式更方便。
### DataTaskDelegate
DataTaskDelegate继承TaskDelegate和NSURLSessionDataDelegate。
用闭包方式来重载dataTaskDidReceiveResponse，dataTaskDidBecomeDownloadTask，dataTaskDidReceiveData，dataTaskWillCacheResponse，原因同上。
### Request
主要信息都放在TaskDelegate属性里面了，包括Task，request，response，progress等。
同时，TaskDelegate可能是UploadTaskDelegate，DataTaskDelegate，DownloadTaskDelegate这些子类。
还有鉴权的方法。
支持挂起，恢复和取消。
另外还扩展了Request支持Printable和DebugPrintable协议。

## ParameterEncoding.swift
定义了几种参数类型：URL，JSON，PropertyList，Custom。
encode方法就是对这几种参数做参数拼接。

## Manager.swift
### SessionDelegate
Manager类里面定义了一个final类：SessionDelegate，实现了NSURLSession中的各种协议，包括NSURLSessionDelegate, NSURLSessionTaskDelegate, NSURLSessionDataDelegate, NSURLSessionDownloadDelegate
SessionDelegate内部采用字典存储，key为Task的taskIdentifier字段。
支持下标索引
### Manager
单例
```Swift
    public static let sharedInstance: Manager = {
        let configuration = NSURLSessionConfiguration.defaultSessionConfiguration()
        configuration.HTTPAdditionalHeaders = Manager.defaultHTTPHeaders

        return Manager(configuration: configuration)
    }()
```

声明了queue，session，delegate，startRequestsImmediately，backgroundCompletionHandler。
