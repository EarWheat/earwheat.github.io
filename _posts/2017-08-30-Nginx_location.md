---
layout: post
title: nginx location匹配规则
date: 2017-08-30 
tag: nginx
---

## nginx location匹配规则


工作需要负责配置管理nginx，现学现记，趁热打铁。文章内容非原创，转自[http://www.nginx.cn/115.html](http://www.nginx.cn/115.html)

### location匹配命令


- ~      #波浪线表示执行一个正则匹配，区分大小写
- ~*     #表示执行一个正则匹配，不区分大小写
- ^~    #^~表示普通字符匹配，如果该选项匹配，只匹配该选项，不匹配别的选项，一般用来匹配目录
- =      #进行普通字符精确匹配
- @     #"@" 定义一个命名的 location，使用在内部定向时，例如 error_page, try_files


### location 匹配的优先级(与location在配置文件中的顺序无关)

- = 精确匹配会第一个被处理。如果发现精确匹配，nginx停止搜索其他匹配。
- 普通字符匹配，正则表达式规则和长的块规则将被优先和查询匹配，也就是说如果该项匹配还需去看有没有正则表达式匹配和更长的匹配。
- ^~ 则只匹配该规则，nginx停止搜索其他匹配，否则nginx会继续处理其他location指令。
- 最后匹配带有\~和~*的指令，如果找到相应的匹配，则nginx停止搜索其他匹配；当没有正则表达式或者没有正则表达式被匹配的情况下，那么匹配程度最高的逐字匹配指令会被使用。


例子


```
location  = / {
  # 只匹配"/".
  [ configuration A ] 
}
location  / {
  # 匹配任何请求，因为所有请求都是以"/"开始
  # 但是更长字符匹配或者正则表达式匹配会优先匹配
  [ configuration B ] 
}
location ^~ /images/ {
  # 匹配任何以 /images/ 开始的请求，并停止匹配 其它location
  [ configuration C ] 
}
location ~* .(gif|jpg|jpeg)$ {
  # 匹配以 gif, jpg, or jpeg结尾的请求. 
  # 但是所有 /images/ 目录的请求将由 [Configuration C]处理.   
  [ configuration D ] 
}
```


请求URI例子:

    / -> 符合configuration A
    /documents/document.html -> 符合configuration B
    /images/1.gif -> 符合configuration C
    /documents/1.jpg ->符合 configuration D

@location 例子
````
error_page 404 = @fetch;

location @fetch(
    proxy_pass http://fetch;
)
````