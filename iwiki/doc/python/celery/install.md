# 环境搭建

## 开始

在开始学习或者使用celery的过程中你需要做到以下几点

1. Broker Celery中支持三种方式分别为
2. 安装 Celery

## 安装

官网安装提供了几种的安装方式

+ 直接 `pip` 安装
+ 捆绑安装
+ 源码安装

创建虚拟环境

```python
python3 -m venv py368
```


安装 Celery

```python
pip install -U Celery
```

## Broker

### Redis作为Broker

#### 安装 celery[redis] 依赖
该位置推荐捆绑安装，如果直接运行 `pip install redis` 则安装的最新版本，可能会有版本兼容问题，所以推荐使用捆绑安装会自动解决版本之间的兼容问题
```
pip install -U "celery[redis]"
```
#### 配置

```python
redis://:password@hostname:port/db_number
```

## 应用
Celery的应用具体分为两种

+ 实例应用：实例化对象(测试，学习)
+ 模块应用：项目中独立的模块(生产功能模块)

### 实例应用

实例应用就是初始化Celery获取一个实例化对象，该实例对象会有两个参数分别为 `name` 和 `broker`
```python
# 创建一个文件 touch tasks.py
from celery import Celery
app = Celery('tasks', broker='redis://localhost:6379/0')
@app.task
def add(x, y):
    return x + y
```

### 模块化应用




## 应用的配置

Celery中的应用配置可以分为三种方式

+ 直接在对象的相应属性赋值
+ 基于对象的 `update` 方法一次可以配置多个项
+ 配置文件的配置

示例：直接配置
```python
app.conf.timezone = "Asia/Shanghai"
```

示例：基于 `update` 配置

```python
app.conf.update(
    task_serializer='json',
    accept_content=['json'],  # Ignore other content
    result_serializer='json',
    timezone='Asia/Shanghai',
    enable_utc=True,
)
```

示例：配置文件配置

1. 创建配置文件
2. 加载配置文件 `app.config_from_object()`


示例：基于配置文件的方式启动应用，配置文件
```python
# @FileName celeryconfig.py

broker_url = 'redis://localhost:6379/0'
task_serializer = 'json'
result_serializer = 'json'
accept_content = ['json']
timezone = 'Asia/Shanghai'
enable_utc = True

# 校验语法是否正确
# python -m celeryconfig
```
示例：如何使用配置文件
```python
# @File : tasks_config.py

from celery import Celery
app = Celery() # 创建 Celery 实例 如果不填写 默认名称为 __main__:0x7f7f16596668
app.config_from_object("celeryconfig")


@app.task
def add(x, y):
    return x + y

```

示例：启动实例应用

```python
celery -A tasks_config worker --loglevel=INFO
```
注意：这里的启动文件要和 文件名称 相同
```
celery -A tasks_config worker --loglevel=INFO
/root/py368/lib64/python3.6/site-packages/celery/platforms.py:797: RuntimeWarning: You're running the worker with superuser privileges: this is
absolutely not recommended!

Please specify a different user using the --uid option.

User information: uid=0 euid=0 gid=0 egid=0

  uid=uid, euid=euid, gid=gid, egid=egid,

 -------------- celery@lipanpan v5.0.5 (singularity)
--- ***** -----
-- ******* ---- Linux-3.10.0-693.2.2.el7.x86_64-x86_64-with-centos-7.9.2009-Core 2021-04-26 14:27:36
- *** --- * ---
- ** ---------- [config]
- ** ---------- .> app:         __main__:0x7f7f16596668
- ** ---------- .> transport:   redis://localhost:6379/0
- ** ---------- .> results:     disabled://
- *** --- * --- .> concurrency: 1 (prefork)
-- ******* ---- .> task events: OFF (enable -E to monitor tasks in this worker)
--- ***** -----
 -------------- [queues]
                .> celery           exchange=celery(direct) key=celery

[tasks]
  . tasks_config.add

```



## 运行 Worker
```python
celery -A tasks worker --loglevel=INFO

```
---
```
celery -A tasks worker --loglevel=INFO
/root/py368/lib64/python3.6/site-packages/celery/platforms.py:797: RuntimeWarning: You're running the worker with superuser privileges: this is
absolutely not recommended!

Please specify a different user using the --uid option.

User information: uid=0 euid=0 gid=0 egid=0

  uid=uid, euid=euid, gid=gid, egid=egid,

 -------------- celery@lipanpan v5.0.5 (singularity)
--- ***** -----
-- ******* ---- Linux-3.10.0-693.2.2.el7.x86_64-x86_64-with-centos-7.9.2009-Core 2021-04-25 22:49:02
- *** --- * ---
- ** ---------- [config]
- ** ---------- .> app:         tasks:0x7fc7f4830898
- ** ---------- .> transport:   redis://localhost:6379/0
- ** ---------- .> results:     disabled://
- *** --- * --- .> concurrency: 1 (prefork)
-- ******* ---- .> task events: OFF (enable -E to monitor tasks in this worker)
--- ***** -----
 -------------- [queues]
                .> celery           exchange=celery(direct) key=celery


[tasks]
  . tasks.add

```

## 调用任务

调用任务的时候会返回 AsyncResult 对象

图示：返回 AsyncResult 对象，对象封装了任务的结果的各种状态

![backend](http://39.105.100.168:8888/images/celery_install_task_delay.png)

获取返回结果，如果想要获取返回结果，需要配置我们的结果存储的方式，如果不进行配置则获取结果会报错

图示：如果相应的任务实例不配置 backend 则获取函数的结果会报错


![AsyncResult](http://39.105.100.168:8888/images/celery_install_asyncResult.png)



## 配置结果存储后端
Celery支持的结果存储后端有很多方式，例如MySQL,MongoDB,Redis等，在接下来的案例中使用Redis作为结果后端

```python
from celery import Celery

app = Celery('tasks', broker='redis://172.17.150.44:6379/0',backend='redis://172.17.150.44:6379/1')

@app.task
def add(x, y):
    return x + y

```

![配置结果后端](http://39.105.100.168:8888/images/celery_config_backend.png)


示例：
```
In [1]: from tasks import add

In [2]: r = add.delay(1,2)

In [3]: r.get()
Out[3]: 3

In [4]: r.status
Out[4]: 'SUCCESS'

```

## 总结

在初学Celery中需要注意事项

1. 个人学习搭建的环境是实例应用还是模块化应用
2. 配置过程中一定不能缺少相应的组件分别为 `broker`, `backend`, `任务名称` 是有默认值的可以不配置
3. 学习过程中查询资料注意版本 Celery 在 `4.x` 版本和之前的版本在配置项有较大的差异， `6.x` 以后配置项都会小写
4. 启动 `Celery Worker` 的时候一定要注意路径
5. 一定深入的了解 `AsyncResult` 对象
























Celery 扮演生产者和消费者的角色,

Celery Beat : 任务调度器. Beat 进程会读取配置文件的内容, 周期性的将配置中到期需要执行的任务发送给任务队列.
Celery Worker : 执行任务的消费者, 通常会在多台服务器运行多个消费者, 提高运行效率.
Broker : 消息代理, 队列本身. 也称为消息中间件. 接受任务生产者发送过来的任务消息, 存进队列再按序分发给任务消费方(通常是消息队列或者数据库),可以是 RabbitMQ 、 Redis, 官方推荐 RabbitMQ.
Producer : 任务生产者. 调用 Celery API , 函数, 而产生任务并交给任务队列处理的都是任务生产者.
Result Backend : 任务处理完成之后保存状态信息和结果, 以供查询, 可以是AMQP, Redis, memcached, MongoDB, SQLAlchemy. Django ORM, Apache Cassandra等.
