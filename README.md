# nodejs-crawler
[![David deps][david-image]][david-url]
[![node version][node-image]][node-url]
[![npm license][license-image]][download-url]

[david-image]: https://img.shields.io/david/bensonlove/planegirl.svg?style=flat-square
[david-url]: https://david-dm.org/bensonlove/planegirl
[node-image]: https://img.shields.io/badge/node.js-%3E=_6.0-green.svg?style=flat-square
[node-url]: http://nodejs.org/download/
[license-image]: https://img.shields.io/npm/l/xss.svg
[download-url]: https://npmjs.org/package/xss

总体结构

服务端: koa2 + mongodb + redis  
客户端: react-native (开发中)

* 客户端发送请求

* 服务端接收请求
  2.1 redis缓存中有无数据.有数据则返回;无数据则查询mongodb
  2.2 查询mongodb有数据则缓存到redis并返回,无数据则去执行爬虫
  2.3 爬虫获取的结果保存到mongodb中.

***
redis缓存数据同步方案:         
     *读: 先读缓存,不存在读DB,并将结果写入缓存    
     *写: 失效缓存,修改DB. 再次读数据时,读缓存,此时缓存已失效,则读DB
        (失效缓存而不是删除缓存的原因: 防止DB修改成功而缓存未及时清理导致读取的数据是错误数据)

客户端缓存数据方案:

打开app时,发送设备信息。服务器按照规则生成用户标识保存到数据库中并返回随机授权码