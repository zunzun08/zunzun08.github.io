---
layout: default
title: FinBERT 2 - Model Building
permalink: /docs/Portfolio/FinBERT_2/model_building
parent: FinBERT 2
---



# Introduction to Sentiment Analysis


Sentiment analysis is the popular classification technique for determining attitudes around a subject, entity, or event from natural language. The method and techniques of implementing sentiment analysis are long and well-documented from "simple" and intuitive models such as logistic regression and Naive Bayes classifier and training models using a collection of vocabulary words (Bag of Words) to highly sophisticated and state-of-the-art large language/reasoning models like GPT-4, BERT, Claude Opus 4 which are trained on a large corpus of text through the use of embedding layers that preserve word order. Sentiment analysis seems to be a persistent and well-researched topic surrounding these aforementioned methods of prediction.


### Sentiment Analysis Using BERT
Improvements in the sophistication of models with the rise of attention based architecture have significantly improved performance of sentiment analysis over previous models such as recurrent neural networks or convolution neural networks due to the positional embeddings the model learns through pre-training and the transformer's ability to process whole sentences rather than parse through text word by word. This feature combined with the method of masked language modeling and next word prediction allows transfomer models to understand context in ways in which CNNs or RNNs failed to do as effectively. The downside to the transformer model is its use of massive datasets and heavy computation costs, which are a direct result of its ability to process sentences. In the Attention Is All You Need paper that introduced the transformer architecture, researchers trained the BERT model on the entirety of the Wikipedia and BookCorpus corpora which took a few days of computation. However, we will overcome the limitation of pretraining BERT on the standard Wikipedia and Book corpora by using the pretrained BERT from the Huggingface library.

### Further Pretraining = Improved Results?
Text in financial data is notoriously tricky to detect sentiment due to the unique vocabulary and the lack of proper, and labeled data to train models on; this makes conducting financial sentiment analysis a difficult endeavor for previous machine learning methods. This is where FinBERT comes in. FinBERT is a variation of BERT designed specifically for application in the financial domain by using a method known as "further pre-training" which is training a BERT model on domain relevant corpora using the same training methods (masked language modeling and next sentence prediction) done during the training on the Wikipedia and BookCorpus corpora.
The authors of FinBERT decided to use the Reuters TRC2 dataset and then filter down to financial news articles for the pre-training of BERT; the idea of the pre-training is that it allows BERT to adjust its positional embeddings before it is fine-tuned on a domain-specific dataset that has sentiment labels using the typical train-test split. 

After pretraining, FinBERT was then fine-tuned on the Financial Phrasebank dataset, where it proved noticeable improvements over a standard BERT, ELMo, and (I had forgotten the last one). The result of this further pretraining demonstrated the effectiveness and possibility of making better, domain-specific models for the purpose of sentiment analysis.

# Why FinBERT 2?
The original FinBERT model was created in 2019. Since then, with a substantial rise in retail investors, the COVID-19 pandemic, and the impact of Artificial Intelligence, rising interest rates have made both changes in the way we communicate and the way markets, particularly stock markets, operate in a higher volatility environment.

It is my personal belief that the change in environment should be reflected in the data we use to train our large language models (LLMs), which is particularly important when it comes to training LLMs on domain-specific tasks, since we're dealing with more specialized tasks along with human emotion and handling of these tasks.

For that reason, the data we collect and use to train FinBERT 2 should reflect my sentiment. Ideally, we want an LLM that is knowledgeable in financial vocabulary and news but is also an excellent room reader and can understand the anger, frustration, skepticism that traders may hold towards a stock.

# Collecting Datasets

## Model Training and Data Collection
For the pretraining, we'll be using Huggingface extensively as it offers a variety of benefits when it comes to training LLMs. Using HuggingFace gives us access to a variety of large datasets that are perfect for training LLMs and are easy to access using the HuggingFace API in Python. HuggingFace also provides me with 100GB of storage where I can host my tokenized datasets so that they don't have to be loaded in locally. Most importantly, as noted earlier, HuggingFace provides easy access to the BERT model that is already pretrained on the Wikipedia and BookCorpus corpora along with the functions necessary to carry out the further pretraining. 

## Data Accessing
To access the Huggingface datasets, we'll be using PySpark over popular alternatives such as Pandas. The reason for this is that if we wanted to increase the number of datasets to manage, PySpark would easily be able to handle the processing and loading required by the data.



## Data Collection
For further pretraining, the two datasets we'll look to use are the [Reuters Financial Dataset](https://huggingface.co/datasets/danidanou/Reuters_Financial_News) that records Reuters Financial news articles from 2006-2013 and this corpora of [Financial News Articles](https://huggingface.co/datasets/ashraq/financial-news-articles) from what seems to be 2017-2018. Finally, we'll use the [FinTwit dataset](https://huggingface.co/datasets/StephanAkkerman/financial-tweets).