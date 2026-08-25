# zlab-webpage 数据集

最后更新：2026-08-25 10:18:17

[English](README.md) | 中文

zlab-webpage 是 Z-Lab 加密流量明密映射数据集中的页面级子数据集，主要面向页面指纹识别（Webpage Fingerprinting）、开放世界识别和跨环境鲁棒性评估。公开特征覆盖 Wikipedia、X.com、Bluesky、Mastodon、Threads 和 Tumblr 六个网站。

公开版只包含从流量中提取的派生特征，不包含原始 `pcap`、`sslkey`、Cookie、浏览器 profile、日志或本地路径。原始明密映射数据不在本页面发布。

## 数据集特点

- **页面级目标清晰**：Wikipedia 包含封闭世界词条，五个社交平台包含开放世界账号集合。
- **支持闭集和开放世界识别**：Wikipedia 数据适合闭集页面分类，社交平台数据适合开放世界识别。
- **支持地理位置对照**：Wikipedia 包含 US、France 和 Singapore 采集批次。
- **支持浏览器对照**：Wikipedia 包含 Chrome、Edge 和 Firefox 采集批次。
- **支持快速建模**：公开特征适合模型训练、算法比较和基准复现。

## 数据范围

当前发布目录 `webpage/manifest.csv` 包含以下数据切片。

| 网站名称 | 数据切片 | 数据类型 | 类别数量 | 采集时间 | 地区 | 浏览器 | 协议 | 文件大小 | NPZ名称 |
| --- | --- | --- | ---: | --- | --- | --- | --- | ---: | --- |
| Wikipedia | `wiki_https_20260325_us_chrome` | 封闭世界 | 2,196 | 2026-03-25 | US | Chrome | HTTPS | 0.75 GB | `features_wiki_20260325_us_https_chrome_part(001-009).npz` |
| Wikipedia | `wiki_https_20260325_us_edge` | 封闭世界 | 2,196 | 2026-03-25 | US | Edge | HTTPS | 0.86 GB | `features_wiki_20260325_us_https_edge_part(001-009).npz` |
| Wikipedia | `wiki_https_20260325_us_firefox` | 封闭世界 | 2,196 | 2026-03-25 | US | Firefox | HTTPS | 0.88 GB | `features_wiki_20260325_us_https_firefox_part(001-009).npz` |
| Wikipedia | `wiki_https_20260514_fra_chrome` | 封闭世界 | 2,196 | 2026-05-14 | France | Chrome | HTTPS | 0.52 GB | `features_wiki_20260514_fra_https_chrome_part(001-009).npz` |
| Wikipedia | `wiki_https_20260514_sgp_chrome` | 封闭世界 | 2,196 | 2026-05-14 | Singapore | Chrome | HTTPS | 0.50 GB | `features_wiki_20260514_sgp_https_chrome_part(001-009).npz` |
| Wikipedia | `wiki_https_20260612_us_chrome` | 封闭世界 | 2,196 | 2026-06-12 | US | Chrome | HTTPS | 0.12 GB | `features_wiki_20260612_us_https_chrome_part(001-002).npz` |
| X.com | `social_accounts_x_without_login_20260420_us_chrome` | 开放世界 | 4,000 | 2026-04-20 | US | Chrome | HTTPS | 0.44 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |
| Bluesky | `social_accounts_bsky_without_login_20260420_us_chrome` | 开放世界 | 4,000 | 2026-04-20 | US | Chrome | HTTPS | 0.75 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |
| Mastodon | `social_accounts_mastodon_without_login_20260420_us_chrome` | 开放世界 | 4,000 | 2026-04-20 | US | Chrome | HTTPS | 0.52 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |
| Threads | `social_accounts_threads_without_login_20260420_us_chrome` | 开放世界 | 4,000 | 2026-04-20 | US | Chrome | HTTPS | 0.38 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |
| Tumblr | `social_accounts_tumblr_without_login_20260420_us_chrome` | 开放世界 | 3,995 | 2026-04-20 | US | Chrome | HTTPS | 2.33 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |

## 命名规则

发布压缩包的采集任务名称采用以下格式：

```text
<task>_<protocol>_<capture_time>_<region>_<browser>
```

公开特征文件名采用以下格式：

```text
features_<task>_<capture_time>_<region>_<protocol>_<browser>_part(001-NNN).npz
```

| 字段 | 示例 | 说明 |
| --- | --- | --- |
| `task` | `wiki`、`social_accounts` | 特征构建任务标识；网站目录用于区分社交平台 |
| `capture_time` | `20260325`、`20260420` | 采集日期 |
| `region` | `us`、`fra`、`sgp` | 采集地区或出口区域 |
| `protocol` | `https` | 访问协议或封装协议 |
| `browser` | `chrome`、`edge`、`firefox` | 浏览器环境 |
| `partNNN` | `part(001-009)` | 容量分片范围 |
| `capture_task` | `wiki_https_20260325_us_chrome` | manifest 和压缩包使用的采集切片标识 |

## 发布文件结构

公开特征版本按网站分目录，每个网站目录包含对应采集切片的压缩包：

```text
Google Drive/
├── README.md
├── README_zh.md
├── manifest.csv
├── SHA256SUMS.txt
├── wikipedia/<capture_task>.zip
├── xcom/<capture_task>.zip
├── bluesky/<capture_task>.zip
├── mastodon/<capture_task>.zip
├── threads/<capture_task>.zip
└── tumblr/<capture_task>.zip
```

每个网站压缩包只包含对应批次的派生特征、同名映射 CSV、README 和校验文件。`manifest.csv` 至少记录 `site`、`capture_task`、压缩包文件名、文件大小和 SHA-256；`SHA256SUMS.txt` 用于校验发布目录中的全部文件。

NPZ 文件的典型字段包括：

| 字段 | 含义 |
| --- | --- |
| `times` | 每条样本相对于首包的包时间序列 |
| `sizes` | 包大小序列 |
| `directions` | 包方向序列；`+1` 为客户端到服务器，`-1` 为服务器到客户端，`0` 为未知 |
| `flow_idx` | 当前 trace 内的 flow 索引 |
| `labels` | 样本标签 |
| `metadata` | 页面目标、采集条件和数据切片元数据 |

变长序列使用 NumPy object array 保存，读取时需要：

```python
data = numpy.load(path, allow_pickle=True)
```

### 映射 CSV 字段

映射 CSV 与 NPZ 同名，用于建立特征样本、来源文件、页面目标和采集条件之间的对应关系。

| 字段 | 含义 |
| --- | --- |
| `sample_id` | 当前分片内的样本标识 |
| `pcap_name` | 来源 `pcap` 文件名，不包含本地绝对路径 |
| `capture_task` | 采集切片标识 |
| `page` | 页面 URL 或其他页面目标 |
| `label` | 建模或评估标签 |
| `capture_time` | 采集时间或批次时间 |
| `region` | 采集地区或出口区域 |
| `protocol` | HTTPS、代理协议或封装协议 |
| `browser` | 浏览器环境 |
| `npz_index` | 样本在同名 NPZ 文件中的下标 |

## 推荐实验设置

### 闭集页面指纹识别

使用 Wikipedia 的 US/Chrome 封闭世界批次训练模型，并在同条件或其他 Wikipedia 批次上评估分类性能。

### 开放世界识别

使用 X.com、Bluesky、Mastodon、Threads 和 Tumblr 的开放世界账号集合进行识别与算法比较。

### 时间漂移

使用 Wikipedia 的 2026-03-25 US/Chrome 批次训练模型，在 2026-06-12 US 批次上评估时间泛化能力；France 和 Singapore 批次还可用于同时包含时间与地区变化的对照。

### 空间漂移

使用 Wikipedia 的 2026-03-25 US/Chrome 批次训练模型，在 2026-05-14 France 和 Singapore 批次上评估跨地区泛化能力。由于这些批次的采集日期不同，该对照同时包含地区和时间漂移。

### 浏览器漂移

使用 Wikipedia 的 US/Chrome 批次训练模型，在 US/Edge 和 US/Firefox 批次上评估浏览器差异对流量特征的影响。

## 明密对应分析

公开包不包含 `pcap` 和 `sslkey`，不能直接进行明文与密文对应分析。需要原始明密映射数据时，请按访问方式申请受控数据。

### 访问方式

包含 `pcap` 和 `sslkey` 的原始明密映射数据通过邮件申请获取。公开特征数据按网站放入 Google Drive 压缩包。

申请邮箱：zlab_traffic@gmail.com

公开下载入口：Google Drive 文件夹（链接 TBD）。
