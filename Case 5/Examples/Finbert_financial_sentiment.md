## Financial Sentiment Analysis on Stock Market Headlines With FinBERT & HuggingFace

Authored by ivangoncharov.

Financial news headlines are abundant sources of NLP data, especially when predicting stock performance. This is usually achieved via sentiment analysis, which categorizes phrases into positive, negative, and neutral sentiments. In this guide, we discuss FinBERT and sentiment analysis.

### 🚀 [Follow along on this Google Colab!](https://colab.research.google.com/drive/1C6_ahu0Eps_wLKcsfspEO0HIEouND-oI?usp=sharing)

#### Note: All the code in this example is ran in a jupyter notebook.

### Table of Contents:

1. [What is FinBERT?](#what-is-finbert)
2. [The Importance Of Sentiment Analysis in Finance ML](#importance-of-sentiment-analysis)
3. [Downloading the Stock Market News Data from Kaggle](#download-data)
4. [FinBERT Using HuggingFace](#finbert-using-huggingface)
5. [Running Inference with FinBERT and Stock Market News Headlines](#running-inference)
6. [Visualizing the Results Interactively as a W&B Table](#visualizing-results)
7. [Tips & Tricks for Interacting with W&B Tables](#tips-and-tricks)

<a name="what-is-finbert"></a>

### What is FinBERT?

[FinBERT](https://arxiv.org/abs/1908.10063) is a pre-trained NLP model based on [BERT](https://wandb.ai/mukilan/BERT_Sentiment_Analysis/reports/An-Introduction-to-BERT-And-How-To-Use-It--VmlldzoyNTIyOTA1), Google's revolutionary [transformer](https://wandb.ai/fully-connected/blog/transformer) model. It's essentially BERT but trained specifically on financial data for sentiment analysis.

<a name="importance-of-sentiment-analysis"></a>

### The Importance Of Sentiment Analysis in Finance ML

Sentiment analysis is prevalent in NLP, aiming to assign emotions to text. In the financial domain, changes in sentiment around a company could predict stock fluctuations. FinBERT was trained on financial news and the [FiQA dataset](https://sites.google.com/view/fiqa/home).

<a name="download-data"></a>

### Downloading the Stock Market News Data from Kaggle

For this guide, we use the ["Daily Financial News for 6000+ Stocks"](https://www.kaggle.com/miguelaenlle/massive-stock-news-analysis-db-for-nlpbacktests?select=raw_analyst_ratings.csv) dataset from Kaggle.

```python
!git clone https://gist.github.com/c1a8c0359fbde2f6dcb92065b8ffc5e3.git
```

Read the CSV and preview:

```python
import pandas

headlines_df = pandas.read_csv('c1a8c0359fbde2f6dcb92065b8ffc5e3/300_stock_headlines.csv')
headlines_df.head(5)
```

<a name="finbert-using-huggingface"></a>

### FinBERT Using HuggingFace

[HuggingFace](https://wandb.ai/fully-connected/blog/hugging-face) makes experimenting with NLP models straightforward.

```python
!pip install transformers

from transformers import AutoTokenizer, AutoModelForSequenceClassification

tokenizer = AutoTokenizer.from_pretrained("ProsusAI/finbert")
model = AutoModelForSequenceClassification.from_pretrained("ProsusAI/finbert")
```

<a name="running-inference"></a>

### Running Inference with FinBERT and Stock Market News Headlines

Tokenize and preprocess headlines:

```python
inputs = tokenizer(headlines_list, padding=True, truncation=True, return_tensors='pt')
print(inputs)
```

Run inference:

```python
outputs = model(**inputs)
print(outputs.logits.shape)
```

Post-process outputs:

```python
import torch

predictions = torch.nn.functional.softmax(outputs.logits, dim=-1)
print(predictions)
```

<a name="visualizing-results"></a>

### Visualizing the Results Interactively as a W&B Table

```python
import pandas as pd

positive = predictions[:, 0].tolist()
negative = predictions[:, 1].tolist()
neutral = predictions[:, 2].tolist()

table = {
    'Headline': headlines_list,
    "Positive": positive,
    "Negative": negative,
    "Neutral": neutral
}

df = pd.DataFrame(table, columns=["Headline", "Positive", "Negative", "Neutral"])
df.head(5)
```

Logging to W&B:

```python
!pip install wandb
import wandb

wandb.init(project="FinBERT_Sentiment_Analysis_Project")

wandb.run.log({"Financial Sentiment Analysis Table": wandb.Table(dataframe=df)})
wandb.run.finish()
```

<a name="tips-and-tricks"></a>

### Tips & Tricks for Interacting with W&B Tables

You can easily filter and sort results using the

W&B tables' interface. This provides an interactive way to analyze model outputs.

---

That's all for this guide! Make sure to check out the links and experiment with your own datasets! Happy modeling! 🚀📈
