# WritingPreferenceBench

**Beyond Correctness: Evaluating Subjective Writing Preferences Across Cultures**

[![Dataset](https://img.shields.io/badge/Dataset-Available-green)](#dataset)
[![Demo](https://img.shields.io/badge/Demo-Interactive-orange)](demo.html)
[![License](https://img.shields.io/badge/License-MIT-yellow)](#license)

## Overview

WritingPreferenceBench is the first benchmark to systematically evaluate subjective writing preferences by neutralizing objective quality signals. Our dataset comprises **1,800 human-annotated preference pairs** (1,200 English, 600 Chinese) across 8 creative writing genres, where responses are matched for grammar, factuality, and length.

### Key Findings

- **Sequence classifiers fail**: Standard RLHF architectures achieve only 52.7% accuracy
- **Reasoning helps**: Generative reward models with explicit reasoning reach 81.8% accuracy
- **High variance**: Individual models vary 18-92% across genres, revealing brittle preference functions
- **Scale doesn't help**: 27B models show no consistent improvement over 8B variants

## 🔍 Quick Start


### Dataset Structure

```python
{
    "prompt": "Write a short story about...",
    "chosen": {
        "text": "Creative response with higher quality...",
        "score": 3,
        "length": 445
    },
    "rejected": {
        "text": "More formulaic response...",
        "score": 1,
        "length": 289
    },
    "genre": "fiction",
    "language": "en",
    "metadata": {
        "annotator_agreement": 0.85,
        "score_gap": 2
    }
}
```

## 📊 Results Summary

| Model Type | Architecture | Accuracy | Std Dev |
|------------|-------------|----------|---------|
| Sequence Classifiers | Discriminative head | 52.7% | 10.1% |
| Generative RMs | Reasoning + scoring | 65.9% | 9.2% |
| LLM Judges | Zero-shot evaluation | 53.9% | 11.4% |

**Best performers:**
- RM-R1-Qwen2.5-7B: 81.8% (EN), 73.3% (ZH)
- Doubao-1.5-Pro: 68.7% (EN), 62.5% (ZH)

## 🎯 Dataset Features

- **Signal Neutralization**: Systematic removal of objective quality indicators
- **Expert Annotation**: 7 trained annotators with calibrated 4-point creativity scale
- **Cross-Cultural**: Equivalent methodology across English and Chinese
- **Genre Diversity**: 8 categories from fiction to promotional writing
- **Quality Control**: Multi-stage validation ensures reliable preference gaps

## 📈 Evaluation Protocol

### For Reward Models
```python
def evaluate_reward_model(model, pairs):
    correct = 0
    for pair in pairs:
        chosen_score = model.score(pair.chosen)
        rejected_score = model.score(pair.rejected)
        if chosen_score > rejected_score:
            correct += 1
    return correct / len(pairs)
```

### For LLM Judges
```python
def evaluate_llm_judge(model, pairs):
    correct = 0
    for pair in pairs:
        preference = model.judge(pair.chosen, pair.rejected)
        if preference == "chosen":
            correct += 1
    return correct / len(pairs)
```

## 🧪 Reproducing Results

1. **Clone this repository**
```bash
git clone https://github.com/your-repo/WritingPreferenceBench
cd WritingPreferenceBench
```

2. **Install dependencies**
```bash
pip install -r requirements.txt
```

3. **Run evaluation**
```bash
python evaluate.py --model your_model --dataset data/writing_preference_bench.json
```

## 📊 Genre Breakdown

| Genre | English Pairs | Chinese Pairs | Description |
|-------|---------------|---------------|-------------|
| Fiction | 150 | 75 | Creative storytelling |
| Poetry | 150 | 75 | Verse and literary expression |
| Humor | 150 | 75 | Comedy and wit |
| Promotional | 150 | 75 | Marketing and persuasion |
| Non-Fiction | 150 | 75 | Informational writing |
| Functional | 150 | 75 | Practical communication |
| Scriptwriting | 150 | 75 | Dialogue and screenwriting |
| Role-Playing | 150 | 75 | Character-based writing |

## 🔬 Research Implications

### For RLHF
- Current sequence classifiers optimize for error detection, not quality recognition
- Generative architectures with reasoning show promise for subjective tasks
- High genre variance suggests need for more robust training objectives

### For Model Development
- Scale alone doesn't improve subjective preference modeling
- Intermediate reasoning representations are crucial
- Cross-lingual consistency requires specialized attention

### For Evaluation
- Objective benchmarks don't predict subjective performance
- Genre-specific evaluation reveals model brittleness
- Human annotation remains essential for creative domains

## 📄 Citation

```bibtex
@article{writingpreferencebench2025,
  title={Beyond Correctness: Evaluating Subjective Writing Preferences Across Cultures},
  author={Anonymous Authors},
  journal={xxxx},
  year={2025}
}
```

## 🤝 Contributing

We welcome contributions to improve WritingPreferenceBench:

- **Bug reports**: Found an issue? Please open a GitHub issue
- **Feature requests**: Have ideas for improvements? Let us know
- **Additional languages**: Interested in extending to other languages? Contact us
- **Model evaluation**: Want to evaluate your model? Follow our evaluation protocol


## 📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- Expert annotators for their careful evaluation work
- The creative writing community for genre taxonomy guidance
- Model providers for enabling comprehensive evaluation
- Reviewers and the broader research community for feedback

---

*WritingPreferenceBench: Advancing the science of subjective preference learning*