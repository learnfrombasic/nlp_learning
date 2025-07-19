## 🎯 Why These Evaluation Metrics?

### **The 3 Metrics Explained:**

**1. 🎯 Accuracy**
```python
accuracy = correct_predictions / total_predictions
```
- **What it measures**: Overall percentage of correct predictions
- **Strength**: Easy to interpret - "85% accuracy means 85% of languages were correctly identified"
- **When it's good**: When you care about overall system performance
- **Limitation**: Can be misleading with imbalanced datasets

---

**2. 📊 F1-Score (Macro)**  
```python
f1_macro = average(f1_score_per_language)
```
- **What it measures**: Average F1 score across ALL languages (treats each language equally)
- **Strength**: **Excellent for multilingual tasks** - gives equal weight to rare and common languages
- **Why crucial here**: Prevents model from ignoring low-resource languages
- **Example**: If model is great at English (5000 samples) but terrible at Estonian (50 samples), macro F1 will show this problem

---

**3. ⚖️ F1-Score (Weighted)**
```python
f1_weighted = weighted_average(f1_score_per_language, weights=language_frequency)
```
- **What it measures**: F1 score weighted by language frequency in dataset
- **Strength**: Reflects real-world performance where some languages are more common
- **Use case**: Better represents performance on your actual data distribution

---

### **🤔 Why This Combination for Multilingual NLP?**

| Scenario | Accuracy | F1-Macro | F1-Weighted | What It Tells You |
|----------|----------|-----------|-------------|-------------------|
| **Balanced Performance** | High | High | High | ✅ Model works well for all languages |
| **Majority Class Bias** | High | Low | High | ⚠️ Model ignores minority languages |
| **Minority Focus** | Low | High | Low | 🔍 Model prioritizes rare languages |
| **Poor Overall** | Low | Low | Low | ❌ Model struggles across the board |

### **🌍 Multilingual-Specific Considerations:**

**Why not just Accuracy?**
- Your dataset has 36+ languages with different frequencies
- Language 5: 5,000+ examples vs Language 35: 200 examples
- A model could get 90% accuracy by just being good at top 5 languages
- **F1-Macro catches this bias!**

**Why F1 instead of Precision/Recall alone?**
- **Precision**: "Of predictions for Swahili, how many were correct?"
- **Recall**: "Of actual Swahili texts, how many did we catch?"
- **F1**: Balances both - important for language ID where both matter

**Real-World Example:**
```
Model A: 92% Accuracy, 0.45 F1-Macro, 0.89 F1-Weighted
→ Good at common languages, terrible at rare ones

Model B: 88% Accuracy, 0.82 F1-Macro, 0.85 F1-Weighted  
→ Balanced across all languages (better choice!)
```

### **🎯 What to Look For:**

- **High F1-Macro**: Model respects linguistic diversity
- **F1-Weighted ≈ Accuracy**: Consistent performance pattern
- **Large gap between Weighted/Macro**: Indicates language bias
- **All metrics similar**: Well-balanced model (ideal!)

### **🔍 Alternative Metrics We Could Add:**

```python
# Per-language breakdown
precision_per_lang = precision_score(y_true, y_pred, average=None)
recall_per_lang = recall_score(y_true, y_pred, average=None)

# Top-K accuracy (for similar languages)
top_3_accuracy = top_k_accuracy_score(y_true, y_pred_proba, k=3)

# Confusion between language families
# (e.g., Germanic languages confused with each other)
```

**Why we didn't use these:**
- **Per-language metrics**: Too detailed for initial comparison
- **Top-K**: Less relevant for definitive language ID
- **Language family analysis**: Adds complexity without clear benefit for model selection
