# Connecting with API Blog

I wrote a vignette to explain how I collect and perform basic EDA on stock data from Polygon.io API. In order to track my work I created a github repository and connected it with RStudio. 

## My Work

Process

There are two main sessions in the vignette.  In the first session, I built a few user-friendly functions to interact with the Polygon API to query, parse, and return well-formatted data. These functions all allow users to customize their query to return specific data. Users can query ticker types, ticker symbols, specific historical stock data and grouped daily data from Polygon.io through these defined functions. Alternative methods like wrapper function and parallel computing process are also provided.

In the second session, I performed a basic exploratory data analysis (EDA) on historical stock data for companies – Apple, Google, Tesla, and Zoom. I analyzed their close price, returns, and moving prices to find some interesting trends and relationships. Contingency tables, descriptive statistics, plots are used to show the trends, patterns, and characters of the data.  

Interesting Findings

Since this is my first project on financial data, the really different results from analyzing close price, returns, and moving price surprised me a lot. I’d say the most interesting finding is that these four tickers’ returns have pairwise positive correlation. And one important thing is that I know all these four tickers are going down in 2022. Now if someone talks to me about stock I won’t be completely gullible. 

## Reflection

Most difficult part

The most difficult or time-consuming part for me was to be familiar with the basic financial concepts and methods used for time series analysis. I spent a lot of time learning financial data analysis. Also constructing the URL to query data and define associate functions took me some time.

 In the future

In the future I won’t define the function at the very beginning, I will do this after checking if my logic works for one specific argument.  When I tested one of my defined functions, an error was returned because of null results gained from API. Then I had to debug my function code and found it was not about my logic. If I tried the logic first, I would be more confident about it and try to find other possible causes directly. 

In the future if I work on a similar project, I also want to give the package quantmod (Quantitative Financial Modeling Framework) a shot. 

[Link to my github pages](https://fang2403.github.io/Vignette-for-reading-and-summarizing-data-from-API/)

[Link to my github repo](https://github.com/Fang2403/Vignette-for-reading-and-summarizing-data-from-API)
