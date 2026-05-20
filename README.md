# Breast-Cancer-Diagnosis-Using-Machine-Learning-and-Visualization-Based-Feature-Engineering
# Scalable Real-Time Detection of AI-Generated Arabic Text

## Project Overview

This project implements a scalable Big Data pipeline for detecting AI-generated Arabic academic abstracts. The goal is to classify Arabic text as either human-written or AI-generated using distributed data processing, Arabic-specific preprocessing, stylometric feature engineering, Spark MLlib models, and stream simulation.

The project was developed using Apache Spark, PySpark, Spark DataFrames, Spark MLlib, Parquet storage, and a manual micro-batch streaming simulation.

---

## Problem Statement

Large Language Models (LLMs) can generate high-quality text that is difficult to distinguish from human-written content. This creates challenges in academic integrity, digital trust, and Arabic content verification.

Arabic AI-generated text detection is especially important because Arabic has fewer detection tools and resources compared with English. This project addresses the problem by building a scalable pipeline for detecting AI-generated Arabic abstracts.

---

## Objectives

The main objectives of this project are:

- Build a scalable Big Data pipeline using Apache Spark.
- Process Arabic text using Spark DataFrames.
- Apply Arabic-specific preprocessing.
- Store processed data in an efficient columnar format.
- Extract traditional stylometric features.
- Generate scalable TF-IDF representations.
- Train and compare Spark MLlib models.
- Simulate real-time AI-text detection using micro-batches.
- Benchmark batch processing time and stream latency/throughput.

---

## Dataset

The dataset used in this project is:

**KFUPM-JRCAI/arabic-generated-abstracts**

The dataset contains Arabic academic abstracts and AI-generated versions from multiple models, including:

- ALLAM
- JAIS
- LLaMA
- OpenAI

The original dataset contains **8,388 human-written Arabic abstracts**. Each abstract has multiple AI-generated versions, so the dataset was converted into a binary classification format.

### Binary Classification Labels

- `human`: original human-written Arabic abstracts
- `ai_generated`: AI-generated Arabic abstracts

### Full Dataset Distribution

| Class | Count |
|---|---:|
| Human-written texts | 8,388 |
| AI-generated texts | 33,552 |
| Total records | 41,940 |

During initial exploration, **5,427 duplicate text records** were found. Duplicate handling was important to reduce the risk of data leakage between training and testing sets.

For local experimentation, a smaller sampled dataset was used to reduce execution time and memory usage.

---

## Project Pipeline

The project pipeline consists of the following stages:

1. Data acquisition and loading
2. Initial data exploration
3. Arabic text preprocessing
4. Storage in Parquet format
5. Exploratory Data Analysis
6. Traditional stylometric feature extraction
7. TF-IDF feature engineering
8. Train/validation/test splitting
9. Batch model training
10. Model evaluation
11. Stream simulation
12. Scalability benchmarking
13. Feature importance analysis

---

## Arabic Text Preprocessing

The preprocessing pipeline includes:

- Column renaming
- Null value handling
- Duplicate text removal
- Arabic normalization
- Diacritics removal
- Tatweel removal
- Non-Arabic symbol cleaning
- Whitespace normalization
- Tokenization
- Arabic stop-word removal
- Light stemming / lemmatization

Processed data was saved in **Parquet** format for efficient Spark processing.

---

## Feature Engineering

### Traditional Stylometric Features

Four traditional linguistic/stylometric features were extracted:

1. **Honoré’s R measure**  
   Measures lexical richness.

2. **Noun frequency**  
   Measures the frequency of nouns and proper nouns.

3. **Genitive construction count**  
   Counts likely Arabic Idafa/genitive constructions.

4. **Entity density**  
   Measures the density of named entities in the text.

The final stylometric dataset contains:

```text
text
label
honore_r
noun_frequency
genitive_count
entity_density
```

### Advanced Features

TF-IDF was used as a scalable text representation using Spark MLlib:

- RegexTokenizer
- StopWordsRemover
- CountVectorizer
- IDF

---

## Machine Learning Models

Three Spark MLlib models were trained and evaluated:

1. Logistic Regression
2. Random Forest
3. Linear SVM

The models were evaluated using:

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Confusion matrix

---

## Model Results

| Model | Accuracy | Precision | Recall | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 0.6755 | 0.6769 | 0.6755 | 0.6754 | 0.7560 |
| Random Forest | 0.6879 | 0.6900 | 0.6879 | 0.6877 | 0.7538 |
| Linear SVM | 0.6950 | 0.6997 | 0.6950 | 0.6942 | 0.7584 |

The best-performing model was **Linear SVM**, which achieved:

- Accuracy: **0.6950**
- F1-score: **0.6942**
- ROC-AUC: **0.7584**

---

## Confusion Matrix Summary

Assuming the label mapping is:

```text
0 = ai_generated
1 = human
```

### Logistic Regression

```text
True 0, Predicted 0: 193
True 0, Predicted 1: 82
True 1, Predicted 0: 101
True 1, Predicted 1: 188
```

### Random Forest

```text
True 0, Predicted 0: 199
True 0, Predicted 1: 76
True 1, Predicted 0: 100
True 1, Predicted 1: 189
```

### Linear SVM

```text
True 0, Predicted 0: 208
True 0, Predicted 1: 67
True 1, Predicted 0: 105
True 1, Predicted 1: 184
```

---

## Feature Importance / Coefficients

Feature coefficient analysis showed that **noun frequency** and **entity density** were the most influential features.

| Feature | Coefficient | Absolute Coefficient |
|---|---:|---:|
| noun_frequency | -15.7302 | 15.7302 |
| entity_density | -12.9217 | 12.9217 |
| genitive_count | 0.0449 | 0.0449 |
| honore_r | 0.0002 | 0.0002 |

The results show that noun usage and named entity density were the strongest stylometric indicators for distinguishing human-written and AI-generated Arabic text.

---

## Stream Simulation

A manual micro-batch stream simulation was implemented to avoid local notebook checkpoint issues with Spark Structured Streaming.

Each CSV file represented a new incoming batch of Arabic abstracts. The stream simulation used the stylometric feature dataset and processed each batch sequentially.

For each batch, the system:

1. Loaded the batch file.
2. Assembled the four stylometric features.
3. Applied the trained model.
4. Generated predicted labels.
5. Measured latency and throughput.

---

## Stream Benchmark Results

The stream benchmark used:

- 10 batches
- 10 records per batch
- 100 total streamed records

| Metric | Result |
|---|---:|
| Average latency | 0.0950 seconds |
| Minimum latency | 0.0726 seconds |
| Maximum latency | 0.1407 seconds |
| Average throughput | 109.60 records/second |
| Minimum throughput | 71.08 records/second |
| Maximum throughput | 137.78 records/second |

The results show that the micro-batch streaming simulation can process small incoming Arabic text batches efficiently.

---

## Scalability Benchmark

Batch processing was benchmarked using different Spark local resource settings.

| Spark Mode | Cores | Batch Time | Accuracy | F1-score | ROC-AUC |
|---|---:|---:|---:|---:|---:|
| local[1] | 1 | 2.6712 sec | 0.9792 | 0.9792 | 0.9966 |
| local[2] | 2 | 2.2059 sec | 0.9792 | 0.9792 | 0.9964 |
| local[4] | 4 | 1.8863 sec | 0.9792 | 0.9792 | 0.9964 |

Increasing Spark local cores reduced batch processing time while model performance remained stable.

---

## Batch vs Stream Processing

Batch processing was used for:

- Feature engineering
- Model training
- Model tuning
- Full evaluation

Stream processing was used for:

- Real-time prediction simulation
- Micro-batch inference
- Latency and throughput measurement

Batch processing provides stronger evaluation and supports heavier feature engineering. Stream processing is more suitable for real-time deployment but requires careful attention to latency, throughput, and feature consistency.

---

## Technologies Used

- Python
- Apache Spark
- PySpark
- Spark DataFrames
- Spark MLlib
- Parquet
- Pandas
- Matplotlib
- Jupyter Notebook / VS Code
- Stanza / Arabic NLP tools
- Git and GitHub

---

## Limitations

This project has several limitations:

- A smaller sampled dataset was used during local development.
- The streaming component was implemented as manual micro-batch simulation rather than full Kafka deployment.
- POS and NER-based feature extraction can be computationally expensive.
- The model performance was moderate, showing that four stylometric features alone may not fully capture the complexity of AI-generated Arabic text.
- The dataset focuses on academic abstracts, so results may not generalize to all Arabic domains.

---

## Future Work

Future improvements include:

- Train on the full dataset using a stronger Spark cluster.
- Deploy a full Kafka + Spark Structured Streaming pipeline.
- Add transformer-based Arabic embeddings such as AraBERT.
- Compare Spark MLlib models with deep learning models.
- Add more Arabic linguistic and semantic features.
- Evaluate on Arabic news, essays, social media, and dialectal text.
- Optimize real-time feature extraction for production deployment.

---

## Conclusion

This project demonstrates a scalable Spark-based pipeline for detecting AI-generated Arabic text. The system supports distributed preprocessing, feature engineering, model training, stream simulation, and scalability benchmarking.

Linear SVM achieved the best overall model performance, while noun frequency and entity density were the most influential stylometric features. The scalability results showed that increasing Spark cores improved batch processing time, and the stream simulation achieved low latency and high throughput for small micro-batches.

Overall, the project shows that Big Data tools such as Apache Spark can support both batch and near real-time AI-generated Arabic text detection.

