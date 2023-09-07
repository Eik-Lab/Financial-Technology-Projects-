## Financial Sentiment Analysis on Stock Market Headlines With FinBERT & HuggingFace

**The original article can be found here:**  
https://wandb.ai/ivangoncharov/FinBERT_Sentiment_Analysis_Project/reports/Financial-Sentiment-Analysis-on-Stock-Market-Headlines-With-FinBERT-HuggingFace--VmlldzoxMDQ4NjM0


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

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/48220549/50adcd60-c6ce-420d-8015-7be69a210011)



```python
!git clone https://gist.github.com/c1a8c0359fbde2f6dcb92065b8ffc5e3.git
```

Read the CSV and preview:

```python
import pandas

headlines_df = pandas.read_csv('c1a8c0359fbde2f6dcb92065b8ffc5e3/300_stock_headlines.csv')
headlines_df.head(5)
```
After reading the data, we'll shuffle the  entries, and convert them to a Python list with just the headlines. This is the input to the FinBERT model.

```python
import numpy as np


headlines_array = np.array(headlines_df)
np.random.shuffle(headlines_array)
headlines_list = list(headlines_array[:,2])


print(headlines_list)
```

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/48220549/f370ff0e-18ad-4df4-823d-e09215dc78a3)



<a name="finbert-using-huggingface"></a>

### FinBERT Using HuggingFace

[HuggingFace](https://wandb.ai/fully-connected/blog/hugging-face) makes experimenting with NLP models straightforward. You can even test the model locally.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/48220549/714fadf0-58a8-4168-855d-78ed319004b5)

We'll start working with the NLP model by installing the HuggingFace transformers library. 

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

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/48220549/5a69007b-79ce-442c-9dac-1475160b3c96)

Logging to W&B:

```python
!pip install wandb
import wandb

wandb.init(project="FinBERT_Sentiment_Analysis_Project")

wandb.run.log({"Financial Sentiment Analysis Table": wandb.Table(dataframe=df)})
wandb.run.finish()
```
<a name="visualizing-the-w&b-table"/>

Now that we've logged the Table, it will start a new run in our project and print something like this in the console. We can click on this "Run page" link to open our Run Page dashboard and see the W&B Table we've created. 

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/48220549/f7654d99-6037-4c2d-b3f7-6c9d171b8dbf)


Here is how it looks at our end.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/48220549/02c66f44-aa7e-4046-bee7-0a3534cf9764)


<a name="tips-and-tricks"></a>

### Tips & Tricks for Interacting with W&B Tables

#### Filtering

You can easily filter and sort results using the

W&B tables' interface. This provides an interactive way to analyze model outputs.

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/48220549/76c220ed-742f-4b22-a2db-b662ca51622f)


#### Sorting

We can sort the Positive, Negative and Neutral  columns in ascending or descending order. You can think of this finding "the most positive/negative/neutral" headlines. 

![image](https://github.com/Eik-Lab/NBIM-hackathon/assets/48220549/800e9519-294b-4d2c-8d86-0cff7f172661)


---

That's all for this guide! Make sure to check out the links and experiment with your own datasets! Happy modeling! 🚀📈
