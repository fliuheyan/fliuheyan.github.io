## 定长decoder

### feature

1. 没有对应的encoder,所以发送的时候，如果body长度不足，一般是用空格
补齐，所以不常用.

对于`new FixedLengthFrameDecoder(3)`来说    
以下frame会被decode为
```
   +---+----+------+----+
   | A | BC | DEFG | HI |
   +---+----+------+----+
```

```
   +-----+-----+-----+
   | ABC | DEF | GHI |
   +-----+-----+-----+
```

`FixedLengthFrameDecoder`的构造器

//TODO frame里都是bytes，frameLength这里对应的是字符流？？？
所以是构造了一个frameLength*8 的buffer么
```
public FixedLengthFrameDecoder(int frameLength) {
    if (frameLength <= 0) {
        throw new IllegalArgumentException(
                "frameLength must be a positive integer: " + frameLength);
    }
    this.frameLength = frameLength;
}
```
