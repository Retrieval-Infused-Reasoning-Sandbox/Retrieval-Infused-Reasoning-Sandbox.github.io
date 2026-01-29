# RetrievalInfusedReasoningSandbox

## **🔍 Overview**

**DER²** (Decoupled Retrieval and Reasoning) is a controlled deep-research sandbox designed to isolate document-grounded reasoning from retrieval capabilities. While standard benchmarks evaluate end-to-end RAG pipelines, DER² decouples the two to operationalize "retrieval loss" vs. "reasoning loss".

### **Key Findings**

* **Mode-Switch Fragility**: Many models perform worse when provided with a full document set than with no documents at all, indicating that external context can disrupt parametric reasoning.

* **Structural Concept Misuse**: Models often correctly identify scientific concepts but fail to execute them as procedural steps (e.g., algorithm instantiation).

* **Retrieval vs. Reasoning**: Concept extraction remains a major source of loss; however, providing oracle concepts does not eliminate errors, revealing bottlenecks in multi-concept coordination.

* **Scaling Limits**: Performance degradation under noise is non-linear, suggesting that "deep research" is not a simple extension of RAG accuracy.


## **📊 Results Summary**

| Model Configuration | Accuracy (Avg) | Description |
| :---- | :---- | :---- |
| **Concepts-only** | 75.4% | Oracle concepts provided; measures pure reasoning  |
| **Related-only** | 62.9% | Only relevant documents; measures extraction \+ reasoning   |
| **Instruction-only** | 55.9% | No documents; measures parametric knowledge   |
| **Full-set** | 51.2% | Relevant \+ noise documents; measures denoising   |

**Best performers (Full-set Accuracy):**

1. **OpenAI-GPT-5.2-high**: 71.1%

2. **Gemini-3-Flash-Preview**: 66.0%

3. **OpenAI-GPT-5.1-high**: 57.0%


## 🎯 Dataset Features

* **Four Evaluation Regimes**: Progressively adds information (None → Concepts → Clean Docs → Noisy Docs) to isolate failure causes.

* **Two-Phase Validation**: Ensures tasks are unsolvable via parametric memory alone but solvable given oracle concepts.

* **Expert Curation**: Annotated by PhD students (Project 985\) in specialized academic fields.

* **Frozen Document Library**: Uses 2023–2025 theoretical papers to prevent parametric leakage from training data.

* **Process-Level Evaluation**: Includes expert-annotated Chain-of-Thought (CoT) rationales for error attribution.

## 📂 Dataset Structure

Each instance is defined by a tuple designed for fine-grained diagnosis:

```python
{
    "instruction": "Determine the time complexity for...",
    "concepts": [
        "Convex Body Sculpting",
        "Epsilon-net dimensionality reduction",
        "Subspace enumeration"
    ],
    "cot_reference": "Step 1: Obtain noisy estimate... Step 2: Construct convex body...",
    "answer": "O(m^3)",
    "doc_set": {
        "related": ["paper_segment_A.md", "formula_B.md"],
        "noise": ["adjacent_topic_X.md", "outdated_theory_Y.md"]
    },
    "metadata": {
        "domain": "Theoretical CS",
        "reasoning_depth": 8,
        "concept_count": 6
    }
}

```

---

## 📈 Evaluation Protocol

The benchmark operationalizes three specific loss types:

1. **Knowledge Loss**: Gap between *Concepts-only* and *Instruction-only*.
2. **Retrieval Loss (RLoss)**: Gap between *Concepts-only* and *Full-set*.
3. **Noise-induced Loss**: Gap between *Related-only* and *Full-set*.

Error Categories 

* **MC**: Missing core concept.
* **UC**: Misused/incorrect core concept.
* **R**: Reasoning-process error.
* **NF**: Numeric or formalization error.

---

## 📄 Citation

```bibtex
@article{der2_2026,
  title={Retrieval-Infused Reasoning Sandbox: A Benchmark for Decoupling Retrieval and Reasoning Capabilities},
  author={Ying, Shuangshuang and Wang, Zheyu and Peng, Yunjian and Chen, Jin and et al.},
  journal={arXiv preprint},
  year={2026}
}

```
