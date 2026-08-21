# zlab-website 数据集

zlab-website 是 Z-Lab 加密流量明密映射数据集中的网站级子数据集，主要面向网站指纹识别（Website Fingerprinting）、开放世界识别和跨环境鲁棒性评估。数据集围绕网站首页访问构建，包含 Top100 监控网站和 Top100K 背景网站，并覆盖时间、地区、浏览器和代理协议变化场景。

本数据集的核心特点是：在受控访问版本中，每条访问样本同时提供原始加密流量 `pcap` 与对应的 TLS key log（`sslkey`），支持在同一条 trace 上建立明文内容与密文流量特征之间的精确对应关系。

## 数据集特点

- **明密映射**：同一访问样本配套提供 `pcap` 和 `sslkey`，可对齐密文侧信道特征与解密后的网页加载内容。
- **网站级识别任务清晰**：Top100 网站作为主要监控集合，Top100K 网站作为大规模背景集合，适合闭集分类和开放世界识别。
- **支持时间漂移评估**：同一 Top100 集合包含多个采集月份，可评估模型随时间变化的性能衰减和跨时间泛化能力。
- **支持空间漂移评估**：包含美国、日本、南非、新加坡、澳大利亚、德国等采集地区，可比较不同网络路径和出口位置带来的分布变化。
- **支持浏览器漂移评估**：包含 Chrome、Edge 和 Firefox 场景，可分析浏览器实现、默认配置和网络栈差异对流量特征的影响。
- **支持代理协议漂移评估**：包含 HTTPS 直连以及 Shadowsocks、Trojan、VLESS XTLS Vision、VMess WebSocket TLS、VMess TLS、VMess 等代理或封装协议场景。
- **同时支持深度分析和快速建模**：受控访问数据适合协议解析、明密对应和可解释性研究；公开特征数据适合模型训练、算法对比和基准复现。

## 数据范围

当前整理表 `website.csv` 中包含以下数据切片。

| 数据切片                                                | 每个网站采集次数 | 用途                             |
| --------------------------------------------------- | -------: | ------------------------------ |
| `web_train_m0_top100_us_https_chrome_home`          |      140 | Top100 训练集，美国、HTTPS、Chrome、首页  |
| `web_background_m0_top100k_us_https_chrome_home`    |        1 | Top100K 背景集，美国、HTTPS、Chrome、首页 |
| `web_test_m1_top100_us_https_chrome_home`           |       30 | 时间漂移测试，月份 m1                   |
| `web_test_m2_top100_us_https_chrome_home`           |       30 | 时间漂移测试，月份 m2                   |
| `web_test_m3_top100_us_https_chrome_home`           |       30 | 时间漂移测试，月份 m3                   |
| `web_test_m4_top100_us_https_chrome_home`           |       30 | 时间漂移测试，月份 m4                   |
| `web_test_m5_top100_us_https_chrome_home`           |       30 | 时间漂移测试，月份 m5                   |
| `web_test_m0_top100_jp_https_chrome_home`           |       30 | 空间漂移测试，日本                      |
| `web_test_m0_top100_za_https_chrome_home`           |       30 | 空间漂移测试，南非                      |
| `web_test_m0_top100_sg_https_chrome_home`           |       30 | 空间漂移测试，新加坡                     |
| `web_test_m0_top100_au_https_chrome_home`           |       30 | 空间漂移测试，澳大利亚                    |
| `web_test_m0_top100_de_https_chrome_home`           |       30 | 空间漂移测试，德国                      |
| `web_train_ma_top100_us_https_chrome_home`          |      140 | 代理协议漂移实验的 HTTPS 基准训练集          |
| `web_test_ma_top100_us_shadowsocks_chrome_home`     |       30 | 代理协议漂移测试，Shadowsocks           |
| `web_test_ma_top100_us_trojan_chrome_home`          |       30 | 代理协议漂移测试，Trojan                |
| `web_test_ma_top100_us_vlessxtlsvision_chrome_home` |       30 | 代理协议漂移测试，VLESS XTLS Vision     |
| `web_test_ma_top100_us_vmesswstls_chrome_home`      |       30 | 代理协议漂移测试，VMess WebSocket TLS   |
| `web_test_ma_top100_us_vmesstls_chrome_home`        |       30 | 代理协议漂移测试，VMess TLS             |
| `web_test_ma_top100_us_vmess_chrome_home`           |       30 | 代理协议漂移测试，VMess                 |
| `web_test_ma_top100_us_https_edge_home`             |       30 | 浏览器漂移测试，Edge                   |
| `web_test_ma_top100_us_https_firefox_home`          |       30 | 浏览器漂移测试，Firefox                |

## 命名规则

数据切片名称采用统一格式：

```text
web_<split>_<month>_<scale>_<region>_<protocol>_<browser>_<scope>
```

字段含义如下：

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| `web` | `web` | 表示 zlab-website 子数据集 |
| `split` | `train`、`test`、`background` | 训练集、测试集或开放世界背景集 |
| `month` | `m0`、`m1`、`m2`、`ma` | 采集月份或实验组标识；`ma` 表示代理协议和浏览器漂移实验组 |
| `scale` | `top100`、`top100k` | 网站集合规模 |
| `region` | `us`、`jp`、`za`、`sg`、`au`、`de` | 采集地区或出口区域 |
| `protocol` | `https`、`shadowsocks`、`trojan`、`vlessxtlsvision`、`vmesswstls`、`vmesstls`、`vmess` | 访问协议、代理协议或封装协议 |
| `browser` | `chrome`、`edge`、`firefox` | 浏览器环境 |
| `scope` | `home` | 页面范围；当前 Website 数据集主要为网站首页 |

## 发布文件结构

受控访问版本建议按数据切片打包。每个切片内部包含：

```text
web_train_m0_top100_us_https_chrome_home/
├── pcaps/
│   └── *.pcap
├── sslkeys/
│   └── *.log
└── README.md
```

其中：

- `pcaps/` 保存原始加密流量；
- `sslkeys/` 保存与 `pcap` 对应的 TLS key log；
- 切片内的 `README.md` 说明该切片的采集条件、标签文件、文件命名和注意事项。

公开特征版本建议采用 `.npz` 文件发布，命名规则与原始切片保持一致：

```text
features_web_<split>_<month>_<scale>_<region>_<protocol>_<browser>_<scope>.npz
```

典型字段包括：

| 字段 | 含义 |
| --- | --- |
| `times` | 包时间序列 |
| `sizes` | 包大小序列 |
| `directions` | 包方向序列，`+1` 表示客户端到服务器，`-1` 表示服务器到客户端 |
| `flow_idx` | 当前 trace 内的 flow 索引 |
| `labels` | 网站标签 |
| `metadata` | 数据切片、采集条件和目标网站信息 |

## 推荐实验设置

### 闭集网站指纹识别

使用 `web_train_m0_top100_us_https_chrome_home` 训练模型，并在同条件或其他 Top100 测试切片上评估分类准确率。

### 开放世界识别

使用 Top100 作为监控网站集合，使用 `web_background_m0_top100k_us_https_chrome_home` 作为非监控背景集合，评估开放世界条件下的真阳性率、假阳性率、精确率和召回率。

### 时间漂移

使用 `m0` 训练模型，在 `m1` 到 `m7` 的 Top100 测试切片上评估性能变化，分析网站内容变化、CDN 变化和前端资源变化带来的模型衰减。

### 空间漂移

使用美国采集的 HTTPS Chrome 数据训练模型，在日本、南非、新加坡、澳大利亚和德国采集的测试切片上评估跨地区泛化能力。

### 浏览器漂移

使用 Chrome 数据训练模型，在 Edge 和 Firefox 测试切片上评估浏览器差异对流量特征的影响。

### 代理协议漂移

使用 HTTPS 直连数据训练模型，在 Shadowsocks、Trojan、VLESS XTLS Vision 和 VMess 系列协议测试切片上评估协议封装、流量混淆和代理转发带来的分布变化。

## 明密对应分析

受控访问版本中的每条样本都应能够通过 `pcap` 与 `sslkey` 建立对应关系。研究者可以使用 `tshark` 或 Wireshark 解密：

```bash
tshark \
  -r sample.pcap \
  -o tls.keylog_file:sample_sslkey.log
```

解密后可对齐分析：

- 加密流量侧：包长、方向、时间间隔、TLS record 大小、flow 数量和持续时间；
- 明文内容侧：网页主文档、脚本、样式、图片、字体、接口请求和第三方资源；
- 元数据侧：网站标签、采集月份、地区、浏览器、协议和数据划分。

