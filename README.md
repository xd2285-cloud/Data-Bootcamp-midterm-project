## Data-Bootcamp-midterm-project
##Introduction

In this project we aim to identify promising restaurant segments in Manhattan, where "promising" refers to stronger consumer appeal and more stable operational performance rather than guaranteed profitability, and ‘restaurant segments’ refers to separating restaurants by cuisine type. 

Our analysis mainly relies on two datasets. The first is collected from the Yelp API which covers a large number of restaurants’ information, capturing customer-facing metrics such as ratings, review counts, cuisine type, price level, and service options. 

The second is from NYC Open Data's DOHMH Restaurant Inspection Results, which contain official health inspection records, including scores, violation counts, and letter grades.
These two datasets approach the same landscape from opposite angles — one reflecting how customers perceive a restaurant, the other representing how regulators evaluate it. 


##Data Acquirement and Cleaning


For the first data source, because Yelp’s search API can only return a limited and somewhat popularity-biased set of results, we did not rely on a single search term.

Instead, we switched our searching terms in our API from simply ‘restaurant’ to multiple terms such as specific cuisines, pizza, coffee, brunch, bars, and bakeries, then combined the results and removed duplicates by business ID. This gave us a broader coverage sample rather than a narrow list of only the most popular restaurants.

Then to avoid the raw dataframe being too messy, we extract all the valuable ones into a new dataframe, and convert some of the important objects into numerical variables (for example, we convert the price sign $ to numeric level).

After cleaning and keeping Manhattan ZIP codes, our main Yelp analysis used 1,868 restaurants with more than enough mixed numeric and categorical features for the project requirements.

Furthermore, when checking missing values, we found that only the ‘price level’ column contains 980 missings out of a total of 1879 samples, with all the other information complete, so we choose to only drop the missing price information when we dig into the price analysis.

For the NYC open dataset, we dropped clearly unusable rows such as cuisine_description, and then aggregated violations from the same restaurant into one row, reducing data to the latest inspection at the restaurant level. After this aggregation step, we had 9,308 restaurant-level inspection records.


##EDA Analysis



We first research the Yelp dataset, using the ‘rating’ feature as our indicator of promising and success. 
Our analysis started with an overview of  Manhattan’s overall market structure. 
The Yelp rating distribution shows that most Manhattan restaurants are already concentrated between 4.0 and 4.5 stars, which suggests that Manhattan is a highly competitive market. This matters because it means small differences in average rating may still be meaningful. 
We also examined review_count distribution and found that it is highly skewed: a small number of restaurants receive a very large amount of attention, while many others operate with much lower visibility. This led us to use a log-transformed review count in some parts of the analysis.


Next, we move to our topic question regarding the identification of promising restaurant segments in Manhattan.
The bar chart above shows the average Yelp rating for each cuisine category, ranked from highest to lowest across all Manhattan restaurants in the dataset. From the chart we can conclude the relative-strong-performing groups including Seafood, Sushi Bars, Ramen, Korean, Thai, Japanese, and Italian, while Dim Sum and American cuisines sit at the lower end near 4.0.
Considering that average ratings alone may be misleading as two cuisines with the same mean may differ greatly in consistency, we also looked at boxplots and stacked rating composition charts which show the restaurants’ full rating distribution, including the median, and interquartile range for each cuisine. From the deeper analysis, we find that Italian stands out as one of the most convincing strong segments because it combines a high average rating with a large sample size. Korean, and Sushi Bars also stand out with relatively narrow interquartile ranges, meaning their high performance is stable and predictable rather than driven by a few exceptional outliers.
In contrast, cuisines like Mediterranean and Chinese show wider variance, suggesting more uneven quality across establishments. If we look from an investor’s view, we would think that a narrow box and high median is a more reliable signal than a high average alone which indicates that success in that cuisine category is more reproducible, not just occasionally achievable. So taken together, we suggest that  Italian, Sushi Bars, and Korean cuisines represent the strongest combination of high ratings and consistent performance in Manhattan's market from the current resources.
We then moved to location, using ZIP code to represent local market environments. In terms of restaurant count, supply is especially concentrated in ZIP codes such as 10019, 10003, 10016, 10001, and 10036. These areas look like core restaurant markets with dense competition and more consumers. At the same time, the highest average ratings do not always appear in the largest restaurant clusters. ZIP codes such as 10029, 10009, 10002, and 10014 show especially strong average ratings, which suggests that some neighborhoods may offer more specialized or differentiated local opportunities. 
This was an important finding for our project because it shows that “promising” can mean different things: either entering a large, mature market with proven demand, or targeting a smaller local market where standout concepts may perform especially well. In the later extra research problem part, our ZIP code–by–cuisine heatmaps also extended this idea by showing which cuisines are strong in which ZIP codes and where there may be local cuisine gaps.
We also explored supporting factors such as price and review count. But the price level, delivery, reservation and pickup is almost unrelative to the performance of restaurants based on our analysis.


After that, we rely on NYC Open Data to analyze which cuisine type has better inspection performance, using the key target ‘inspection score’, where a lower score indicates better inspection performance. We think this part is important because a restaurant or cuisine type that looks attractive on Yelp may still be harder to execute consistently in practice.
The inspection results show that a large majority of Manhattan establishments receive grade A, which means most restaurants meet acceptable standards overall. Still, inspection scores and critical violations vary across cuisines. 
The bar chart above ranks all cuisine types by their average inspection score. Simple, lower-complexity food formats perform better: Hot dogs/Pretzels, Donuts, Salads, Cafes, Bakeries, and similar drink/snack formats, likely reflecting their simpler preparation processes and lower food safety risks.
Among sit-down dining cuisines that are also strong performers on Yelp, French and Italian both average around 13–14, sitting just at or within the A-grade threshold. In contrast, Chinese, Indian, Thai, African and Fusion cuisines show a relatively greater execution complexity.
The boxplot provides a closer look at the distributions of scores across the top 10 most common cuisines. For example, Chinese stands out with both a high median and a wide interquartile range, indicating not just poor average performance but high inconsistency across establishments. French, Italian, Bakery Products/Desserts show tighter distributions, and narrow boxes, suggesting more reliable hygiene standards and operational stability.
It’s noticeable that the most common violation categories include sanitation and equipment maintenance, pest control, temperature control, and cross-contamination and food safety procedures.
This conclusion became our important finding from the project: the “promising” restaurant segment may vary depending on different criterions, such as the goal is lower-risk operational consistency or higher-upside consumer demand.

##Extra Research Question Analysis
To increase the depth of our research, we also think of another four questions that we’re interested in
#Correlation Analysis

To get a brief idea of how Yelp variables correlate with each other, we create a correlation matrix, which shows that all factors have only weak correlations with ‘rating’, ranging from -0.13 to 0.10. However, the limitation is that the matrix only captures linear relationships; thus, we cannot directly conclude that ‘rating’ is irrelevant to other factors.

#Zip Code with Success (potential cuisine gap)


By combining restaurant numbers, average ratings, and average review counts, we identified the top 5 Zipcodes in Manhattan for further cuisine analysis: 10002, 10007, 10009, 10014, and 10029. 
Then we generate two heatmaps above which examine how the spread of restaurants across Manhattan's top 5 ZIP codes and how ratings vary across these locations. The restaurant count heatmap reveals that supply is highly uneven across both ZIP codes and cuisine types. ZIP code 10014 (West Village/Chelsea) shows the broadest coverage, with strong presence across Italian, Pizza, and several other categories. ZIP code 10002 (Lower East Side/Chinatown) is notable for its concentration of Chinese bakeries and Korean bakeries.Meanwhile, 10007 appears sparsely populated across almost all cuisines, suggesting it is not a primary dining destination in our dataset. 
The rating heatmap adds an important layer — high restaurant counts do not always correspond to high ratings. Japanese restaurants in 10009 achieved an exceptional average of 4.8 despite having only 2 establishments, while Sushi Bars in 10014 averaged 4.7 with 4 restaurants. Desserts and Izakaya in 10002 both average 4.6, yet appear only once or twice in the count heatmap. This divergence between density and quality points to potential market gaps — cuisines that are both highly rated and underrepresented in a given ZIP code may represent untapped investment opportunities.

#Better Cuisine within Same Price Level

To further refine the investment lens, we try to break down the top-performing cuisines within each price tier, researching which cuisine types achieve the highest ratings within the same price level.
At Price Level 1, we find Cafes lead with an average rating of 4.45, followed by Bakeries and Coffee & Tea,  suggesting that low-cost, low-complexity food concepts can still achieve strong consumer satisfaction when executed well. 
At Price Level 2, the top performers shift toward more specialized cuisines: Vietnamese, Ramen, and Noodles dominate, indicating that affordable ethnic specialty dining resonates strongly with Manhattan consumers. Notably, the ratings across all five cuisines at this tier are tightly clustered between 4.1 and 4.2, suggesting strong but relatively even competition. 
Price Level 3 treats Thai, Bars, and Korean emerge as top performers, all averaging above 4.3 — a meaningful threshold given the compressed rating landscape established earlier. 
At the highest tier, Price Level 4, only four cuisines appear with sufficient data: Japanese, Sushi Bars, Korean, and Italian, all averaging between 4.2 and 4.45. The consistency of Korean and Italian across both Price Level 3 and 4 further reinforces our previous conclusion of their status as reliable high-performers regardless of price positioning.
Taken together, these charts suggest that the most resilient cuisine choices — those that perform well across multiple price tiers — are Korean and Italian. At the same time, Ramen and Cafes represent strong opportunities within their respective budget segments.

#Common Dishes in Highly Rated Cuisines


We think another useful extension of this project was our analysis of common dish patterns in highly rated cuisines using TheMealDB API. This was not the core of the project, but we came up with this idea when thinking of the menus under these "promising" cuisine types. So we insert another API and search for the dishes that appear in a high frequency within the top five-rated cuisine types.
For example, Thai dishes repeatedly centered around curry, noodles, soups, and stir-fry items. Japanese dishes highlighted sushi, teriyaki, katsu, and udon-style meals. Italian dish terms often emphasized pasta-related items such as spaghetti, alfredo, ricotta, and linguine. Mexican patterns were tied to tacos, enchiladas, and fajitas. This supports the idea that strong cuisines are not just abstract labels; they are often built around recognizable signature dishes that shape consumer expectations and demand.

##Summary

Overall, our final conclusion is that there is no single universally “best” restaurant segment in Manhattan. Instead, the answer depends on what we mean by promising. If we care more about safer and more stable segments, then coffee shops, cafes, bakeries, and lighter-service concepts look especially attractive because they combine decent consumer appeal with relatively manageable operational risk. If we care more about higher upside and stronger consumer excitement, then Italian, Japanese/Sushi Bars, Korean, Ramen, and Thai look more compelling, especially in strong local markets. Among them, Italian is probably the most balanced example of a high-performing cuisine with a convincing sample size.
At the same time, we admit that our project still has several limitations. First, the Yelp sample is a coverage sample, not a complete census of every restaurant in Manhattan, because API search results depend on Yelp’s search system. Second, Yelp ratings measure customer perception rather than revenue, margin, or profitability. Third, the Yelp and inspection datasets were mainly compared at the segment level rather than matched one-to-one for the exact same restaurants, so the results should be interpreted as directional rather than causal. Finally, our dish-pattern extension depends on TheMealDB, which has limited cuisine coverage, so we think the results should only be used as an analysis support instead of exact conclusion.




