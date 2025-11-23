# AOJ-CodeRank-Benchmark: Dual-Structure Efficiency Ranking Dataset

This repository hosts a highly curated dataset derived from the **Aizu Online Judge (AOJ)** platform, designed specifically for benchmarking Large Language Models (LLMs) on code efficiency ranking tasks. The data is structured into two versions to support both robust evaluation and data validation needs.

## 1. 🥇 Primary Benchmark File (Evaluation Ready)

**File Name: `AOJ-CodeRank-Benchmark.jsonl`**

This is the final, flattened, and ready-to-use dataset structure optimized for direct use in LLM fine-tuning and evaluation.

### Structure (JSONL v2 - Flattened)

Each line is a single task group containing up to 20 candidates.

| Field Name | Type | Description |
| :--- | :--- | :--- |
| `group_id` | string | Unique ID for the task (e.g., `aoj_ALDS1_1_A_cpp`). |
| `problem_description` | string | Full, cleaned text description of the coding challenge. |
| `candidates` | List | List of ranked submissions for this problem. |

**Key Ground Truth Fields (`candidates` array):**

| Field Name | Type | Meaning |
| :--- | :--- | :--- |
| `submission_id` | string | AOJ Submission ID. |
| **`accuracy`** | float | **Primary Sort Key.** Submission correctness (0.0 to 1.0). |
| `time_ms` | integer | Actual Execution Time (in milliseconds). |
| **`score_of_the_acc`** | float | **Secondary Sort Key.** Normalized efficiency score (Time/Memory). |
| **`final_rank`** | integer | **Final Competition Rank** (1-2-2-4 rule). |

### 🔍 Ranking Logic (The Core Value)

The final rank is determined by a strict two-tiered system:

1.  **Primary Sort:** Sort by **`accuracy`** (Descending).
2.  **Secondary Sort:** Sort by **`score_of_the_acc`** (Descending).

**Efficiency Score Formula:**
The efficiency score is the negative sum of normalized time and memory costs.
$$\text{Score} = -(\text{Norm\_Time} + \text{Norm\_Memory})$$

## 2. 📁 Raw Data Archive (For Validation)

**File Name: `AOJ-Raw-Nested-Archive.jsonl`**

This file retains the original nested structure, primarily for data validation and debugging the transformation pipeline. **It should not be used for final LLM evaluation.**

### Structure (JSONL v1 - Nested)

| Field Name | Structure | Description |
| :--- | :--- | :--- |
| `group_id` | string | Task ID. |
| `elements` | List | Contains only `id` and the mixed `data` string (Description + Code). |
| `ground_truth` | **Dictionary** | Separate dictionary mapping submission IDs to all performance metrics. |

---

## 3. License and Acknowledgments

* **License**: CC-BY-SA-4.0
* **Data Source**: Aizu Online Judge (AOJ).
