# dddd-red(原dddd2)
一款高可拓展的指纹识别、供应链漏洞探测工具。
原项目：https://github.com/SleepingBag945/dddd

加入纷传圈子获取：https://pc.fenchuan8.com/#/index?forum=99314

You can purchase the linked item to get it：https://afdian.com/item/85f8e5443ad011f0b2475254001e7c00

# 逻辑重构V1.1
原工具添加POC的逻辑需要从workflow工作流添加指纹和POC名称十分繁琐故重新设计了POC扫描逻辑，便于随时添加POC和指纹（The logic of adding a POC in the original tool requires adding a fingerprint and POC name from the workflow workflow, which is very cumbersome, so the POC scanning logic has been redesigned to facilitate the addition of POC and fingerprint at any time）
- 配置文件在本目录下的config文件夹，工具会优先从该文件夹下的配置文件中读取，若不存在配置文件会使用自身原有的配置
- config目录下的allpoc文件夹为新增的外带POC配置，默认会从该文件夹中通过相似度去匹配指纹对应的POC后合并在原有的POC列表（若不使用外带POC可将该文件夹重新命名）
- allpoc文件夹下可设置以指纹名称为名的子文件，匹配到该指纹会在该文件夹中获取POC；在”ALL“子文件中放置POC所有请求都会测试该文件夹下的POC
- 新增参数-app（指定外带POC文件夹的路径，默认使用同目录下的config文件夹配置）和-st（指纹与POC名称相似度阈值(0-1) 默认0.7)
- POC错误检测，对存在错误的yaml会提示并跳过
- 添加最新POC和指纹
- 新增黑名单指纹配置（blackfinger.yaml）
![image](https://github.com/kk12-30/dddd-red/blob/main/1.png)
![image](https://github.com/kk12-30/dddd-red/blob/main/2.png)
![image](https://github.com/kk12-30/dddd-red/blob/main/4.png)

# 更新V1.2
添加最新指纹和POC（2025.5）

