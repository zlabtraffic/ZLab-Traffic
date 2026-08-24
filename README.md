# Z-Lab Encrypted Traffic Dataset with Plaintext-Ciphertext Mapping

English | [中文](README_zh.md)

This repository publishes datasets for encrypted traffic analysis, website fingerprinting, and model robustness evaluation. The defining feature is **plaintext-ciphertext mapping**: in the controlled-access release, each visit sample includes both the original encrypted traffic capture (`pcap`) and its corresponding TLS key log (`sslkey`). Researchers can therefore align encrypted side-channel features with decrypted application-layer content from the same session.

The repository currently contains two sub-datasets:

- **zlab-website**: Website-level visits focused on homepages. It covers a monitored Top 100 set, a Top 100K background set, and scenarios that vary collection time, region, browser, and proxy protocol. See the [zlab-website README](Website/README.md).
- **zlab-webpage**: Page-level visits to finer-grained targets such as Wikipedia articles, news articles, GitHub repository homepages, and social media profile pages. It supports page-level and content-level identification as well as open-world background evaluation. See the [zlab-webpage README](Webpage/README.md).

## Why plaintext-ciphertext mapping matters

Most encrypted traffic datasets expose only ciphertext-side features such as packet sizes, directions, and timestamps. These features support classification, but they make it difficult to determine which page elements, request structures, or resource-loading behaviors a model uses. By pairing each `pcap` with its `sslkey`, this dataset allows researchers to:

- align encrypted features, including packet size, direction, inter-arrival time, and TLS record size, with HTTP requests, response objects, resource types, and load order;
- extract custom features instead of relying on a fixed feature representation chosen by the dataset publisher;
- study how page structure, browser behavior, proxy encapsulation, and network conditions affect encrypted traffic distributions;
- investigate the interpretability, generalization, and robustness of website fingerprinting models.

## Dataset composition

| Sub-dataset | Primary task | Coverage |
| --- | --- | --- |
| zlab-website | Website-level identification | Top 100 website homepages and Top 100K background website homepages |
| zlab-webpage | Page-level identification | Wikipedia articles, news articles, GitHub repository homepages, and social media profile pages |

Both sub-datasets follow the same release model: raw plaintext-ciphertext pairs support in-depth analysis, while derived features support rapid modeling and benchmark reproduction.

## Release formats

### Controlled-access data

Controlled-access data is distributed as compressed archives. Each archive represents a specific data slice defined by data type, purpose, collection month, scale, collection region, protocol, browser, and page scope.

The recommended internal structure for each slice is:

```text
<dataset_slice>/
├── pcaps/                  # Original encrypted traffic captures
├── sslkeys/                # TLS key logs corresponding to the pcaps
└── README.md               # Collection conditions, labels, and notes for this slice
```

Controlled-access data supports protocol parsing, plaintext-ciphertext alignment, interpretability analysis, and custom feature extraction. Because the release contains `sslkey` files that can decrypt TLS sessions, access must be requested by email.

### Public feature data

The public release contains only feature sequences extracted from the raw traffic. It does not include original `pcap` files, `sslkey` files, or other material that directly enables decryption. This version is intended for reproducing experiments, training models, and comparing algorithms.

Typical fields include:

| Field | Description |
| --- | --- |
| `times` | Packet timestamp sequence |
| `sizes` | Packet size sequence |
| `directions` | Packet direction sequence; `+1` indicates client to server and `-1` indicates server to client |
| `flow_idx` | Flow index within the current trace |
| `labels` | Sample labels |
| `metadata` | Collection conditions, target information, and data-slice information |

## Example: analyzing aligned plaintext and ciphertext

Samples in the controlled-access release can be decrypted with a tool that supports TLS key logs. For example:

```bash
tshark \
  -r sample.pcap \
  -o tls.keylog_file:sample_sslkey.log
```

After decryption, researchers can align and analyze:

- encrypted side-channel features: packet sizes, directions, inter-arrival times, TLS record sizes, and flow durations;
- plaintext application-layer information: request and response objects, resource load order, content types, and protocol fields;
- labels and metadata: target website, page, collection time, collection region, browser, and proxy protocol.

## Access

### Requesting controlled-access data

Request controlled-access data by email. The request should include:

- the applicant's name, affiliation, and contact details;
- the research purpose and the requested data scope;
- the requested sub-datasets and data slices;
- the planned storage, access-control, and deletion procedures;
- whether the applicant plans to publish experimental results based on the data.

Contact: zlab_traffic@gmail.com

### Downloading public feature data

Public feature downloads are listed on the release page.

- Website: https://drive.google.com/file/d/12nDKNlPquAbS-ryqjEuzWTOECLcZqRj8/view?usp=drive_link
