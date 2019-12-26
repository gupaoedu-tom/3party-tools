# 🚂 py12306 购票小助手（请勿用于商业用途）
分布式，多账号，多任务购票

## Features
- [x] 多日期查询余票
- [x] 自动打码下单
- [x] 用户状态恢复
- [x] 电话语音通知
- [x] 多账号、多任务、多线程支持
- [x] 单个任务多站点查询 
- [x] 分布式运行
- [x] Docker 支持
- [x] 动态修改配置文件
- [x] 邮件通知
- [x] Web 管理页面
- [x] 微信消息通知
- [ ] 代理池支持 ([pyproxy-async](https://github.com/pjialin/pyproxy-async))

## 使用
py12306 需要运行在 python 3.8 以上版本（其它版本暂未测试)

**1. 安装依赖**
```bash
git clone https://github.com/pjialin/py12306

pip install -r requirements.txt
```

**2. 配置程序**
```bash
cp env.py.example env.py
```
自动打码

（若快已停止服务，目前只能设置**free**打码模式）
free 已对接到打码共享平台，[http://www.ruokuai.com](http://www.ruokuai.com)

语音通知

语音验证码使用的是阿里云 API 市场上的一个服务商，需要到 [https://market.aliyun.com/products/56928004/cmapi026600.html](https://market.aliyun.com/products/56928004/cmapi026600.html) 购买后将 appcode 填写到配置中

**3. 启动前测试**

目前提供了一些简单的测试，包括用户账号检测，乘客信息检测，车站检测等

开始测试 -t 
```bash
python main.py -t
```

测试通知消息 (语音, 邮件) -t -n
```bash
# 默认不会进行通知测试，要对通知进行测试需要加上 -n 参数 
python main.py -t -n
```

**4. 运行程序**
```bash
python main.py
```

### 参数列表

- -t 测试配置信息
- -t -n 测试配置信息以及通知消息
- -c 指定自定义配置文件位置

### 分布式集群

集群依赖于 redis，目前支持情况
- 单台主节点多个子节点同时运行
- 主节点宕机后自动切换提升子节点为主节点
- 主节点恢复后自动恢复为真实主节点
- 配置通过主节点同步到所有子节点
- 主节点配置修改后无需重启子节点，支持自动更新
- 子节点消息实时同步到主节点

**使用**

将配置文件的中 `CLUSTER_ENABLED` 打开即开启分布式

目前提供了一个单独的子节点配置文件 `env.slave.py.example` 将文件修改为 `env.slave.py`， 通过 `python main.py -c env.slave.py` 即可快速启动


## Docker 使用
**1. 将配置文件下载到本地**
```bash
docker run --rm pjialin/py12306 cat /config/env.py > env.py
# 或
curl https://raw.githubusercontent.com/pjialin/py12306/master/env.docker.py.example -o env.py
```

**2. 修改好配置后运行**
```bash
docker run --rm --name py12306 -p 8008:8008 -d -v $(pwd):/config -v py12306:/data pjialin/py12306
```
当前目录会多一个 12306.log 的日志文件， `tail -f 12306.log`

### Docker-compose 中使用
**1. 复制配置文件**
```
cp docker-compose.yml.example docker-compose.yml
```

**2. 从 docker-compose 运行**

在`docker-compose.yml`所在的目录使用命令
```
docker-compose up -d
```

## Web 管理页面

目前支持用户和任务以及实时日志查看，更多功能后续会不断加入

**使用**

打开 Web 功能需要将配置中的 `WEB_ENABLE` 打开，启动程序后访问当前主机地址 + 端口号 (默认 8008) 即可，如 http://127.0.0.1:8008

## 更新
- 19-01-10
    - 支持分布式集群
- 19-01-11
    - 配置文件支持动态修改
- 19-01-12
    - 新增免费打码
- 19-01-14
    - 新增 Web 页面支持
- 19-01-15
    - 新增 钉钉通知
    - 新增 Telegram 通知
    - 新增 ServerChan 和 PushBear 微信推送
- 19-01-18
    - 新增 CDN 查询
- 19-12-24
    - 兼容pip3
    - 兼容Python 3.8

### 关于防封
目前查询和登录操作是分开的，查询是不依赖用户是否登录，放在 A 云 T 云容易被限制 ip，建议在其它网络环境下运行

## License

此代码转载自[Apache License](https://github.com/pjialin/py12306/blob/master/LICENSE)，如侵权请联系删除。

## 畅销书推荐
[《Spring 5核心原理与30个类手写实战》](https://item.jd.com/12650902.html)
