title: Node.js 0.12 全解
date: 2014-02-25 14:52:15
tags:
---
Node 0.12 马上就会发布, 距离 0.10 发布已有一年多的时间, 该版本不仅增加了多个重大 API, 还大幅度提升了速度. 0.12 也是 1.0 之前最后一个重大版本. 至此 Node 的 API 基本固定, 为企业化大规模应用做好准备. 


## New API

## Speed up

#### writable streams 增加 cork 支持
写入流增加了类似 `man tcp` 的 `TCP_CORK` 和 `TCP_NOPUSH` socket 选项支持. 当流处于 corked 状态, 写进流的数据会进行队列处理, 直到流回到 uncorked 状态. 这样 Node.js 可以将小数据写入组装成大数据写入, 从而减少系统调用和 TCP 往返.

http 模块已经进行升级, 在发送 chunked 的请求和响应时, 会使用 corked 模式. 通过 strace 输出可以看到增加了 writev 调用, 减少了 write 调用.

#### TLS 性能提升
tls 模块在 v0.12 中是完全不同的工作方式

Node.js v0.10 tls 建立在 net 模块之上, 在传输 stream 时透明的进行加密和解密, 从工程学角度这层是根本没有必要的, 还会带来额外消耗(更多的内存移动, 更多的 V8 VM 调入调出). v0.12 重写直接使用 libuv, 直接从 wire 获取数据进行解密, 而不用经过中间层. 

在非正式的测试中, 带来了 10% 的性能提升和更小的内存使用.

这些改变对于用户来说都是透明的, 但有一个需要用户注意 TLS 的连接现在继承自 `tls.TLSSocket`, 而之前是 `tls.CryptoStream`

#### Crypto 性能提升
多个加密算法在 0.12 中将会更快, 具体提升细节参看下面 strongloop 原文

#### Reduced garbage collector strain
multi-context 重构带来的一个重大好处是, 他极大的降低了 Node.js 内核持久句柄的数量.



#### 更好的 cluster 性能
v0.10 中的 cluster 模块依赖系统将发送过来的请求平均分配给多个 worker.
在 Solaris 和 linux 上一直重负荷工作量导致这种非配非常不平衡, v0.12 的分配方式切换
为 [round-robin](http://strongloop.com/strongblog/whats-new-in-node-js-v0-12-cluster-round-robin-load-balancing/)

#### 更快的 timers, setImmediate(), process.nextTick()
setTimeout() 和 相关方法现在使用更快和更准的 time source. 该优化在所有平台上都得到提升, 在 linux 上通过直接从 VDSO 获取当前时间(极大的降低了 gettimeofday 和 clock_gettime 的系统调用时间), 更进一步提升了性能

setImmediate() 和 process.nextTick() 也通过添加更快的分发路径提升了速度. Said 方法已经很快了, 现在也更快了

[What’s New in Node.js v0.12 – Performance Optimizations](http://strongloop.com/strongblog/performance-node-js-v-0-12-whats-new/)

## 参考

* [What’s New in Node.js v0.12: Debugging Clustered Apps with Node-Inspector](http://strongloop.com/strongblog/whats-new-nodejs-v0-12-debugging-clusters/)
* [What’s New in Node.js v0.12 – Running Multiple Instances in a Single Process](http://strongloop.com/strongblog/whats-new-node-js-v0-12-multiple-context-execution/)
* [What’s New in Node.js v0.12: Cluster Round-Robin Load Balancing](http://strongloop.com/strongblog/whats-new-in-node-js-v0-12-cluster-round-robin-load-balancing/)
* [What’s New in Node.js V0.12(video)](http://strongloop.com/developers/videos/#whats-new-in-nodejs-v012)

