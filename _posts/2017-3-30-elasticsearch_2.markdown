---
layout: post
title:  Elasticsearch（二）, scrapy, Flume & Kafka, Flutter, 数据结构 & 普通CS算法
category: AI 
---

# Elasticsearch

## ELK的配置部署（续）

5.Bootstrap checks failing

当配置的host不是localhost的时候，ES会进行Bootstrap checks。其主要目的是增加ES能够获得的各种资源。一般不推荐在实际生产环境中，关闭Bootstrap checks。

>max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]

修改/etc/security/limits.conf：（需要root权限）

{% highlight text %}
es  soft    nproc     65536
es  hard    nproc     65536
es  soft    nofile    65536
es  hard    nofile    65536
{% endhighlight %}

重新登录es用户后，修改生效。

在bin/elasticsearch的开头添加：

`ulimit -n 65536`

>注：ulimit增加的资源数，不能超过limits.conf中的数量，否则会报错。

>max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

修改/etc/sysctl.conf：（需要root权限）

`vm.max_map_count=262144`

执行`sysctl -p`使配置生效。

参考：

http://stackoverflow.com/questions/42300463/elasticsearch-5-x-bootstrap-checks-failing

5.数据存储

默认情况下，data和log都在ES文件夹下的同名文件夹下。可在config/elasticsearch.yml中修改之。

## Java REST Client

ES的Client支持多种语言。其中，Java语言有两种API：Java API和Java REST API。其中，前者对后者的调用进行了封装，但由于REST命令可以直接在kibana中调试，实际使用中，反而后者更方便一些。

Java REST API的示例参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/elasticsearch/es_client_hello

其中，test1函数给出了基本的查询示例。test2函数给出了json格式查询的示例，test3函数对查询返回的json数据采用jackson的树模型进行解析，test3函数对查询返回的json数据采用jackson的流模型进行解析。

更全面的示例参见：

https://github.com/Top-Q/elasticsearch-client

## logstash-output-jdbc

`bin/logstash-plugin install logstash-output-jdbc`

https://github.com/theangryangel/logstash-output-jdbc/blob/master/examples/mysql.md

## Spring

Spring也提供了对ES的支持。

SpringBoot官方的ES文档：

http://docs.spring.io/spring-data/elasticsearch/docs/current/reference/html/

这篇文章里的Table 2写的很好，可以方便用户快速掌握最常用的查询语法。

SpringBoot官方的ES示例：

https://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples/spring-boot-sample-data-elasticsearch

Spring的问题在于它的发布由于和ES并无关联，因此更新比较迟。而且内部由于使用的是ES Java API，对ES版本有要求，通用性上不如Java REST API。

参考：

http://blog.720ui.com/2016/springboot_02_data_elasticsearch/

Spring Boot揭秘与实战（二）数据存储篇-ElasticSearch

https://juejin.im/entry/58b56c4a8d6d81005765fc73

SpringBoot整合Elasticsearch

## 参考

http://cloud.51cto.com/art/201505/476450.htm

五类Elasticsearch扩展性插件推荐

http://blog.csdn.net/cnweike/article/details/33736429

Elasticsearch基础教程

http://blog.csdn.net/a809146548/article/details/52371110

Logstash使用详解

http://www.cnblogs.com/ajianbeyourself/p/5529575.html

Elasticsearch教程-从入门到精通

http://www.freebuf.com/sectool/78225.html

Elk大数据查询系列：Elasticsearch与Logstash基础篇

http://www.tuicool.com/articles/YR7RRr

ELK搭建实时日志分析平台

http://467754239.blog.51cto.com/4878013/1700828/

ELK 日志分析系统

https://www.ibm.com/developerworks/cn/opensource/os-cn-elk/

集中式日志系统ELK协议栈详解

https://es.xiaoleilu.com/

Elasticsearch权威指南（中文版）

http://udn.yyuap.com/doc/logstash-best-practice-cn/

logstash最佳实践

https://zhuanlan.zhihu.com/p/24428355

使用ElasticSearch踩过的坑

https://zhuanlan.zhihu.com/p/25723815

教你快速使用Tensorflow/Elasticsearch实现全文的图片搜索

http://www.cnblogs.com/buzzlight/p/logstash_elasticsearch_kibana_log.html

使用logstash+elasticsearch+kibana快速搭建日志平台

http://blog.csdn.net/longxibendi/article/details/35237543/

ELK入门学习资源索引

http://www.jianshu.com/p/0b4346f503e3

探索elasticsearch。该文包含如何使用ES进行TF/IDF的方法

https://my.oschina.net/taogang/blog/983586

ElasticSearch对比Splunk

https://mp.weixin.qq.com/s/jZ_jM6yUPK8Ev7FSwdgTIA

360私有云平台Elasticsearch服务初探

https://mp.weixin.qq.com/s/osOggCYvzun6X6yquD7cYg

浅析ElasticSearch原理

https://mp.weixin.qq.com/s/j_9PCwWoGu9cZM9sD1klog

Elasticsearch性能监控（一）

http://bbotte.com/logs-service/use-elk-processing-logs-multiple-log-file-send/

ELK日志服务使用-filebeat多文件发送

https://www.digitalocean.com/community/tutorials/how-to-map-user-location-with-geoip-and-elk-elasticsearch-logstash-and-kibana

How To Map User Location with GeoIP and ELK (Elasticsearch, Logstash, and Kibana)

https://mp.weixin.qq.com/s/kxBxaK72ovaMHQgumXALmw

Elasticsearch性能监控（二）

http://blog.csdn.net/qq_21383435/article/details/79367457

linux下ElasticSearch.6.2.1与head、Kibana、X-Pack、SQL、IK、PINYIN插件的配置安装

https://mp.weixin.qq.com/s/Wzrt7H9gDIUQn7KEc0qZHQ

在Python中使用Elasticsearch

# scrapy

scrapy是一个Python写的网页抓取分析工具。网页抓取分析的学名叫做“Web scraping”，可在wiki上获得更多的相关信息。

官网：

https://scrapy.org/

安装：

`sudo apt install python-scrapy`

`scrapy crawl csdn`

新建工程：

`scrapy startproject tutorial`

参考：

https://segmentfault.com/a/1190000000583419

一个中文简易教程。

https://github.com/scrapy/dirbot

官方例程。

http://www.cnblogs.com/fengzheng/p/4974509.html

另一个中文简易教程。

https://mp.weixin.qq.com/s/nIcUBS0lRrOwVUHoWmKecw

爬虫系列之使用scrapy框架

https://mp.weixin.qq.com/s/A2QNr4-LTUNy-H8zE0OgXA

教你用Scrapy建立你自己的数据集

https://mp.weixin.qq.com/s/i-umuOi8jGw8dMQmG44liQ

如何租到靠谱的房子？Scrapy爬虫帮你一网打尽各平台租房信息！这篇blog中，还有如何用Kibana处理数据的内容

https://mp.weixin.qq.com/s/hzl8D-AhpCwqZVSLTK56XQ

Python爬虫--Scrapy入门

# Flume & Kafka

Flume和Kafka都是日志系统。

Flume官网：

https://flume.apache.org/

Kafka官网：

https://kafka.apache.org/

以下是它们的比较：

http://www.cnblogs.com/ibyte/p/5830715.html

Flume与Kafka区别

http://www.aichengxu.com/view/2412170

kafka和flume的对比

http://www.cnblogs.com/lishouguang/p/4560858.html

flume使用场景 flume与kafka的比较

https://mp.weixin.qq.com/s/bjlDHFLwxjej2t8iDhVb1A

Spark Streaming消费Kafka数据的两种方案

https://mp.weixin.qq.com/s/o-zfrJS5Ito1kWPBJUIryg

Kafka相关资源

https://mp.weixin.qq.com/s/l0AL89M0xPbWMFj6U7yYZw

消息中间件选型分析：从Kafka与RabbitMQ的对比看全局

https://mp.weixin.qq.com/s/oKBUb2IbG_h1xDhL42TbuQ

360开源又一力作——KafkaBridge：让操作kafka更简单！

https://mp.weixin.qq.com/s/P6K1tXuBZHaaizwGVo-22A

Kafka的API那么多，到底该怎么选？

# Flutter

Flutter是Google用以帮助开发者在Ios和Android两个平台开发高质量原生应用的全新移动UI框架。Beta1版本于2018年2月27日在2018世界移动大会上公布。

官网：

https://flutter.io/

参考：

https://mp.weixin.qq.com/s/pU75twMDry4VUYtTHeV_IQ

一文深入了解Flutter界面开发

https://mp.weixin.qq.com/s/yPvaB7sLuJoGfsjj7x7wcg

深入理解Flutter的编译原理与优化

https://mp.weixin.qq.com/s/SWP7Nu9DEUhvAyvdIG-Ixw

聊一聊Flutter Engine线程管理与Dart Isolate机制

https://mp.weixin.qq.com/s/cJjKZCqc8UuzvEtxK1BJCw

Flutter的原理及美团的实践

https://mp.weixin.qq.com/s/l6xvmnLE6HfRtw6upo6yUA

高效开发与高性能并存的UI框架——携程Flutter实践

# 数据结构 & 普通CS算法

https://mp.weixin.qq.com/s/52JFdyaLwZq7nWiZq6z6qA

二叉堆是什么鬼？

https://mp.weixin.qq.com/s/4V1E2L14c3k-5i0c-JyTVQ

如何在10亿数中找出前1000大的数

https://mp.weixin.qq.com/s/yNn57Uw_-itzZXCSEAzrJw

一起搞定面试中的二叉树（一）

https://mp.weixin.qq.com/s/msv2NZ0aG96d3xQhX3HhNA

一起搞定面试中的二叉树（二）

https://mp.weixin.qq.com/s/xttZJVR0r5vssOJ19sBHGQ

快速排序算法

https://mp.weixin.qq.com/s/jQ6fEi_K2RSesJxZn_kujg

外部排序

https://mp.weixin.qq.com/s/QAAfjSeZtfd3PSN3wAP8RQ

什么是外部排序？

![](/images/img2/DS.jpg)

![](/images/img2/sort.jpg)

http://blog.csdn.net/u013709270/article/details/53470428

跳跃表的原理及实现

https://mp.weixin.qq.com/s/t7Q0slX3q8Qlhg8F8pXrZQ

如何找到字符串中的最长回文子串？

https://www.cnblogs.com/tuding/p/7335853.html

双调排序Bitonic Sort，适合并行计算的排序算法

https://mp.weixin.qq.com/s/fP36LYFjAqqfP3rbYxAJlA

优秀的排序算法如何成就了伟大的机器学习技术

![](/images/img2/sort.gif)

# ML参考资源+

https://mp.weixin.qq.com/s/Sez56UaCAt3Aspn7OTFj_Q

让AI来一场“简单”的黄金点游戏

https://mp.weixin.qq.com/s/1dOFsRCuTbm6UI1Pd5MNjw

快速扩展随机树路径规划算法

https://mp.weixin.qq.com/s/C50IBm8EgrE13MOPMgPseQ

基于背景和潜在异常字典构造的高光谱图像异常目标检测

https://mp.weixin.qq.com/s/doi26r9AVIMbpkZ01wsCZA

北京大学何洋波博士《因果推断和因果图模型》机器学习报告

https://mp.weixin.qq.com/s/NbEki2ByMmA93DyoQF1yjA

运满满如何将机器学习应用于车货匹配和公路干线价格预测？

https://mp.weixin.qq.com/s/R7Qud_x6DGqbHAOl7OMruQ

一文带你入门算法分发！

https://mp.weixin.qq.com/s/s2XpGne0sAqnuN-u6g5k1A

资金流因子全面测试兼正交化方法详解

https://mp.weixin.qq.com/s/oblwg01BUIJjVhBeHa9HQQ

程序员分析了50万条拼多多商品数据，告诉你到底是消费升级还是降级！

https://mp.weixin.qq.com/s/4HVkEnk6_hONWJYdi8L53g

闲聊结构化预测（structured learning）

https://mp.weixin.qq.com/s/GP_t_fhklpVNhC6J6i0YbA

再聊聊自然语言处理中的结构化预测问题

https://mp.weixin.qq.com/s/_AHqFqf1zFNtBifayiTfXQ

你应具备这3大统计学常识&大数据思维

https://mp.weixin.qq.com/s/G6-XbqCO5iuwGDen-r6aWg

唐宏:基于概率主题模型的高分辨率遥感图像非监督语义分割

https://mp.weixin.qq.com/s/21LVssq2ynMgp_x1lwSC9w

基于稀疏自表示与模糊双C均值聚类的SAR图像分割

https://mp.weixin.qq.com/s/Oi1yJty_guzTUbRGZ5MuEw

使用高斯过程的因果推理：GP CaKe的基本思路

https://mp.weixin.qq.com/s/vLvjo8Cso_bYEuW-AFoyiA

南大周志华等人提出无组织恶意攻击检测算法UMA

https://mp.weixin.qq.com/s/l4W7qmKJ4Pc-rlF-KvhrPA

机器学习在信用评分卡中的应用
