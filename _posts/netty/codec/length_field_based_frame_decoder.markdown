## 变长协议

1. 客户端通过`LengthFieldPrepender`将body length
写入报文头部(//TODO TCP or IP ?)
2. `LengthFieldBasedFrameDecoder` 根据length进行拆包

### Q & A
1. 服务端拆包的时候，如果报文实际大小大于length，则没有读取完的
直接丢弃？？？

2. 
