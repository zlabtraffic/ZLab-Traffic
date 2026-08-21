# zlab-website Dataset

English | [中文](README_zh.md)

zlab-website is the website-level sub-dataset of the Z-Lab Encrypted Traffic Dataset with Plaintext-Ciphertext Mapping. It supports website fingerprinting, open-world identification, and robustness evaluation across collection environments. The dataset focuses on website homepage visits and includes a monitored Top 100 set, a Top 100K background set, and scenarios that vary collection time, region, browser, and proxy protocol.

In the controlled-access release, each visit sample includes both the original encrypted traffic capture (`pcap`) and its corresponding TLS key log (`sslkey`). This pairing enables precise alignment between decrypted page-loading content and encrypted traffic features from the same trace.

## Features

- **Plaintext-ciphertext mapping**: Each visit sample pairs a `pcap` with its `sslkey`, allowing encrypted side-channel features to be aligned with decrypted page-loading content.
- **Well-defined website-level tasks**: The Top 100 websites form the primary monitored set, while the Top 100K websites form a large background set suitable for closed-world classification and open-world identification.
- **Temporal drift evaluation**: The same Top 100 set is collected in multiple months, supporting measurement of performance degradation and cross-time generalization.
- **Geographic drift evaluation**: Collection regions include the United States, Japan, South Africa, Singapore, Australia, and Germany, allowing comparisons across network paths and egress locations.
- **Browser drift evaluation**: Chrome, Edge, and Firefox data supports analysis of traffic differences caused by browser implementations, default settings, and network stacks.
- **Proxy protocol drift evaluation**: The dataset includes direct HTTPS and scenarios using Shadowsocks, Trojan, VLESS XTLS Vision, VMess WebSocket TLS, VMess TLS, and VMess.
- **Support for both in-depth analysis and rapid modeling**: Controlled-access data supports protocol parsing, plaintext-ciphertext alignment, and interpretability research. Public feature data supports model training, algorithm comparison, and benchmark reproduction.

## Data coverage

The current `website.csv` index contains the following data slices.

| Data slice | Samples per website | Purpose |
| --- | ---: | --- |
| `web_train_m0_top100_us_https_chrome_home` | 140 | Top 100 training set; United States, HTTPS, Chrome, homepage |
| `web_background_m0_top100k_us_https_chrome_home` | 1 | Top 100K background set; United States, HTTPS, Chrome, homepage |
| `web_test_m1_top100_us_https_chrome_home` | 30 | Temporal drift test, month m1 |
| `web_test_m2_top100_us_https_chrome_home` | 30 | Temporal drift test, month m2 |
| `web_test_m3_top100_us_https_chrome_home` | 30 | Temporal drift test, month m3 |
| `web_test_m4_top100_us_https_chrome_home` | 30 | Temporal drift test, month m4 |
| `web_test_m5_top100_us_https_chrome_home` | 30 | Temporal drift test, month m5 |
| `web_test_m0_top100_jp_https_chrome_home` | 30 | Geographic drift test, Japan |
| `web_test_m0_top100_za_https_chrome_home` | 30 | Geographic drift test, South Africa |
| `web_test_m0_top100_sg_https_chrome_home` | 30 | Geographic drift test, Singapore |
| `web_test_m0_top100_au_https_chrome_home` | 30 | Geographic drift test, Australia |
| `web_test_m0_top100_de_https_chrome_home` | 30 | Geographic drift test, Germany |
| `web_train_ma_top100_us_https_chrome_home` | 140 | HTTPS baseline training set for proxy protocol drift experiments |
| `web_test_ma_top100_us_shadowsocks_chrome_home` | 30 | Proxy protocol drift test, Shadowsocks |
| `web_test_ma_top100_us_trojan_chrome_home` | 30 | Proxy protocol drift test, Trojan |
| `web_test_ma_top100_us_vlessxtlsvision_chrome_home` | 30 | Proxy protocol drift test, VLESS XTLS Vision |
| `web_test_ma_top100_us_vmesswstls_chrome_home` | 30 | Proxy protocol drift test, VMess WebSocket TLS |
| `web_test_ma_top100_us_vmesstls_chrome_home` | 30 | Proxy protocol drift test, VMess TLS |
| `web_test_ma_top100_us_vmess_chrome_home` | 30 | Proxy protocol drift test, VMess |
| `web_test_ma_top100_us_https_edge_home` | 30 | Browser drift test, Edge |
| `web_test_ma_top100_us_https_firefox_home` | 30 | Browser drift test, Firefox |

## Naming convention

Data slice names follow this format:

```text
web_<split>_<month>_<scale>_<region>_<protocol>_<browser>_<scope>
```

| Field | Examples | Description |
| --- | --- | --- |
| `web` | `web` | Identifies the zlab-website sub-dataset |
| `split` | `train`, `test`, `background` | Training, test, or open-world background split |
| `month` | `m0`, `m1`, `m2`, `ma` | Collection month or experiment-group identifier; `ma` denotes the proxy protocol and browser drift group |
| `scale` | `top100`, `top100k` | Size of the website set |
| `region` | `us`, `jp`, `za`, `sg`, `au`, `de` | Collection region or egress region |
| `protocol` | `https`, `shadowsocks`, `trojan`, `vlessxtlsvision`, `vmesswstls`, `vmesstls`, `vmess` | Access, proxy, or encapsulation protocol |
| `browser` | `chrome`, `edge`, `firefox` | Browser environment |
| `scope` | `home` | Page scope; the current Website dataset primarily contains homepages |

## Release file structure

The controlled-access release is organized by data slice. Each slice contains:

```text
web_train_m0_top100_us_https_chrome_home/
├── pcaps/
│   └── *.pcap
├── sslkeys/
│   └── *.log
└── README.md
```

- `pcaps/` stores the original encrypted traffic captures.
- `sslkeys/` stores the TLS key logs corresponding to the `pcap` files.
- The slice-level `README.md` documents collection conditions, label files, file naming, and usage notes.

The public feature release uses `.npz` files whose names follow the same convention as the raw slices:

```text
features_web_<split>_<month>_<scale>_<region>_<protocol>_<browser>_<scope>.npz
```

Typical fields include:

| Field | Description |
| --- | --- |
| `times` | Packet timestamp sequence |
| `sizes` | Packet size sequence |
| `directions` | Packet direction sequence; `+1` indicates client to server and `-1` indicates server to client |
| `flow_idx` | Flow index within the current trace |
| `labels` | Website labels |
| `metadata` | Data slice, collection conditions, and target website information |

## Recommended experiments

### Closed-world website fingerprinting

Train a model on `web_train_m0_top100_us_https_chrome_home`, then evaluate classification accuracy on a Top 100 test slice collected under the same or different conditions.

### Open-world identification

Use the Top 100 set as monitored websites and `web_background_m0_top100k_us_https_chrome_home` as the unmonitored background set. Evaluate true-positive rate, false-positive rate, precision, and recall under open-world conditions.

### Temporal drift

Train on the `m0` data and evaluate on the Top 100 test slices from `m1` through `m7`. This experiment measures performance changes caused by updates to website content, CDNs, and front-end resources.

### Geographic drift

Train on the HTTPS Chrome data collected in the United States, then evaluate cross-region generalization on test slices collected in Japan, South Africa, Singapore, Australia, and Germany.

### Browser drift

Train on Chrome data and evaluate on the Edge and Firefox test slices to measure how browser differences affect traffic features.

### Proxy protocol drift

Train on direct HTTPS traffic, then evaluate on test slices collected through Shadowsocks, Trojan, VLESS XTLS Vision, and the VMess protocol variants. This experiment measures distribution shifts caused by protocol encapsulation, traffic obfuscation, and proxy forwarding.

## Plaintext-ciphertext alignment

Each sample in the controlled-access release pairs a `pcap` with an `sslkey`. Researchers can decrypt a sample with `tshark` or Wireshark:

```bash
tshark \
  -r sample.pcap \
  -o tls.keylog_file:sample_sslkey.log
```

After decryption, researchers can align and analyze:

- encrypted traffic: packet sizes, directions, inter-arrival times, TLS record sizes, flow counts, and flow durations;
- plaintext content: the main document, scripts, stylesheets, images, fonts, API requests, and third-party resources;
- metadata: website labels, collection month, region, browser, protocol, and data split.
