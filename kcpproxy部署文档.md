# kcpserver部署文档
> **一、创建配置和日志目录**
```
mkdir -p /work/kcpserver/config
mkdir -p /work/kcpserver/log
```
>**二、编辑kcptun server 配置文件kcpserver.json**
```json
vi /work/kcpserver/config/kcpserver.json

{
    "target":"bn.huobipool.com:1800",
    "key":"aes", 
    "mode":"fast", 
    "log":"/work/log/kcpserver.log" 
}
```  


>> 参数说明：    
target:矿池接入点  
key:加密方式aes  
mode:模式fast1  
log:日志目录  
kcpserver监听端口默认为29900


> **三、启动Docker**
```bash
docker run -it -d --restart always --name kcpproxy -v /work/kcpproxy/config:/work/config -v /work/kcpproxy/log:/work/log --network=host registry.cn-hongkong.aliyuncs.com/huobipool-public/kcpserver:1.0
```


# kcpproxy部署文档
> **一、创建配置和日志目录**
```
mkdir -p /work/kcpproxy/config
mkdir -p /work/kcpproxy/log
```
> **二、编辑代理配置文件**
```
vi /work/kcpproxy/config/proxy_config.toml
```toml
server="127.0.0.1:12948,127.0.0.1:12949"
port="6666,7777"
```
>**三、编辑kcptun clien 01配置文件**
```json
vi /work/kcpproxy/config/kcpclient_01.toml


{
    "remoteaddr":"47.243.17.167:29900",
    "key":"aes",
    "mode":"fast",
    "localaddr":":12948",
    "log":"/work/log/kcpclient_01.log"
}
```  
>> 参数说明：  
remoteaddr:kcptun server地址  
key:加密方式aes  
mode:模式fast1  
localaddr:监听端口，默认12948  
log:日志目录  

  
> **四、编辑kcptun clien 02配置文件**
```json
vi /work/kcpproxy/config/kcpclient_02.toml


{
    "remoteaddr":"47.243.17.167:29900",
    "key":"aes",
    "mode":"fast",
    "localaddr":":12949",
    "log":"/work/log/kcpclient_02.log"
}
```
>> 参数说明：  
remoteaddr:kcptun server地址  
key:加密方式aes  
mode:模式fast1  
localaddr:监听端口，默认12948  
log:日志目录
> 启动Docker
```bash
docker run -it -d --restart always --name kcpproxy -v /work/kcpproxy/config:/work/config -v /work/kcpproxy/log:/work/log --network=host registry.cn-hongkong.aliyuncs.com/huobipool-public/kcpproxy:1.0
```
# 测试说明
连接bn.huobipool.com:1800接入点，使用矿机模拟程序模拟3w台矿机压测  
代理信道延时6ms，处理3w台矿机共599786条share数据耗时20s
# kcptun说明
![kcptun 图标](https://github.com/6612172/kcptun/raw/master/kcptun.png)  

kcptun参数配置详细说明[README.md](https://github.com/6612172/kcptun)
