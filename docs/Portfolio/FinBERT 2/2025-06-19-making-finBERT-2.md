---
layout: default
title: FinBERT 2
has_children: true
permalink: /docs/Portfolio/FinBERT_2
---



# Introduction

In quantitative finance, speed is everything. The faster news can be processed and identified as bullish or bearish, the faster a trader can adjust their strategy and know what securities can be moved. To address this need, I want to build a sentiment analysis model that quickly quantifies the sentiment of real-time, relevant financial news articles of publicly traded companies. To start, I need a model that achieves the following:

- Injest news articles from major American news sources along with news aggregators for sentiment on general, daily, stock market sentiment
- Store data obtained from web crawling, model training, and model predictions
- Store the trained Large Language Model for quick, one off predictions.

Ultimately, the system I have in mind should resemble the following:
<p align="center">
<img src="/assets/system_architecture (v1).png">
</p>

Users of the model, FinBERT 2 should be able to call the model for prediction and should also call the data used to train FinBERT and the previous headlines it has predicted. The construction of the system will be seperated into three parts including:

(WIP)
[Scraper Development]
(WIP)
[Database and pipeline Development]
