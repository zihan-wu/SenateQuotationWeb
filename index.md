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
  <img src="figures/Sanders_said.png" />
</p>

(Note: Since Quotebank may extract different numbers of quotations each month, we weigh quotations in each month by the inverse of the total number of quotes of that month contained in Quotebank. Detail can be found in jupyter notebook of github repository.)

## Total quotation is positively correlated to results.

We can also calculate each candidate's total quotations before the election and compare the number of total quotations to election results. For both types of the quotations, we use scatter plots to show how total quotation numbers and vote rates are related.

<p align="center">
  <img src="figures/Sanders_said.png" />
</p>

We further examine the correlation for both types of the quotations by calculating the **Pearson Correlation Coefficient**. 

1. For the quotations said by the senate candidates, we have Pearson Correlation 0.65, with P-Value 7.6e-23. 
2. For the quotations said by the senate candidates, we have Pearson Correlation 0.53, with P-Value 1.1e-22.

**Senates with higher number of quotations before election tend to have higher vote rate!

## Weigh quotes by website view of the source media

While the above finding accords with our intuition (that more quotations indicate more success), purely counting the number of quotations ignores the qualities of different quotations. In fact, a quotation in New York Times means much more fame than a quotation from a local tabloid. Therefore, we weigh each quotation by the website views of the source media.

QuoteBank, however, only provides the url where each quote is extracted. So, we first extract the main webdomain of the urls from 50 thousand quotations, and examine distribution of source media. 

<p align="center">
  <img src="figures/Sanders_said.png" />
</p>

As we can see from the histogram, the distribution is heavy tailed, with most quotations from a few media. We also notice that the 50 thousand quotations have over 4000 different media sources, inidcating that the set of quotations from the QuoteBank is an efficient representation of all quotations in news articles.

Since it is difficult to automatically find the website view data of all media source, we decide to manually search the website view statistics of the top 30 frequently appearing media. Among the rest media, we manually search the website view statistics of 10 randomly selected media, and then we use the median value to weigh the quotations of the rest media. Below, we show the website view of top 10 frequently appearing media in Quotebank.

<p align="center">
  <img src="figures/Sanders_said.png" />
</p>

Now, we can look at the weighted quotations said by Bernie Sanders:

<p align="center">
  <img src="figures/Sanders_said.png" />
</p>

We can also calculate the correlation between weighted total quotations (before election) and vote rate:

1. For the quotations said by the senate candidates, we have Pearson Correlation 0.65, with P-Value 1.2e-22. 
2. For the quotations said by the senate candidates, we have Pearson Correlation 0.59, with P-Value 5.0e-29.

The result is very close to the correlations between number of quotations and vote rate. **Senates with higher number of weighted quotations before election also tend to have higher vote rate!

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
