# zlab-webpage Dataset

English | [中文](README_zh.md)

zlab-webpage is the page-level sub-dataset of the Z-Lab Encrypted Traffic Dataset with Plaintext-Ciphertext Mapping. It is intended for Webpage Fingerprinting, open-world identification, and robustness evaluation across collection environments. The public features cover six websites: Wikipedia, X.com, Bluesky, Mastodon, Threads, and Tumblr.

The public release contains only derived features extracted from traffic. It does not contain raw `pcap`, `sslkey`, cookies, browser profiles, logs, or local paths. The original plaintext-ciphertext mapping data is not released on this page.

## Features

- **Clear page-level targets**: Wikipedia provides a closed-world collection of articles, while the five social platforms provide open-world account collections.
- **Closed-world and open-world identification**: Wikipedia data supports closed-world page classification, while the social-platform data supports open-world identification.
- **Geographic comparisons**: Wikipedia includes collection batches from the United States, France, and Singapore.
- **Browser comparisons**: Wikipedia includes Chrome, Edge, and Firefox collection batches.
- **Rapid modeling**: The public features support model training, algorithm comparison, and benchmark reproduction.

## Data coverage

The current `webpage/manifest.csv` lists the following data slices.

| Website | Data slice | Data type | Classes | Capture date | Region | Browser | Protocol | File size | NPZ name |
| --- | --- | --- | ---: | --- | --- | --- | --- | ---: | --- |
| Wikipedia | `wiki_https_20260325_us_chrome` | Closed-world | 2,196 | 2026-03-25 | US | Chrome | HTTPS | 0.75 GB | `features_wiki_20260325_us_https_chrome_part(001-009).npz` |
| Wikipedia | `wiki_https_20260325_us_edge` | Closed-world | 2,196 | 2026-03-25 | US | Edge | HTTPS | 0.86 GB | `features_wiki_20260325_us_https_edge_part(001-009).npz` |
| Wikipedia | `wiki_https_20260325_us_firefox` | Closed-world | 2,196 | 2026-03-25 | US | Firefox | HTTPS | 0.88 GB | `features_wiki_20260325_us_https_firefox_part(001-009).npz` |
| Wikipedia | `wiki_https_20260514_fra_chrome` | Closed-world | 2,196 | 2026-05-14 | France | Chrome | HTTPS | 0.52 GB | `features_wiki_20260514_fra_https_chrome_part(001-009).npz` |
| Wikipedia | `wiki_https_20260514_sgp_chrome` | Closed-world | 2,196 | 2026-05-14 | Singapore | Chrome | HTTPS | 0.50 GB | `features_wiki_20260514_sgp_https_chrome_part(001-009).npz` |
| Wikipedia | `wiki_https_20260612_us_chrome` | Closed-world | 2,196 | 2026-06-12 | US | Chrome | HTTPS | 0.12 GB | `features_wiki_20260612_us_https_chrome_part(001-002).npz` |
| X.com | `social_accounts_x_without_login_20260420_us_chrome` | Open-world | 4,000 | 2026-04-20 | US | Chrome | HTTPS | 0.44 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |
| Bluesky | `social_accounts_bsky_without_login_20260420_us_chrome` | Open-world | 4,000 | 2026-04-20 | US | Chrome | HTTPS | 0.75 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |
| Mastodon | `social_accounts_mastodon_without_login_20260420_us_chrome` | Open-world | 4,000 | 2026-04-20 | US | Chrome | HTTPS | 0.52 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |
| Threads | `social_accounts_threads_without_login_20260420_us_chrome` | Open-world | 4,000 | 2026-04-20 | US | Chrome | HTTPS | 0.38 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |
| Tumblr | `social_accounts_tumblr_without_login_20260420_us_chrome` | Open-world | 3,995 | 2026-04-20 | US | Chrome | HTTPS | 2.33 GB | `features_social_accounts_20260420_us_https_chrome_part(001-002).npz` |

## Naming convention

Release archive capture-task names follow this format:

```text
<task>_<protocol>_<capture_time>_<region>_<browser>
```

Public feature filenames follow this format:

```text
features_<task>_<capture_time>_<region>_<protocol>_<browser>_part(001-NNN).npz
```

| Field | Examples | Description |
| --- | --- | --- |
| `task` | `wiki`, `social_accounts` | Feature-building task identifier; the website directory distinguishes social platforms |
| `capture_time` | `20260325`, `20260420` | Capture date |
| `region` | `us`, `fra`, `sgp` | Collection or egress region |
| `protocol` | `https` | Access or encapsulation protocol |
| `browser` | `chrome`, `edge`, `firefox` | Browser environment |
| `partNNN` | `part(001-009)` | Capacity-split range |
| `capture_task` | `wiki_https_20260325_us_chrome` | Capture-slice identifier used by the manifest and archive |

## Release file structure

The public feature release is organized by website. Each website directory contains the archives for its corresponding capture slices:

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

Each website archive contains only the derived features for the corresponding batch, same-name mapping CSV files, README files, and checksum files. `manifest.csv` records at least `site`, `capture_task`, archive filename, file size, and SHA-256. `SHA256SUMS.txt` verifies all files in the release directory.

Typical NPZ fields include:

| Field | Description |
| --- | --- |
| `times` | Packet timestamp sequence relative to the first packet in each sample |
| `sizes` | Packet-size sequence |
| `directions` | Packet-direction sequence; `+1` is client to server, `-1` is server to client, and `0` is unknown |
| `flow_idx` | Flow index within the current trace |
| `labels` | Sample labels |
| `metadata` | Page target, collection conditions, and data-slice metadata |

Variable-length sequences are stored as NumPy object arrays. Load them with:

```python
data = numpy.load(path, allow_pickle=True)
```

### Mapping CSV fields

Each mapping CSV has the same base name as its NPZ file and links feature samples to source filenames, page targets, and collection conditions.

| Field | Description |
| --- | --- |
| `sample_id` | Sample identifier within the current split |
| `pcap_name` | Source `pcap` filename without a local absolute path |
| `capture_task` | Capture-slice identifier |
| `page` | Page URL or other page target |
| `label` | Modeling or evaluation label |
| `capture_time` | Capture date or batch date |
| `region` | Collection or egress region |
| `protocol` | HTTPS, proxy, or encapsulation protocol |
| `browser` | Browser environment |
| `npz_index` | Sample index in the corresponding NPZ file |

## Recommended experiments

### Closed-world webpage fingerprinting

Train a model on the Wikipedia US/Chrome closed-world batch and evaluate classification performance on the same condition or other Wikipedia batches.

### Open-world identification

Use the open-world account collections from X.com, Bluesky, Mastodon, Threads, and Tumblr for identification and algorithm comparison.

### Temporal drift

Train on the Wikipedia 2026-03-25 US/Chrome batch and evaluate temporal generalization on the 2026-06-12 US batch. The France and Singapore batches additionally support comparisons where time and geography vary together.

### Geographic drift

Train on the Wikipedia 2026-03-25 US/Chrome batch and evaluate cross-region generalization on the 2026-05-14 France and Singapore batches. Because these batches were collected on a different date, this comparison includes both geographic and temporal shift.

### Browser drift

Train on the Wikipedia US/Chrome batch and evaluate the effect of browser differences on traffic features using the US/Edge and US/Firefox batches.

## Plaintext-ciphertext alignment

The public package does not contain `pcap` or `sslkey`, so it cannot be used directly for plaintext-ciphertext alignment analysis. Request the original plaintext-ciphertext mapping data through the access procedure when needed.

### Access

The original plaintext-ciphertext mapping data containing `pcap` and `sslkey` is available by email request. Public feature data is distributed as website-specific archives in Google Drive.

Contact: zlab_traffic@gmail.com

Public download: TBD
