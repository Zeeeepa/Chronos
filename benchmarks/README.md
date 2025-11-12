# Kodezi Chronos Benchmarks

This directory contains the evaluation benchmarks used to assess Chronos's debugging capabilities. Note that these are benchmark specifications and protocols - the actual Chronos model is only available through Kodezi OS.

## Benchmark Overview

### 1. SWE-bench Lite (Industry Standard Benchmark)

**State-of-the-Art Performance Achieved:**

Chronos achieves the highest performance on SWE-bench Lite, the industry-standard debugging benchmark:

| Rank | System | Success Rate | Instances Resolved |
|------|--------|--------------|-------------------|
| 🥇 **1** | **Kodezi Chronos** | **80.33%** | **241/300** |
| 🥈 2 | ExpeRepair-v1.0 + Claude 4.5 Sonnet | 60.33% | 181/300 |
| 3 | Claude 4.5 Sonnet (Bash Only) | ~14% | ~42/300 |
| 4 | Claude 4.1 Opus (Bash Only) | 14.2% | 43/300 |
| 5 | GPT-4.1 | 13.8% | 41/300 |
| 6 | Gemini 2.0 Pro | 13.4% | 40/300 |

**Key Achievement**: 20 percentage point absolute lead over second place

**Repository-Specific Performance:**
- **sympy** (symbolic mathematics): 96.1%
- **sphinx** (documentation systems): 93.8%
- **django** (web frameworks): 90.4%

**The Debugging Gap**: General-purpose models achieving 70%+ on code generation (SWE-bench Full) drop to <15% on debugging tasks (SWE-bench Lite), revealing a 50+ percentage point gap. Chronos's specialized debugging architecture bridges this gap.

### 2. Multi Random Retrieval (MRR) Benchmark

Our novel benchmark designed specifically for debugging-oriented retrieval capabilities:

- **5,000 real-world debugging scenarios**
- **12,500 total bugs evaluated across all benchmarks**
- **Context scattered across 10-50 files**
- **Temporal dispersion spanning 3-12 months**
- **Obfuscated dependencies with refactored names**
- **Multi-modal artifacts (code, tests, logs, docs)**

**Key Metrics:**
- Retrieval Precision@k (92% achieved)
- Retrieval Recall@k (85% achieved)  
- Fix Accuracy (67.3% ± 2.1%)
- Context Efficiency (O(k log d) complexity)
- Human Preference (89% N=50)

### 3. Debugging Task Categories

We evaluate across six major bug categories:

| Category | Description | Test Cases |
|----------|-------------|------------|
| **Syntax** | Syntax errors and typos | 500 |
| **Logic** | Logical errors in algorithms | 1,200 |
| **Concurrency** | Race conditions, deadlocks | 800 |
| **Memory** | Memory leaks, buffer overflows | 600 |
| **API** | API misuse, version conflicts | 900 |
| **Performance** | Performance regressions | 400 |

**Total Test Cases**: 4,400 (expanded to 12,500 with variations)

### 4. Repository Scale Tests

Testing debugging performance across different codebase sizes:

- Small: <10K LOC
- Medium: 10K-100K LOC
- Large: 100K-1M LOC
- Enterprise: >1M LOC

## Benchmark Results Summary

### Overall Performance

| Model | Debug Success | Root Cause Acc. | Avg. Fix Iterations |
|-------|---------------|-----------------|-----------------|
| GPT-4.1 | 13.8% | 12.3% | 1-2 |
| Claude 4.1 Opus | 14.2% | 11.7% | 1-2 |
| Gemini 2.0 Pro | 15.0% | 15.8% | 1-2 |
| **Kodezi Chronos** | **67.3%** | **89%** | **7.8** |

### MRR Benchmark Performance

| Model | Precision@10 | Recall@10 | Fix Accuracy |
|-------|--------------|-----------|--------------|
| GPT-4.1 + RAG | 42.3% | 31.7% | 8.9% |
| Claude 4.1 Opus + Vector DB | 48.1% | 36.2% | 11.2% |
| Gemini 2.0 Pro + Graph | 51.7% | 41.8% | 14.6% |
| **Kodezi Chronos** | **92%** | **85%** | **67.3%** |

## Evaluation Protocol

### 1. Test Case Selection
- Randomly sampled from real-world bug reports
- Verified by human developers
- Categorized by complexity and type

### 2. Evaluation Process
1. Present bug report/symptoms to model
2. Measure retrieval accuracy
3. Evaluate proposed fix
4. Run automated tests
5. Check for regressions
6. Measure end-to-end success

### 3. Fairness Considerations
- All models tested on identical scenarios
- Same computational resources allocated
- Human verification of results
- Statistical significance testing

## Running Benchmark Evaluations

While the Chronos model itself is not publicly available, researchers can:

1. **Use our test scenarios** to evaluate their own models
2. **Follow our protocols** for consistent evaluation
3. **Compare results** using our metrics

### Example Evaluation Script

```python
# This is a conceptual example - actual implementation requires model access
from benchmarks import MRRBenchmark, DebugTaskEvaluator

# Load benchmark
benchmark = MRRBenchmark.load("./multi-random-retrieval/mrr_v1.json")

# Evaluate your model
evaluator = DebugTaskEvaluator(your_model)
results = evaluator.run_benchmark(benchmark)

# Compare with Chronos results
comparison = results.compare_with_baseline("chronos_results.json")
print(comparison.summary())
```

## Benchmark Data Format

### MRR Task Format
```json
{
  "task_id": "mrr_001",
  "bug_description": "NullPointerException in user export after auth refactor",
  "repository_snapshot": "path/to/repo/snapshot",
  "relevant_files": ["auth/service.py", "export/handler.py", ...],
  "ground_truth_fix": {
    "files_modified": [...],
    "patch": "...",
    "test_results": "all_pass"
  },
  "metadata": {
    "bug_category": "null_pointer",
    "complexity": "medium",
    "cross_file_dependencies": 3
  }
}
```

## Contributing

We welcome contributions to improve our benchmarks:

1. **New test cases** - Submit real-world debugging scenarios
2. **Evaluation metrics** - Propose new ways to measure debugging effectiveness
3. **Baseline comparisons** - Add results from other models/tools

Please see [CONTRIBUTING.md](../CONTRIBUTING.md) for guidelines.

## Citation

If you use these benchmarks in your research:

```bibtex
@article{khan2025chronos,
  title={Kodezi Chronos: A Debugging-First Language Model for Repository-Scale, Memory-Driven Code Understanding},
  author={Khan, Ishraq and Chowdary, Assad and Haseeb, Sharoz and Patel, Urvish},
  journal={arXiv preprint arXiv:2507.12482},
  year={2025}
}
```