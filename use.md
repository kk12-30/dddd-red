# DDDD 工具使用手册

## 1. 工具介绍

DDDD 是一款强大的批量信息收集和供应链漏洞探测工具，专为优化红队工作流设计，能够减少繁琐、枯燥的机械性操作。该工具集成了多种扫描模块，支持从目标发现到漏洞扫描的完整工作流程，提供了丰富的报表功能。

### 1.1 主要特点

* **智能输入识别**：自动识别输入类型，无需手动分类
* **高效指纹识别**：支持主动/被动指纹识别，及复杂逻辑运算
* **漏洞扫描**：集成 Nuclei v3，提供大量 POC
* **指纹漏洞映射**：通过指纹精准匹配漏洞，减少无效请求
* **子域名收集**：高效的子域名枚举/爆破，精准过滤泛解析
* **搜索引擎集成**：支持 Hunter、Fofa、Quake 等搜索引擎
* **低感知模式**：提供 Hunter 低感知扫描功能
* **跨平台支持**：低依赖，多系统开箱即用
* **报表功能**：生成高效的 HTML 报表，包含详细请求和响应
* **审计日志**：记录完整扫描行为，适合敏感环境使用

## 2. 快速开始

### 2.1 安装

从 Release 页面下载与您操作系统对应的二进制文件和 config.zip 配置文件包。解压后直接在命令行中运行即可。

> 注意：从 DDDD v2.2 开始，工具可以独立于 config 文件夹运行。

### 2.2 基本使用

#### 扫描单个 IP

```bash
dddd -t 192.168.0.1
```

#### 扫描网段

```bash
dddd -t 192.168.0.1/24
dddd -t 192.168.0.0-192.168.0.12
```

#### 扫描网站

```bash
dddd -t http://example.com
```

#### Web 模式启动

```bash
dddd -web
```

通过 Web 界面可以进行扫描配置和查看结果，默认访问地址为：http://localhost:7977

## 3. 工作流程

DDDD 工具的扫描过程遵循以下工作流程：

1. **目标输入处理**：支持多种输入格式（IP、域名、URL、CIDR等）
2. **资产发现**：子域名枚举、网络空间搜索引擎查询
3. **主机发现**：ICMP、TCP 探测存活主机
4. **端口扫描**：支持 TCP、SYN 方式扫描端口
5. **服务识别**：识别开放端口上运行的服务
6. **Web 服务探测**：探测 Web 服务并获取响应
7. **指纹识别**：识别服务和应用的指纹特征
8. **漏洞扫描**：基于指纹匹配 POC 进行漏洞检测
9. **服务爆破**：对常见服务进行弱口令检测
10. **报告生成**：生成资产报告和漏洞报告

## 4. 命令参数

### 4.1 目标设置

| 参数 | 说明 |
|------|------|
| `-t, -target <string>` | 扫描目标，支持：IP、CIDR、域名、URL，或指定文件 |

### 4.2 端口扫描设置

| 参数 | 说明 |
|------|------|
| `-p, -port <string>` | 指定扫描端口，默认扫描 Top1000 |
| `-np, -no-port <string>` | 指定排除的端口 |
| `-sct, -scan-type <string>` | 端口扫描方式：tcp 或 syn（默认 tcp） |
| `-tst, -tcp-scan-threads <int>` | TCP 扫描线程数（默认 1000） |
| `-sst, -syn-scan-threads <int>` | SYN 扫描线程数（默认 10000） |
| `-mr, -masscan-rate <int>` | Masscan 扫描速率（默认 3000） |
| `-mp, -masscan-path <string>` | 指定 masscan 程序路径 |
| `-pmc, -ports-max-count <int>` | IP 端口数量阈值，超过此值将被丢弃（默认 300） |

### 4.3 主机发现设置

| 参数 | 说明 |
|------|------|
| `-Pn` | 禁用主机发现功能 |
| `-nip, -no-icmp-ping` | 禁用 ICMP 主机发现 |
| `-tp, -tcp-ping` | 启用 TCP 主机发现 |

### 4.4 子域名设置

| 参数 | 说明 |
|------|------|
| `-sd, -subdomain` | 启用子域名收集 |
| `-nsb, -no-subdomain-bruteforce` | 关闭子域名爆破 |
| `-sbw, -subdomain-wordlist <string>` | 指定子域名爆破字典 |
| `-sbt, -subdomain-bruteforce-threads <int>` | 子域名爆破线程数（默认 25） |

### 4.5 Web 设置

| 参数 | 说明 |
|------|------|
| `-wt, -web-threads <int>` | Web 请求线程数（默认 50） |
| `-wto, -web-timeout <int>` | Web 请求超时时间（秒，默认 10） |
| `-nds, -no-dir-search` | 禁用目录爆破 |
| `-ejc, -enable-js-crawler` | 启用 JS 爬虫 |
| `-ejlc, -enable-js-login-crawler` | 启用 JS 登录爬虫 |
| `-acda, -allow-cdn-assets` | 允许扫描 CDN 资产 |

### 4.6 网络空间搜索引擎

| 参数 | 说明 |
|------|------|
| `-hunter` | 从 Hunter 获取资产 |
| `-hps, -hunter-page-size <int>` | Hunter 查询每页条数（默认 100） |
| `-hmpc, -hunter-max-page-count <int>` | Hunter 最大查询页数（默认 10） |
| `-lpm, -low-perception-mode` | Hunter 低感知模式 |
| `-fofa` | 从 Fofa 获取资产 |
| `-fmc, -fofa-max-count <int>` | Fofa 查询资产条数（默认 100） |
| `-quake` | 从 Quake 获取资产 |
| `-qmc, -quake-max-count <int>` | Quake 查询资产条数（默认 100） |

### 4.7 输出设置

| 参数 | 说明 |
|------|------|
| `-o, -output <string>` | 结果输出文件（默认 result.txt） |
| `-ot, -output-type <string>` | 结果输出格式：text 或 json（默认 text） |
| `-ho, -html-output <string>` | HTML 漏洞报告文件名 |
| `-ar, -asset-report` | 生成资产报告（默认开启） |
| `-web` | 启用 Web 服务器模式 |
| `-web-port <int>` | Web 服务器端口（默认 7977） |

### 4.8 漏洞探测设置

| 参数 | 说明 |
|------|------|
| `-npoc` | 关闭漏洞探测，只进行信息收集 |
| `-poc, -poc-name <string>` | 模糊匹配 POC 名称 |
| `-gpt, -golang-poc-threads <int>` | GoPoc 运行线程数（默认 50） |
| `-ngp, -no-golang-poc` | 关闭 Golang POC 探测 |
| `-dgp, -disable-general-poc` | 禁用无视指纹的漏洞映射 |
| `-et, -exclude-tags <string>` | 排除指定 tags 的模板 |
| `-s, -severity <string>` | 指定漏洞严重程度：info,low,medium,high,critical |
| `-nb, -no-brute` | 禁用服务爆破 |
| `-hw` | 护网模式 |

### 4.9 反连设置

| 参数 | 说明 |
|------|------|
| `-ni, -no-interactsh` | 禁用 Interactsh 服务器 |
| `-iserver, -interactsh-server <string>` | 指定 Interactsh 服务器 |
| `-itoken, -interactsh-token <string>` | Interactsh Token |

### 4.10 代理设置

| 参数 | 说明 |
|------|------|
| `-hp, -http-proxy <string>` | 设置 HTTP 代理 |
| `-hpt, -http-proxy-test` | 测试代理连接 |
| `-hptu, -http-proxy-test-url <string>` | 代理测试 URL（默认 http://www.baidu.com） |

### 4.11 智能模式

| 参数 | 说明 |
|------|------|
| `-sm, -smart-mode` | 启用智能扫描模式 |
| `-sh, -smart-hosts <string>` | 智能扫描主机配置 |
| `-sp, -smart-ports <string>` | 智能扫描端口配置 |

### 4.12 其他设置

| 参数 | 说明 |
|------|------|
| `-dm, -detection-mode` | 使用简单探测模式 |
| `-v, -verbose` | 启用详细输出模式 |
| `-vv, -debug` | 启用调试输出模式 |
| `-a, -audit` | 启用审计日志功能 |

## 5. 使用场景示例

### 5.1 红队外网资产收集

#### 从 Hunter 获取公司资产

```bash
dddd -t 'icp.name="目标公司"' -hunter -oip
```

#### 同时使用 Hunter 和 Fofa

```bash
dddd -t 'icp.name="目标公司"' -hunter -fofa -oip
```

#### 子域名枚举

```bash
dddd -t example.com -sd
```

### 5.2 内网扫描

#### 标准内网扫描

```bash
dddd -t 192.168.1.0/24
```

#### 内网快速扫描

```bash
dddd -t 192.168.1.0/24 -p 22,80,443,3389,8080
```

#### 敏感环境扫描（带审计日志）

```bash
dddd -t 192.168.1.0/24 -a
```

### 5.3 指定漏洞扫描

#### 指定 POC 扫描

```bash
dddd -t example.com -poc spring
```

#### 仅进行指纹识别

```bash
dddd -t example.com -npoc
```

#### 指定严重等级扫描

```bash
dddd -t example.com -s high,critical
```

### 5.4 Web 模式

#### 启动 Web 服务

```bash
dddd -web
```

默认访问地址：http://localhost:7977

## 6. 输出文件

DDDD 工具提供以下几种输出：

1. **文本输出**：默认保存到 `result.txt`，可通过 `-o` 参数更改
2. **HTML 报告**：漏洞报告默认使用时间戳命名，可通过 `-ho` 参数更改
3. **资产报告**：收集的资产信息以 HTML 格式呈现，包含主机、服务、Web 应用等信息
4. **审计日志**：使用 `-a` 参数开启，记录到 `audit.log`

## 7. 注意事项

1. **SYN 扫描**：需要 root/管理员权限，Windows 下需要指定 masscan 路径
2. **扫描速率**：调整线程参数可控制扫描速度和资源占用
3. **代理设置**：使用 `-hp` 参数设置代理，支持 HTTP 和 SOCKS5 代理
4. **内存占用**：大规模扫描时注意控制内存使用
5. **防火墙干扰**：某些防火墙可能会干扰扫描结果
6. **合规使用**：请确保在授权范围内使用该工具

## 8. 进阶功能

### 8.1 智能扫描模式

智能模式针对大型网络进行优化，能够智能选择扫描策略：

```bash
# 基础智能模式
dddd -t 192.168.0.0/16 -sm

# 指定智能主机范围
dddd -t 192.168.0.0/16 -sm -sh 1,254

# 指定智能端口范围
dddd -t 192.168.0.0/16 -sm -sp 80,443,8080
```

### 8.2 护网模式

护网模式自动调整扫描策略，减少特征明显的扫描操作：

```bash
dddd -t example.com -hw
```

### 8.3 指纹定制

可以在 `config/finger.yaml` 中添加或修改指纹规则，支持的匹配类型包括：

- 标题匹配
- 头部匹配
- 正文匹配
- 图标哈希匹配
- 多条件逻辑组合

## 9. 常见问题

1. **问题**：SYN 扫描失败  
   **解决**：检查是否以管理员/root权限运行，确认 masscan 安装路径正确

2. **问题**：扫描速度过慢  
   **解决**：调整线程参数，如 `-tst`、`-wt` 等；对于大型网络考虑使用智能模式 `-sm`

3. **问题**：出现大量误报  
   **解决**：调整 POC 严重程度筛选 `-s`，或使用 `-et` 排除特定类型的 POC

4. **问题**：内存占用过高  
   **解决**：减少并发线程，对大型网络进行分批扫描

5. **问题**：扫描被防火墙/WAF拦截  
   **解决**：使用低感知模式 `-lpm`，减少扫描线程，延长请求间隔

## 10. 免责声明

本工具仅面向**合法授权**的企业安全建设行为，如您需要测试本工具的可用性，请自行搭建靶机环境。

在使用本工具进行检测时，您应确保该行为符合当地的法律法规，并且已经取得了足够的授权。**请勿对非授权目标进行扫描。**

如您在使用本工具的过程中存在任何非法行为，您需自行承担相应后果，我们将不承担任何法律及连带责任。 