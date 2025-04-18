## 基于大模型/深度学习的多平台消息筛选整合和分类标记工具
吉林大学25届大创实验项目-MessFiltrator(MF)
***
### 总体设计
#### BackGround
>随着社交平台日益普及，许多学生与工人尤其是管理者在生活中需要处理大量的消息尤其是通知等关键信息，然而由于厂商竞争,平台差异等多种因素,从微信到QQ再到各种邮箱之间形成了一个个信息孤岛，这给人们及时获取所需关键信息带来了极大的不便，所以我们想做一个工具类软件，能够对各个常用平台的信息进行抓取，然后借助深度学习/大模型的帮助下对信息进行筛选与分类，标注时段等操作后再借助易于查看的GUI界面统一展现给用户读取，辅助用户更好地管理自身信息，避免因为在大量信息轰炸下漏掉某些重要消息或是浪费大量时间在各个平台和频道里自行翻找与整合。

鉴于小组成员实际项目开发与该类作业的经验不足,若有更好的意见和提出问题请PR(pull request)和issue或是通过聊天软件交流 :smile_cat:
#### Core
该工具本质上被设计看作是一个针对聊天平台的信息搬运处理软件，
以<font color = red>信息的准确性</font>传递为第一标准，而信息本身的真伪交由用户自身判断。
作为一个<font color = red>信息二次接收集中端</font>，用户一般情况下只考虑<font color = red>只读</font>权限
...待补充
全程基于Core进行功能的开发

#### Expected Fuction
##### 基础功能
实现对QQ，Wechat的聊天文件信息的提取解码，将源数据通过自建模型/大模型api的筛选分类及总结后进行保存，
最后以易于读取的GUI界面呈现给用户，从而实现单智能终端接收信息。（类似ai搜索,但针对聊天信息)
##### 扩展功能(可选)
* 允许将收取内容按照时间段（XX年XX月XX日到 YY年YY月YY日）进行分类查看，
这一点十分有利于用户区分新旧信息，免于重复浏览过时信息
* 实现对多平台收取内容的统一搜索，功能可能不如wechat，qq各自本身搜索强大，但能提供一定的便利（尤其是在[PE2](#pe2)实现的前提下
* 待提出

***
### 开发流程规划
>根据每项任务量的不同，可能存在一人负责多个任务以及多人共同负责一个任务的情况
由于编写者自身的认知水平有限，以下部分请多多结合自身调查并加以反馈，包括但不限于任务的划分
并希望大家也可以充分利用ai带来的思路和源码作出一份贡献
#### 1.平台源数据提取及格式化
>负责人：暂未分配
问题描述：Wechat以及QQNT(新版QQ)的聊天信息实际是通过轻量级数据库SQLite存储于本地的，文件均通过AES进行加密，需要对这两个平台的
聊天信息文件进行提取和解密，并进一步处理导出为可用的用户数据或是清洗为用于训练模型的数据

可能的解决方案：
* 微信数据提取可以参考github上( :star: 6.9k仍在维护更新)的项目[PyWxDump](https://github.com/xaoyaoo/PyWxDump)提供的源码以及教程,可以提取转换为html,csv等格式
* QQNT的数据提取则可以学习专门解密查看QQ聊天文件的社区网站[QQDecrypt](https://qq.sbcnm.top/)中提供的教程
以及其中标明的针对各平台提取的github项目
* 如果需要训练模型对数据进行清洗，...待补充

#### 2.数据处理模型搭建或大模型调用
>负责人：暂未分配
##### 思路1：借助深度学习中训练文本相关模型
##### 思路2：借助已有的大模型的api进行调用
##### 思路3：简单部分通过自己训练，复杂情况则借助大模型api

#### 3.存储结构以及其他功能实现
>负责人：暂未分配

#### 4.前端UI设计与搭建
>负责人：暂未分配





***
### 可能存在的问题及更多发展方向

#### Possible Problem

1.消息受到重复表述以及更正，并有可能表达方式有所变化
>例如：缩写和语境下表述，如老师说马克思主义原理在4月17号考试，AA同学没看到，询问群里其他的人，大家几个人对其回复“在4月17日”，“马原4月17日考”，以及老师后来说“马原改为4月10号考试”，这一类情况有时发生。

可能会导致重复收录某些信息，这对模型分辨相似信息以及上下文关联的能力作出了一定需求,
    而且内容本身也受到交流习惯影响，在无法保证识别的情况下考虑收为一个事项的信息列表,并标注来源和时间并按照时间戳进行排序
    从而交由用户本人进行判断 （优先保证消息的准确传递）

2.<a id = "p2"></a>第一接受端的更新不及时带来的影响
>生活中经常出现此类情况：如老师发签到了，可是我的通知栏却迟迟没反应，
听同学一说立马打开学习通，就一下子蹦出来了（和一个大大的“迟到”），便是第一接受端信息未能及时更新的情况

(vital)二次收取的前提是第一接收端的信息数据已经完成接受，
    但很多时候第一接收端的数据本身更新不及时或未启动无法接收，甚至要用户自己打开对应平台方可收到
    这一点带来的麻烦包括但不限于用户可能需要将各个平台逐一启动再进行下一步的操作

3.待提出

#### Possible Expand
~~1.(crazy idea)改变工具内核，令多平台与该工具建立同步映射，将该工具作为多个平台的统一终端，兼具发送和接受功能
    预计结果：极难实现，能做到一部分也可能是工具下架以及涉及商业违法~~<br>

2.基础功能的平台数量扩展：<a id="pe2"></a>
* 出于对Python爬虫的部分认知，考虑借助爬虫爬取<font color = yellow>校园oa网站 </font> 以获取更多相关通知信息作为源数据
* 通过生活中大部分邮箱账号可登入不同邮箱平台的现象以及[CSDN上有关邮箱提取的介绍](https://blog.csdn.net/weixin_35757704/article/details/132623569)，
认为实现对<font color = yellow>邮箱信息</font>的提取具有一定的可行性，适合作为考虑项
    
> 可能存在的问题：本校网站可能存在反爬虫设置（未验证），并且开发爬虫对技术能力有一定要求，可能投入的学习成本较高；
        提取邮箱信息可能会放大[问题PP2](#p2),使得用户在每次使用时增加操作负担

3.待提出
