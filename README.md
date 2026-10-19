# Beyond BERT: Knowledge Graph-Enhanced Resource-Aware EHR Semantic Processing

This repository implements a three-phase, knowledge graph-enhanced framework designed for scalable, relation-oriented semantic processing of Electronic Health Records (EHRs) across distributed environments. 

By complementing contextual language embeddings with explicit entity-relation structures and balanced CPU/GPU task scheduling, the framework significantly reduces inference latency and GPU overhead while boosting extraction precision and query throughput.

---

## 📌 Key Highlights

- **Hybrid Representation:** Bridges the gap between contextual transformer encodings and explicit clinical networks (patients, symptoms, diagnoses, medications, and labs).
- **Query-Oriented Subgraphs:** Dynamically generates patient-specific local subgraphs, avoiding redundant traversals of global biomedical graphs.
- **Resource-Aware Scheduling:** Offloads graph construction and structural traversal to a 15-server distributed CPU layer, reserving GPU compute for core neural inferences.
- **Strict Baseline Benchmarking:** Direct inference-time comparison against an unmodified BERT baseline across identical query volumes, concurrency levels, and hardware.

---

## 🏗️ System Architecture

1. **Phase 1: Preprocessing & Normalization**  
   Clinical text parsing, document segmentation, entity tagging, query simplification, and request token generation across 10,000 synthetic patient records (`Phase1_Synthetic_EHR_Dataset_10000.csv`).
2. **Phase 2: Semantic Graph Construction**  
   Extraction of clinical concepts and relationship triples to construct both persistent global ontologies and query-customized local subgraphs.
3. **Phase 3: Distributed Execution & Evaluation**  
   Workload distribution across 15 backend servers and 4× NVIDIA A100 GPUs, measuring semantic quality, latency, queue behavior, and hardware footprint under concurrent loads (10 to 1,000 requests).

---

## 📊 Benchmark Summary

### 1. Semantic Extraction Performance
| Metric | BERT Baseline | Proposed Framework | Δ Improvement |
| :--- | :---: | :---: | :---: |
| **Entity Precision** | 0.860 | **0.940** | +9.3% |
| **Entity Recall** | 0.810 | **0.910** | +12.3% |
| **Entity F1-Score** | 0.835 | **0.925** | +10.8% |
| **Relation Precision** | 0.820 | **0.920** | +12.2% |
| **Relation Recall** | 0.770 | **0.890** | +15.6% |
| **Relation F1-Score** | 0.795 | **0.905** | +13.8% |

### 2. Runtime & Resource Utilization (at Scale)
- **Latency:** ~35.70% reduction in average latency; ~34.27% reduction in P95 latency (117 ms vs. 178 ms at 1,000 concurrent requests).
- **Peak Throughput:** Increases from 468 req/s (BERT) to **588 req/s** (+25.6%).
- **Queue Stabilization:** Peak queue accumulation drops from 66 to **44 requests** at max concurrency.
- **Hardware Footprint:** ~31.11% lower A100 GPU utilization, 27.52% lower GPU VRAM consumption (10.75 GB vs. 14.88 GB), achieved via intelligent server-layer CPU offloading.

---

## 🔗 Resources

- **Colab Notebook:** [Run on Google Colab](https://colab.research.google.com/drive/1HpfZMLAWnV_kBKTzYL6TwOpMfs-H53w4)
- **Dataset:** `Phase1_Synthetic_EHR_Dataset_10000.csv` (10k patient profiles with longitudinal text narratives, structured diagnostic attributes, and tokenized queries).
