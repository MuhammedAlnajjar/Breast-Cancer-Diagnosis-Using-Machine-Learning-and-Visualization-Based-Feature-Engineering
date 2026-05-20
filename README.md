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
