## docker & docker-compose

### FAQ
* CMD, RUN, ENTRYPOINT 区别
1. RUN命令执行命令并创建新的镜像层，通常用于安装软件包
2. CMD命令设置容器启动后默认执行的命令及其参数
3. ENTRYPOINT配置容器启动时的执行命令(不会被忽略，一定被执行)
