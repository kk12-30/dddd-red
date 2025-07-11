# 最新版本：dddd-redV2.7(Go语言)  
# 最新版本：dddd-rustV2.1(Rust语言)
一款高可拓展的指纹识别、供应链漏洞探测工具。

https://mp.weixin.qq.com/s/39o7yRixuz9bBF1nRBtd0Q

加入纷传圈子获取：https://pc.fenchuan8.com/#/index?forum=99314

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

# 更新V1.3
1、新增Web爬虫功能，对爬虫到的路径URL进行指纹识别并使用poc，增大对组件型nday扫描的全面性

-js：启用无头浏览器模式进行爬取

-js-login：自动禁用无头浏览模式，开启一个浏览器供手动登录。在登录完毕后在命令行界面点击回车键继续爬取。

2、优化POC扫描逻辑，减少重复的POC扫描

3、修改spring常见漏洞为全局POC

4、为了进一步支持高度自定义，将工具的内置字典全部放置config目录中可自由修改

5、添加最新指纹和POC

6、优化主动指纹识别的逻辑

7、使用-nb参数可跳过服务爆破，适合大规模快速打点

# 更新V1.4
1、优化原gopoc爆破模块所有代码，支持未授权检测，新增进度条

# 更新V1.5
1、支持window模式下syn端口快速扫描模式  dddd.exe -t IP -sct syn -mr 3000

2、新增-hw参数，跳过默认POC且自动禁用Golang Poc扫描和服务爆破，适合大规模资产快速打点 dddd.exe -t IP -hw

3、最新POC、指纹更新

# 更新V1.7
1、新增资产信息报告

2、新增100+POC

3、优化gopoc爆破功能
![image](https://github.com/kk12-30/dddd-red/blob/main/1750822165030.jpg)

# V2.0 全新web管理界面
https://mp.weixin.qq.com/s/N2C3ddxV4IcOWMZIjOmR5A

# 更新V2.4
1、参考gogo启发式扫描，快速发现大型网段存活ip  dddd.exe -t 192.168.1.1/16 -sm

2、新增hvvPOC

3、修复已知bug

# 更新V2.5
1、新增指纹

2、修复已知bug

3、优化启发式扫描

# 更新v2.6
增加POC文件索引大幅提升POC匹配速度、新增-det参数（简单探测模式，仅使用/config/allpoc/ALL目录下的POC进行扫描，并关闭workflow的POC扫描，建议使用参数-det -nd）、优化子域名爆破、优化proxy代理

# 更新v2.7
更新POC/指纹、新增资产报告excle导出功能

# 更新dddd-rustV2.0
https://mp.weixin.qq.com/s/39o7yRixuz9bBF1nRBtd0Q

1、新增指纹模糊匹配POC功能

# 更新dddd-rustV2.1
  - 自适应超时学习：根据网络环境自动调整超时参数
  - 随机端口扫描顺序：降低被防火墙或IDS检测的概率
  - 高效并发控制：智能管理并发连接，提高扫描效率
  - 实时扫描速率显示：动态显示每秒扫描端口数

-s指定端口并发数量（默认500）
rust-dddd.exe -t 127.0.0.1 -s 1000
--fingerprint-detection开启主动指纹识别功能（默认关闭）
rust-dddd.exe -t 127.0.0.1 --fingerprint-detection
如果需要按顺序扫描，可以使用--serial 参数
