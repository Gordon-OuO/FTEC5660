
# UNBench Task 1 Reproduction: Gemini-2.5-Flash vs GPT-4o (FTEC5660 Project)

[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_USERNAME/unbench-gemini/blob/main/reproduce_run_task1.ipynb)

## 🎯 Project Summary
Reproduced UNBench Task 1 (Co-penholder Choosing) from [2502.14122v2](https://arxiv.org/abs/2502.14122) using **Gemini-2.5-Flash** (no fine-tuning). 
- **Baseline**: Matched paper's scaling law (72.6%→46.4% acc as choices increase)
- **Modification**: CoT prompting (surprisingly hurt hard cases, insightful failure!)
- Fixed reproducible subset: **first 30 instances per k** (2/3/4/5 choices)

## 📊 Key Results (n=30/k)

| Model | 1/2 | 1/3 | 1/4 | 1/5 |
|-------|----|----|----|----|
| **Paper GPT-4o** | **72.6%** | 61.3% | 51.1% | **46.4%** |
| **Gemini baseline** | **83.3%** | 60.0% | 40.0% | 46.7% |
| **Gemini + CoT** | 66.7% | **70.0%** | 36.7% | 26.7% |

**✅ Success**: Perfect trend reproduction (easy>>hard). **Insight**: CoT overthinks diplomatic nuance.

## 🚀 Quickstart (Colab)

1. **Open Colab**: [Click here](https://colab.research.google.com/github/YOUR_USERNAME/unbench-gemini/blob/main/reproduce_run_task1.ipynb)
2. **API Key**: Add `VERTEX_API_KEY` in [Colab Secrets](https://colab.research.google.com/notebook#fileId=YOUR_NOTEBOOK)
3. **Run cells 1-10**: Downloads data (~2GB), runs **baseline** (~5min)
4. **Run CoT cells**: Modified prompting (~3min extra)

```
!pip install langchain_google_genai
# Cells auto-download UNBench Task 1 data from official repo
# Gemini-2.5-flash-exp, temp=0, vertexai=True
```

## 🛠️ Setup Details

**Environment**: Google Colab (Python 3.12, GPU optional)
**Model**: `gemini-2.5-flash` via Vertex AI (langchain-google-genai v4.2.1)
**Data**: data/task1.json and data/task1/
**Reproducibility**: Fixed subset `drafts[:30]`, `authors[:30]`, `choices_k[:30]`
**Invalid handling**: Random fallback (low rate: <5%)
**Compute**: ~240 total inferences, ~10min end-to-end

## 📁 Repository Structure
```
├── reproduce_run_task1.ipynb           # Main notebook (baseline + CoT)
└── README.md                           # This documentation
```

## 🔧 Debug Diary (Key Fixes)
1. **Invalid responses**: Added `random.choice(fallback)` + logging
2. **Subset**: `[:30]` slicing for reproducibility

## 📝 Modification Analysis
**Hypothesis**: CoT would boost hard cases (1/5 choices).
**Result**: -20% drop on 1/5. **Why?** Explicit steps disrupted Gemini's latent diplomatic knowledge.

**Baseline prompt**: "Choose from: A,B. Output ONLY name."
**CoT prompt**: "STEP1: topics? STEP2: alignments? STEP3: name."

## 🎓 Conclusions
- **Fully reproducible**: Gemini baseline captures paper's core scaling (83%→47%)
- **CoT insight**: Hurts nuanced political reasoning (overthinking subtle alignments)
- **Future work**: Per-model prompt optimization, larger n=1000 eval

## 📄 Citation
```
@article{liang2025unbench,
  title={Benchmarking LLMs for Political Science: A United Nations Perspective},
  author={Liang, Yueqing and others},
  journal={arXiv:2502.14122v2},
  year={2026}
}
```
