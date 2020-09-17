## Netty

### Basic Concept

### Channel
an open connection to an file/network socket, can perform I/O operations, reading or writing.
Channel -> Channel Pipeline -> Channel Config


`OP_ACCEPT`
`OP_CONNECT`
`OP_READ`
`OP_WRITE`

1. New channel registers with selector
2. Selector handles notification of state changes
3. Previously registered channels
4. Selector.select() blocks until new state changes are received or timeout
5. Checks if there were state changes
6. Handle state changes
7. Execute other tasks 

Netty 用一套API同时支持AIO和BIO
设置SO_TIMEOUT socket flag, 如果超时，则抛出SocketTimeoutException.
Netty catches this exception and continues the processing event loop to try again.

#### Zero copy
Feature available with NIO & Epoll transport.
It allows you to quickly and efficiently move data from a file system to the network without copying from kernel space to user space.
Directly from IO -> JVM heap.

#### Channel Pipeline
Intercepting Filter(拦截过滤器模式). like Unix Pipes
Holds all the ChannelHandler instances.

#### Channel Handler
* Transforming data bytes -> Object(inbound handler) Object -> Bytes(outbound)
* error handle
* registe/deregist from EventLoop
* callback

#### ChannelFuture
1. java 老款`Future`的问题：必须手动的去检查completed or block until it does.(no callback)

2. `ChannelFuture`和Java8 `CompletbleFuture`比较

### EventLoop
Control flow, multithreading, concurrency.

* An EventLoopGroup contains >=1 EventLoops
* An EventLoop is bound to a single Thread for its lifetime.
* many/one Channel map to one EventLoop 


### ChannelPipeline

### ChannelHandlerContext



### Bootstrapping

## Codecs

bytes -> Object `decode`
Object -> bytes `encode`

Reason: network data is always a series of bytes.




### ByteBuf
Netty's alternative to `ByteBuffer`

* Switching between reader & writer doesn't require calling ByteBuffer's `flip()` method.

#### ByteBuf

#### ByteBufHolder

#### Usage pattern
1. store in jvm heap

2. Direct buffer (allocate outside jvm heap)

#### CompositeByteBuf (like tuple...)
include multiple buffers (direct and nondirect allocations)


