# Data Visualization and Text Mining

**Università Cattolica del Sacro Cuore**
Academic Year 2025-2026

## About

This repository contains notebooks and materials for the Data Visualization and Text Mining course. Notebooks will be published the day before each lesson.

## Course Contents

### 01 - NLP In Practice
Introduction to Natural Language Processing using spaCy and NLTK.
- Tokenization, Stemming, Lemmatization
- Stop Words
- Part of Speech (POS) Tagging
- Named Entity Recognition (NER)
- Visualization with displacy
- EDA on Text Data with matplotlib and seaborn

### 02 - Text Classification
Apply Machine Learning to text data sources.
- Scikit-Learn basics
- N-grams
- Bag-of-Words, CountVectorizer, TfidfVectorizer
- Text Classification project

### 03 - Neural Networks
Introduction to Deep Learning for NLP.
- Simple Classifier with Keras
- PyTorch basics
- Building Neural Network classifiers

### 04 - Embeddings
Word vectors and text representation.
- Feature Engineering for Text Data
- Word2Vec (CBOW and Skip-gram)
- GloVe embeddings

## Getting Started

### Clone the Repository

```bash
git clone <repository-url>
cd text-mining-dataviz-aa2526
```

### Stay Updated

Since new content is added before each lesson, make sure to pull the latest changes regularly:

```bash
git pull origin main
```

**Tip:** Run this command before starting a lesson to get the new notebooks.

## Open Notebooks in Google Colab

You can open any notebook directly in Google Colab without cloning the repository:

1. Navigate to the notebook file on GitHub
2. Replace `github.com` in the URL with `colab.research.google.com/github`

**Example:**
```
# Original GitHub URL
https://github.com/username/text-mining-dataviz-aa2526/blob/main/notebook.ipynb

# Colab URL
https://colab.research.google.com/github/username/text-mining-dataviz-aa2526/blob/main/notebook.ipynb
```

Alternatively, from Google Colab:
1. Go to [colab.research.google.com](https://colab.research.google.com)
2. Click **File** → **Open notebook**
3. Select the **GitHub** tab
4. Enter the repository URL and select the notebook you want to open

**Note:** Changes made in Colab are saved to your Google Drive, not to the repository. To keep your work, save a copy to your Drive.

## Workflow for Each Lesson

1. **Before the lesson:** Pull the latest updates
   ```bash
   git pull origin main
   ```

2. **During the lesson:** Work through the notebooks

3. **After the lesson:** Your local changes won't conflict with future updates as long as you don't modify the original files. If you want to experiment, consider creating a copy of the notebook.

## Handling Conflicts

If you've made changes to a notebook and encounter conflicts when pulling:

```bash
# Option 1: Stash your changes, pull, then reapply
git stash
git pull origin main
git stash pop

# Option 2: Discard local changes and get the latest version
git checkout -- .
git pull origin main
```

## Tools

The [tools/](tools/) folder contains helpful resources and tutorials:

- [Git-Quick-Tutorial.md](tools/Git-Quick-Tutorial.md) - A quick reference guide for Git commands and workflows
- [Git-Exercises.md](tools/Git-Exercises.md) - Practice exercises to reinforce Git skills

## Contact

For questions about the course materials, please contact the instructor:

**Andrea Belli** - [andrea.belli@unicatt.it](mailto:andrea.belli@unicatt.it)
