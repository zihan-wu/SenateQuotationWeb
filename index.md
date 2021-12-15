# Can Quotations of Politicians Indicate Election Results?

**Github Source:** https://github.com/epfl-ada/ada-2021-project-chilldatagroup.git

## Introduction:

In general, more quotations in news articles indicate more popularity or at least more exposure to the public. Quotations could thereby be vital for the successful careers of politicians. On the other hand, successful politicians are also more frequently quoted in news. **Thus, the career of politicians may be highly correlated with quotations.** The recent release of QuoteBank, a corpus of quotations from news, can be useful in extracting the quotations of politicians. 

In this project, we will focus on the U.S. Senate elections. First, combining the quotations and election results, we want to answer whether more quoted senate election candidates tend to have better election results. Then, we aim to examine if quotations can be an efficient predictor of election results. Furthermore, quotations can either praise or criticize the politicians. Recent advancements in language sentiment analysis enable us to get attitudes toward quoted candidates. We will explore how such attitudes correlated with election results.

## Extract candidate quotes:

We get the senate election result by MIT Election Data and Science Lab[1]. It contains the election information of each candidate. We use the dataset to get the identity, party affiliation, and resulting votes of each candidate. We select the candidates participating in **U.S. Senate elections since 2016**. In QuoteBank corpus, we find quotations **said by** these candidates in news articles **from 2015 to 2020**. By aggregating the quotations based on candidates and dates, we may look at how the number of quotations said by a slected candidate changes over time. Let's look at an example from Bernie Sanders:

<p align="center">
  <img src="figures/Sanders_said.png" />
</p>

On the other hand, there is another type of quotation: quotations **mentioning the candidates** but not necessarily said by themselves. In QuoteBank corpus, we can also find quotations **mentioning** the Senate election candidates in news articles from 2015 to 2020. By aggregating the quotations based on candidates and dates again, we can look at how the number of quotations mentioning a slected candidate changes over time. Let's look at Bernie Sanders from this perspective:

<p align="center">
  <img src="figures/Sanders_mention.png" />
</p>

(Note: Since Quotebank may extract different numbers of quotations each month, we weigh quotations in each month by the inverse of the total number of quotes of that month contained in Quotebank. Detail can be found in jupyter notebook of github repository.)

## Total quotation is positively correlated to results.

We can also calculate each candidate's total quotations before the election and compare the number of total quotations to election results. For both types of the quotations, we use scatter plots to show how total quotation numbers and vote rates are related.

<p align="center">
  <img src="figures/quote_vs_vote.png" />
</p>

We further examine the correlation for both types of the quotations by calculating the **Pearson Correlation Coefficient**. 

1. For the quotations said by the senate candidates, we have Pearson Correlation 0.65, with P-Value 7.6e-23. 
2. For the quotations said by the senate candidates, we have Pearson Correlation 0.53, with P-Value 1.1e-22.

**Senates with higher number of quotations before election tend to have higher vote rate!**

## Weigh quotes by website view of the source media

While the above finding accords with our intuition (that more quotations indicate more success), purely counting the number of quotations ignores the qualities of different quotations. In fact, a quotation in New York Times means much more fame than a quotation from a local tabloid. Therefore, we weigh each quotation by the website views of the source media.

QuoteBank, however, only provides the url where each quote is extracted. So, we first extract the main webdomain of the urls from 50 thousand quotations, and examine distribution of source media. 

<p align="center">
  <img src="figures/media_hist.png" />
</p>

As we can see from the histogram, the distribution is heavy tailed, with most quotations from a few media. We also notice that the **50 thousand quotations have over 4000 different media sources**, inidcating that the set of quotations from the QuoteBank is an efficient representation of all quotations in news articles.

Since it is difficult to automatically find the website view data of all media source, we decide to manually search the website view statistics of the **top 30 frequently appearing media**. Among the rest media, we manually search the website view statistics of **10 randomly selected media**, and then we use the **median value** (which is more robust to outlier) to weigh the quotations of the rest media. Below, we show the website view of top 10 frequently appearing media in Quotebank.


<p align="center">
  <img src="figures/media_df.png" width="500"/>
</p>

Now, we can look at the weighted quotations said by Bernie Sanders:

<p align="center">
  <img src="figures/Sanders_said_weighted.png" />
</p>

We can also calculate the correlation between weighted total quotations (before election) and vote rate:

1. For the quotations said by the senate candidates, we have Pearson Correlation 0.65, with P-Value 1.2e-22. 
2. For the quotations said by the senate candidates, we have Pearson Correlation 0.59, with P-Value 3.4e-29.

The result is very close to the correlations between number of quotations and vote rate. **Senates with higher number of weighted quotations before election also tend to have higher vote rate!**

## Different exposure-gaining process for different politicians.

Different politicians gain public exposure in different ways. Some politicians may gain popularity through years of accumulating fame, while some others may gain overnight fame through some accidents or sponsorships from other powerful people. As a result, if we look at **the curves of how weighted quotations change before election**, we will probably see different shapes. We examine how the weighted quotations of each politician change over time, and whether there are different types of exposure-gaining process.

To do this, we extract the weighted quotations said by each candidate **within 300 days** (roughly a year) before the election day. We then accumulate the weighted quotations **by month**, so that we get a 10 dimensional feature vector per candidate. This vector reflects the how weighted quotations said by each candidate change over time. The vectors are **L2 normalized**, so that the euclidean distance between them reflects the cosine similarity, which better measures the distance between different shapes of exposure-gaining process. We then apply **KMeans clustering** to the feature vector (K=2 has the lowest silhouette score). To better visualize the cluster, we use PCA to lower the dimension to 2 and label cluster with different colors:

<p align="center">
  <img src="figures/pca_cluster.png" />
</p>

The method appears to be efficient at separating the clusters. We then look at the centroid of the two clusters and see two types of exposure gaining: some politicians have **one high peak** of quotations just before the election (type 0), while others have **several peaks** before the election (type 1).

<p align="center">
  <img src="figures/2_types_gain.png"  width="800"/>
</p>

For each type, we look at one example: Dianne Feinstein for type 0 and Allen Buckley for type 1.

<p align="center">
  <img src="figures/2_types_gain_sample.png"  width="800"/>
</p>

## Quotations can predict election result!

We further explore if the election results can be predicted from the quotation data. We use the **monthly aggregated quotation feature vectors** used in clustering analysis above. Besides, the party affiliation can have significant effect on the result. For senate candidates in each state's election, we find the **vote rate of their affiliated party** in previous presidential election[2]. This party vote rate reflects the overall ideology of the state. We concatenate this vote feature to the quotation feature vectors and use this 11-dimensional vector for predicting the election result.

**TO DO: add results**

## Sentiment of quotations cannot reflect support rate.

For quotations mentioning senate candidates, they contain attitudes (positive or negative) of the speaker toward the mentioned politicians. We can use **VADER** sentiment analysis package to extract such attitudes. For instance, let's look at how sentiments toward Bernie Sanders changes over time.

<p align="center">
  <img src="figures/sanders_senti.png" />
</p>

To examine whether sentiments in quotations can represent support rate, we can calculate the **average sentiment score of quotations** mentioning each candidate **within a year before the election**. This can be visualized by scatter plots of average sentiment score and final vote rate:

<p align="center">
  <img src="figures/senti_vs_vote.png" />
</p>

The Pearson Correlation is -0.11, with P-Value 0.09 (> 0.05). As a result, there is no significant correlation between them. **Sentiment of quotations toward politicians cannnot truely reflect the support rate!** This may be counter-intuitive. However, in 2016 election, Donald Trump won the presidential election while most news agencies did not favor him. Nowadays, news articles fail to convey the ideas of most people. 

## Reference:

[1]MIT Election Data and Science Lab, 2017, "U.S. Senate 1976–2020", https://doi.org/10.7910/DVN/PEJ5QU, Harvard Dataverse, V5.

[2]MIT Election Data and Science Lab, 2017, "U.S. President 1976–2020", https://doi.org/10.7910/DVN/42MVDX, Harvard Dataverse, V6.


You can use the [editor on GitHub](https://github.com/zihan-wu/SenateQuotationWeb/edit/gh-pages/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.







### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [Basic writing and formatting syntax](https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/zihan-wu/SenateQuotationWeb/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
