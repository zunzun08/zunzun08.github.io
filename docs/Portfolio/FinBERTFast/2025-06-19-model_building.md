---
layout: default
title: FinBERTFast - Model Building
permalink: /docs/Portfolio/FinBERTFast/model_building
parent: FinBERTFast
order: 1
---


## Initial Problem:
Sentiment analysis plays a crucial role in financial trading by classifying breaking news as bullish or bearish, enabling traders to adjust their buy/sell strategies accordingly. As part of a larger project to capture financial news sentiment, I've been experimenting with BERT (Bidirectional Encoder Representations from Transformers) as a model that can handle the complexity of financial language while avoiding the computational costs associated with billion-parameter large language models.
#### Why BERT?
As mentioned earlier, BERT can be relatively easy to train using Python libraries such as Huggingface and is an introductory model to language modeling that can also produce good results.

#### BERT's Not Enough:
One problem with BERT is that since the data the model is trained only includes the Wikipedia and Book corpus corpora, BERT may have difficulty capturing the bullish or bearish sentiment of financial news since it may be unfamiliar with the terminology or semantics of financial news. One recommendation to remedy this downside is to **fine-tune a BERT model** on a related dataset, fine-tuning allows the model to use and adjusts its parameter weights to make a prediction on a task from a domain specific dataset. While vanilla BERT provides good results on most sentiment analysis tasks, I believe we can squeeze a bit more performance out of BERT.

#### Further Pre-Training:
Another step I can take to optimizing BERT's performance is to **further pre-train BERT**-which is the technique of training BERT on a large, domain specific dataset using Masked Language Modeling (MLM), which is how BERT is originally trained. Essentially, the idea is to give BERT a chance to view words that may be rare in the real world but more common in your domain, so that it can adjust its parameter weights to more closely resemble the domain's distribution before BERT is fine tuned for a specific NLP task.

**In this experiment, I will compare the fine-tuning results from the of two DistilBERT (a lighter BERT variant) models on the<a href="https://huggingface.co/datasets/takala/financial_phrasebank"> Financial Phrasebank dataset</a>: one vanilla DistilBERT and one DistilBERT that has been further pretrained on financial news. The comparison will help to determine whether domain-specific pretraining improves sentiment classification performance.**

## Pre-train Processes:

### Pre-train Dataset
The data we'll be using for pre-training is a combination of two Huggingface datasets: the Reuters financial news dataset and the Financial News dataset. We can visualize the data here:

<div class='tableauPlaceholder' id='viz1752036764024' style='position: relative'>
    <noscript>
        <a href='#'>
            <img alt='Dashboard 1' src='https://public.tableau.com/static/images/Bo/Book1_17515796415940/Dashboard1/1_rss.png' style='border: none' />
        </a>
    </noscript>
    <object class='tableauViz' style='display:none;'>
        <param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' />
        <param name='embed_code_version' value='3' />
        <param name='site_root' value='' />
        <param name='name' value='Book1_17515796415940&#47;Dashboard1' />
        <param name='tabs' value='no' />
        <param name='toolbar' value='yes' />
        <param name='static_image' value='https://public.tableau.com/static/images/Bo/Book1_17515796415940/Dashboard1/1.png' />
        <param name='animate_transition' value='yes' />
        <param name='display_static_image' value='yes' />
        <param name='display_spinner' value='yes' />
        <param name='display_overlay' value='yes' />
        <param name='display_count' value='yes' />
        <param name='language' value='en-US' />
        <param name='filter' value='publish=yes' />
    </object>
</div>

<script type='text/javascript'>
    var divElement = document.getElementById('viz1752036764024');
    var vizElement = divElement.getElementsByTagName('object')[0];
    if (divElement.offsetWidth > 800) {
        vizElement.style.minWidth = '1366px';
        vizElement.style.maxWidth = '1466px';
        vizElement.style.width = '100%';
        vizElement.style.minHeight = '795px';
        vizElement.style.maxHeight = '895px';
        vizElement.style.height = (divElement.offsetWidth * 0.75) + 'px';
    } else if (divElement.offsetWidth > 500) {
        vizElement.style.width = '100%';
        vizElement.style.height = (divElement.offsetWidth * 0.75) + 'px';
    } else {
        vizElement.style.width = '100%';
        vizElement.style.height = '1527px';
    }
    var scriptElement = document.createElement('script');
    scriptElement.src = 'https://public.tableau.com/javascripts/api/viz_v1.js';
    vizElement.parentNode.insertBefore(scriptElement, vizElement);
</script>


From this dataset, we see that 90% of the articles come from Reuters from 2006-2013.  Unfortunately, this dataset isn't very new and in a real world application, a dataset formed with more recent periods (2021 forward) would be favorable due to the immense impact of COVID-19 and the higher interest rates we've seen in the years following the pandemic. However, for this application the timeframe works just fine.

### Tokenization of Articles
In order to train BERT, we first have to tokenize our dataset for BERT to be able to ingest our text data. Instead of training BERT on the full articles, I'm going to **limit the article length to the first 512 words** of the article. I do this for the following reason:
- **Major compute cost savings.** Training on the full article lengths would result in days long tokenization even with the best compute engines. Since we care more about simply knowing if further pre-training has an effect, it's okay to truncate all articles to a standard length.
- **Choosing quantity over quality.** It can be argued, and I think rightfully so, that training DistilBERT (more to come on DistilBERT) on fewer high quality articles would result in better fine-tuning results. Implementing this approach would require additional preprocessing including advanced text parsing, content classification, and possibly manual review to identify which articles to retain. Given the uncertain benefits and the time commitment required to curate such a dataset, I choose to prioritize the larger dataset for this analysis.

Using the Hugging face library, tokenizing the dataset is easy and using Google Colab CPUs, tokenizing is fast and completed in about 4 minutes. I've tokenized the dataset and uploaded 3 tokenized versions:
- <a href="https://huggingface.co/datasets/Czunzun/Financial_news_first_512">First 512 words</a>
- <a href="https://huggingface.co/datasets/Czunzun/Financial_news_middle_512">Middle 512 words</a>
- <a href="https://huggingface.co/datasets/Czunzun/Financial_news_first_4096">First 4096 words (specifically for a LongformBERT) </a>

#### Cost:
Here's the associated time it took to tokenize each dataset using a L4 GPU from Google Colab:
- Financial News First 512 words: 5:52 mins
- Financial News Middle 512 words: 6:04 mins
- Financial News First 4096 words: 0:03 mins
Total Compute Time: 11:59 mins
Total Cost (Price * (Training Time / Total Compute Time Available)): $.042
*As of 07/08/2025 Google Colab Pro is $10 monthly for 100 compute credits and a L4 GPU uses about 2.09 compute units per hour*
### Pre-training BERT:
Now that our dataset is tokenized, we can train our model using MLM. MLM works by applying a mask on randomly selected tokens and having the model predict the value of that token. This is the core principle behind training BERT and was the training method used to train BERT in the paper that introduced it, Attention Is All You Need. I then apply the mask to 15% of the tokens, like  in the Financial News First 512 dataset and perform a 95% train-test split for training. 

You may have noticed in a previous section I said I trained a DistilBERT model instead of a BERT model. Now that we're in the training section I'll explain why I made this choice:

- **Pre-training a BERT model is computationally expensive** given our dataset size of over 200 million words. DistilBERT solves this problem by reducing BERTs size by 40% and retaining 97% of BERT's language understanding capabilities.

#### Cost:
By optimizing the training arguments for speed using Huggingface's Training Arguments method and Google Colab's TPUs, the pre-training took about 18 minutes at a cost of $.12 (Price * (Training Time / Total Compute Time Available)) 

*As of 07/08/2025 Google Colab Pro is $10 monthly for 100 compute credits and a ve5-1 TPU uses about 4.05 compute units per hour*

After pre-training, I uploaded the model to Huggingface for storage. <a href="https://huggingface.co/Czunzun/finbert2_v2">You can access it here! </a>
## Results
Now that our model is pre-trained, its time to fine-tune the models. Given the size of the Financial Phrasebank dataset, optimizing training arguments for speed won't be necessary. Therefore, we can keep the number of epochs at 3 and use the Adam optimizer. 

#### Loss Metrics:
The loss metrics we'll use to evaluate model performance are:
- Accuracy (0-1 range): Did the model correctly predict the label?
- Cross-Entropy Loss (0-1 range): How confident was the model in it's predictions when it was incorrect?
- F1 Score (0-1 range): What is the models predictive power?

Here are the results:

| **Metrics**            | **DistilBERT** | **FinBERTFast** | Difference  |
| :--------------------- | :------------: | :-------------: | ----------- |
| **Accuracy**           |     84.02%     |     85.98%      | **+**1.96%  |
| **F1 Score**           |     80.83%     |     84.24%      | **+**3.41%  |
| **Cross-Entropy Loss** |     82.37%     |     69.66%      | **-**12.71% |

From the results, we see improvements across the board for our specialized model, FinBERTFast. The metric that gives me the most confidence in FinBERTFast is the increase in F1 Score since the increase is not so high that would suggest the model is overfitting, but suggests the model is performing better across all classes.

One problem with FinBERTFast is that it is still overconfident in its predictions even when it's wrong. This is especially dangerous because its accuracy is not high enough to allow inaccurate predictions to be written off as one off mistakes, but the accuracy is also not low enough to discard the model.

## Conclusion:
The results prove that domain-specific pre-training delivers measurable performance improvements at minimal computational cost.

Still, I think the following questions are worth asking:
- Did model performance improve because the model ingested more data or because the data was domain related?
- How does further pre-training affect models with more parameters that have been pre-trained on more data?
- Will continuing to add more data improve the results of FinBERTFast?
- Why is FinBERTFast so overconfident in its predictions?

For a definitive answer to whether or not further pre-training improves model performance, we need to increase the number of parameters in our model and follow the same process above.

That leaves an exciting next step for this project!

For now, we've found a way to improve the results of DistilBERT for our needs!

Long live DistilBERTFast!
