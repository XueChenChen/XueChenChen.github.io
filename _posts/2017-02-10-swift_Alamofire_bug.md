---
layout:     post
title:      "swift中的Alamofire的一个bug而了解url中特殊字符! "
date:       2017-02-10 19:00:00
author:     "Chen"
header-img: "img/Alamofire/alamofire.jpg"
tags:
    - 三方
    - Swift
    - Alamofire
---

[bug 原网页](https://github.com/Alamofire/Alamofire/issues/1959)

```swift
//target: Alamofire.swift line 97
// MARK: - Convenience

func URLRequest(
    method: Method,
    _ URLString: URLStringConvertible,
    headers: [String: String]? = nil)
    -> NSMutableURLRequest
{
    let mutableURLRequest: NSMutableURLRequest

    if URLString.dynamicType == NSMutableURLRequest.self {
        mutableURLRequest = URLString as! NSMutableURLRequest
    } else if URLString.dynamicType == NSURLRequest.self {
        mutableURLRequest = (URLString as! NSURLRequest).URLRequest
    } else {
        mutableURLRequest = NSMutableURLRequest(URL: NSURL(string: URLString.URLString)!)
    }

    mutableURLRequest.HTTPMethod = method.rawValue

    if let headers = headers {
        for (headerField, headerValue) in headers {
            mutableURLRequest.setValue(headerValue, forHTTPHeaderField: headerField)
        }
    }

    return mutableURLRequest
}

```
in your code URLString fix some `"` `spage` not good example:

>https://bibleverse-d0859.firebaseio.com/v/1/books.json?orderBy="version_code"&equalTo="ASV"
>(lldb) po URLString
>"https://bibleverse-d0859.firebaseio.com/v/1/books.json?>orderBy=\"version_code\"&equalTo=\"ASV\""

>(lldb) po URLString.URLString
>"https://bibleverse-d0859.firebaseio.com/v/1/books.json?>orderBy=\"version_code\"&equalTo=\"ASV\""

and your code ` NSURL(string: URLString.URLString)! `view error  please not `!`

my english not good , hope you can understand

**我们在开发中因为服务器端给的接口的特殊性,和三方处理不好而头疼,今天我们看看url中特殊字符怎么表示(我也不知道写啥,就写它吧)**

> 上方的bug我直接 "->%22, 同事的 space->%20



| 字符   | 转义       |  字符   | 转义   |  字符   | 转义   |   字符   | 转义   |
| :------| -----:|               
| backspace  |  %08 |%             |%25 |.              |%2E|>              |%3E|
| tab        |  %09 |&            | %26 |/              |%2F|?              |%3F|
|linefeed    |%0A |'             | %27 |0             |%30| @              |%40|
|creturn |   %0D |(              |%28 |...              |...|[              |%5B|
|space|       %20 |)            |  %29|9              |%39|\              |%5C|
|!            |%21|*             | %2A |:              |%3A |]              |%5D  |
| "|          %22 |+             | %2B |;              |%3B|^             | %5E|
| # |             %23|,  |          %2C|<              |%3C|_             | %5F |
|$  |          %24 |-           |   %2D|=              |%3D|`              |%60|

   

***你可以浏览器里写一下查看http请求头信息中特殊字符的编码***

      
   
   
   
   
   
