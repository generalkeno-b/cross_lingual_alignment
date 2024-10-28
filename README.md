## Setup

1. Install necessary libraries.
2. Download the dataset through the provided scripts.
3. Execute the notebook cells.
   - If you want to experiment with different hyperparameters, re-run the model training and the following three cells to test performance.

## Project Overview

### Cross-Lingual Embedding Alignment Process

1. **Initial Setup with Pretrained FastText Models**:  
   I began by using pretrained English and Hindi FastText embeddings. A bilingual English-Hindi dictionary provided in the repository was split into training and test sets. The embeddings for both languages were centered, after which I applied Procrustes alignment using the training set to find the orthogonal rotation matrix that aligns the English embeddings to the Hindi ones.

2. **Evaluation Metrics**:  
   After alignment, I used the test set to evaluate the following metrics:
   - Precision@1
   - Precision@5
   - Pairwise cosine similarities

The initial results were:
- Precision@1: **0.3259**
- Precision@5: **0.5185**

---

### Training Custom Embeddings

Recognizing that pretrained embeddings might differ due to training on distinct corpora, I decided to train my own models using the **NLLB en-hi dataset**. This bilingual dataset is consistent across both languages, allowing better alignment. 

The results improved:
- Precision@1: **0.3691**
- Precision@5: **0.5208**

This confirmed my hypothesis that training on the same corpus improves alignment.

---

### Experimenting with Larger Training Corpus

Next, I augmented the NLLB dataset with the **CCMatrix bilingual dataset**. However, the additional data resulted in decreased precision scores.

This may have been due to the lower quality of the CCMatrix dataset or other factors yet to be identified.

---

### Hyperparameter Optimization

1. **Epochs**:  
   I experimented by increasing the number of training epochs from 5 to 20, but it did not improve the precision scores. This suggests a possible divergence in 
   loss or another underlying issue:
   - Precision@1: **0.3712**
   - Precision@5: **0.5176**
   
3. **Embedding Dimension and Context Window**:  
   By increasing the output dimension from 100 to 150 and the context window from 5 to 8 (with 5 epochs), I observed improved results:
   - Precision@1: **0.4100**
   - Precision@5: **0.5610**

4. **Further Tuning**:  
   Increasing the embedding dimension to 200, context window to 15, and training for 10 epochs further boosted the metrics:
   - Precision@1: **0.4327**
   - Precision@5: **0.5850**

---

## Results Summary

| Model Setup                                   | Precision@1 | Precision@5 |
|-----------------------------------------------|-------------|-------------|
| Pretrained FastText (English & Hindi)         | 0.3259      | 0.5185      |
| Custom Trained (NLLB Dataset)                 | 0.3691      | 0.5208      |
| NLLB (20 Epochs)                              | 0.3712      | 0.5176      |
| NLLB (150D, Window 8, 5 Epochs)               | 0.4100      | 0.5610      |
| NLLB (200D, Window 15, 10 Epochs)             | 0.4327      | 0.5850      |

## Conclusion

With optimized training settings—larger embedding dimensions, wider context windows, and balanced epochs—the cross-lingual alignment results improved significantly. Further tuning and testing can likely push these results even higher with more time and resources.
