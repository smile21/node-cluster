# 特性

node-cluster 是一个简单易用的 NodeJS 类库，帮助开发人员快速地搭建基于NodeJS的服务程序：

* 基于master + worker 模式，能够有效利用多核处理器;
* 支持多端口监听，master 传递请求端的 socket fd 给各个 worker，性能损失极低;
* 同一端口下的多个worker之间提供简单的负载均衡支持;
* 支持对 worker 进程数的监控，支持单个 worker 根据已处理的请求数自动消亡;
* 支持 master 和 worker 的平滑重启，不丢失请求.

# 使用

demo目录下提供了一个典型的示例，你可以通过下列命令启动这个服务：

	$ nohup node demo/main.js &

其中:

* main.js 是master进程，通过 register 方法注册 worker进程，并通过 dispatch 进行工作; 除此之外，master 进程不需要做任何工作，你就可以实现一个高稳定性的生产服务;
* worker/admin.js 提供了监听在 33749 端口上的 HTTP 服务; 通过NodeJS 原生的http模块实现，demo中仅提供了 hello world的示例;
* worker/api.js 提供监听在 8080 端口上的Socket应答服务.

# 原理

请参考我的同事windyrobin的这篇文章：
http://club.cnodejs.org/topic/4f16442ccae1f4aa27001081 
本文的 node-cluster 在核心功能的实现原理上没有任何新意，只是对代码的组织做了更友好的封装，同时加入了一些基于稳定性考虑的特性.

# 性能

我只能说, node-cluster不会让你失望.
你可以拿 demo 程序实际压测一下，33749端口，开一个worker; 然后拿来和用NodeJS原生的http模块构造的服务进行对比.

# 注意

* worker 进程中的 remain 变量，是判断一个 worker 是否空闲的依据; 因此我强烈建议在你的应用程序 worker 进程中，采用更优雅的幂等操作对其计数，并且通过 worker.monset('remain', remain.value) 的方法回写;

