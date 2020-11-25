
# NYC Restaurant Yelp and Inspection Analysis


## Purpose

New York City is known as one of the best cities in the world for food. NYC is full of people from all different backgrounds, ethnicities, and regions of the world. Because of this, you can find virtually any type of cuisine you can think of in NYC. This also has lead to the creation of many different fusions and new flavor combinations. In addition to a variety of cuisines, NYC also consists of a range of restaurants, from little street carts to world class Michelin star restaurants. All of these types of restaurants along with the ability to be located in such a populous, food forward city, have potential to be enormously successful businesses. One of the most important pieces to becoming a successful restaurant is driving people through your door and into your restaurant for a meal so that you can increase your revenue. In today's technology forward world, many NYC locals and visitors frequently turn to the internet for information about restaurants they may want to eat at. This information has the potential to greatly help or hurt each restaurant depending on the positive or negative information available. Yelp, a website where users can write reviews and ratings for different businesses (including restaurants), is often a starting research point for many as it allows consumers to get an idea of other consumers' experience at each restaurant. Having a high Yelp rating is likely to draw in consumers, while a low rating could scare consumers away.  Another important aspect that many consumers consider prior to eating at a restaurant is what inspection grade the restaurant has been given. Receiving a 'B' or 'C' grade could indicate that significant sanitary violations were found at the restaurant, which likely will deter many from eating there. 

The goal of this analysis is to dive into NYC restaurant Yelp and inspection data, most specifically in Manhattan, in order to determine if any particular aspects of a restaurant can help influence the likely inspection grade, Yelp rating, and amount of Yelp reviews a restaurant receives. This analysis will be most helpful for people in or interested in joining the restaurant world in NYC as it can help them identify certain aspects (i.e. cuisine type, location, price level, etc.) that typically lead to higher Yelp ratings or inspection grades, and therefore should be optimized, or lower Yelp ratings or inspection grades, and therefore may want to be avoided.


## Data Science Process Used

I leveraged the OSEMiN (Obtain, Scrub, Explore, Model, Interpret) process for this project. My notebooks are organized to follow this process. Parts 1-3 (Obtain, Scrub, Explore) can be found in notebook #1, while parts 4 + 5 (model, interpret) can found in notebooks #2 and #3 for each analysis done.

## Part 1: Obtaining The Data

I collected two different datasets that I then merged together into one combined dataset. For our modelling, we are working with 3,930 different restaurants. The datasets are:

**1. Yelp data:**
   - Collected from the Yelp website using the Yelp API on 8/25/20: https://api.yelp.com/v3/businesses/search
   - Contains information and rating/review details about restaurants from Manhattan. Data on 1,000 restaurants from each Manhattan neighborhood was collected. The list of Manhattan neighborhoods used can be found here:  https://www.nyctourist.com/million-manhattan.htm

**2. Restaurant Inspection data:**
   - Provides information about restaurant inspections for all NYC restaurants. The data was collected from the NYC Open Data website on 8/25/20: https://data.cityofnewyork.us/Health/DOHMH-New-York-City-Restaurant-Inspection-Results/43nn-pn8j
   - This dataset includes restaurants from all 5 NYC buroughs, but for our main analyses we are just leveraging data for Manhattan restaurants. 
   

## Part 2: Scrubbing the Data

Six different steps were taken to scrub the data and get it ready for analayzing.

1. Remove duplicate data - some restaurants were listed multiple times in the dataset. Since we only want to look at one entry per restaurant, duplicative rows were removed.
2. Remove unnecessary columns - any columns that were not going to be relevant to our analysis were removed.
3. Feature Engineering - in the Yelp dataset, a few columns (i.e. categories, coordinates, location) consisted of data in dictionary format. Therefore, I extracted these values so that they woould be easeier to work with. Additionally, I used the frequency of each cuisine type to identify which restaurants are mainstream cuisines (cuisine appeared at least 100 times in the dataset) and which are rare cuisines. The PHONE column in the inspection dataset was manipulated to match the format of the display_phone column in the Yelp datset so that this column could be used to merge the datasets later on.
4. Dealing with categorical data - In order to run our analysis, we will need our data to be numerical. Therefore, we used dummy variables to turn the cuisine type, transactions, GRADE, neighborhood, critical_flag, count_range, and price_value columns into numerical data columns.
5. Dealing with missing data - any missing price values were updated to be zero to represent the price level is unknown. The ACTION, CRITICAL FLAG, INSPECTION TYPE, GRADE, SCORE, and GRADE DATE columns were manipulated so that any null values were replaced with values based on indications from the dataset creaters. All other rows with null values were removed.
6. Dealing with outliers - restaurants with a very large number of reviews (greater than 1,000) were updated to say they have 1,000 views to remove outliers but still be able to use the data. Restaurants with a large number of inspections (greater than 70) had their number of inspection visits value replaced with '70' so that outliers were no longer present but the data could still be used and would still indicate a large number of inspections for each particular restaurant updated.


## Part 3: EDA

A variety of different aspects were investigated using EDA. The main focuses were on the Borough (only for the inspection data), the cuisine type, the Manhattan neighborhood, the inspection grade, and the Yelp ratings. Below are a few of the charts created to explore the data.

**Inspection Grade By Borough:** Manhattan has the greatest number of restaurants, and all buroughs have the majority of their restaurants receiving an 'A' grade.
<br>![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Number%20Restaurants%20Per%20Borough.png)
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Inspection%20Grade%20By%20Borough.png)<br>

**Number Of Yelp Reviews Per Cuisine Type:** Korean and Seafood restaurants have the grest number of Yelp reviews on average.
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Number%20Reviews%20Per%20Cuisine.png)

**% Of Yelp Rating Per Cuisine Type:** The majority of cuisines have mostly 4.0 ratings, followed by 3.5. In particular, Korean, Seafood, Pizza/Italian, Japanese, and Asian have the most 4.0 ratings. Cafe/Coffee/Tea and Other appear to have the most 5.0 ratings. Mexican, Pizza, Chinese, and Latin appear to have the most lower Yelp ratings.
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/%25%20Rating%20Per%20Cuisine.png)

**Distribution Of Yelp Rating Per Neighborhood:** On average, many neighborhoods have a similar spread of Yelp ratings, though Morningside Heights has the worst ratings. 
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Yelp%20Review%20Per%20Neighborhood.png)

**% Of Inspection Grade Per Neighborhood:** All of the neighborhoods have a majority of 'A' inspection grades. It looks like Little Italy has the greatest percentage of 'B' grades and Hell's Kitchen has the greatest percentage of 'C'.
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/%25%20Grade%20Per%20Neighborhood.png)

**Inspection Grade By Price Level:** The cheaper restaurants (1 and 2 Dollar Sign) have the greatest percent of B and C grades, while the most expensive restaurants (4 dollar signs) has the greatest percent of A grades and least percent of B and C grades. Therefore, as price rises, so do the inspection grades.
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Grade%20Per%20Price%20Level.png)

**Inspection Grade By # Of Yelp Reviews And Yelp Rating:** It looks like the greatest number of reviews are typically given to restaurants in the 3.5-4.5 range. Few reviews are given for 5 star or 2 and below stars, though this is likely because few restaurants acheived these ratings.  Additionally, restaurants with a 2.5 rating and a 'C' inspection grade seem to comparatively have a lot of reviews. This is likely because consumers had a bad experience at the restaurant and want to share their bad experience with others to warn others about the restaurant. We also see some outliers specifically for restaurants with an 'A' grade, indicating that very positive expereince may help encourage consumers to write reviews. 
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Grade%20By%20Number%20Reviews%20and%20Rating.png)

**Distribution Of Yelp Rating By Price Level:** Restaurants at the highest price level (4 dollar signs) tend to have better Yelp ratings. We also see better Yelp ratings for restaurants with an unknown price level. 
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Price%20Per%20Rating.png)

## Model

Two different analysis were used here, Hypothesis Testing and Machine Learning. Each analysis can be found in its own notebook, ('2. Hypothesis Testing' and '3. Machine Learning(Classification)')

### Part 4: Hypothesis Testing

When doing an analysis, hypothesis testing is often used in order to determine whether or not an outcome is statistically significant. It is important to have a good experimental design in order to ensure that your analysis was run properly and produced trustworthy results. When running a hypothesis test, we must first set our **null and alternative hypotheses.** The null hypothesis is typically that there is no relationship between A and B, with thile alternative hypothesis is your educated guess about the outcome (i.e. A is greater than B). To determine weather we reject or accept our null hypothesis, we look at the relationship between the **p-value**, which is the probability of observing a test statistic at least as large as the one observed, and the **alpha value (𝛼)**, which is the threshold at which we are ok rejecting the null hypothesis. Often, an alpha value of 𝛼=.05 is used, meaning that if our p-value is less than our alpha value of .05 then we can reject the null hypothesis. If we do end up rejecting our null hypothesis, we can then look at the **Effect Size** to determine the difference between the observed groups.

For our analysis, we are using hypothesis testing to evaluate four different questions. We are using an alpha value of .05, meaning that any results with a p-value less than .05 will indicate we should reject the null hypothesis. Below are the four questions along with their outcome:

**1. Does a restaurant's Yelp rating influence how many Yelp reviews the restaurant will receive?**
    <br>--> Reject Null Hypothesis, indicating a higher Yelp rating does lead to having a greate number of Yelp reviews received. However, the effect of this is very small.<br><br>
**2. Does a restaurant's inspection grade influence how many Yelp reviews the restaurant will receive?**
    <br>--> Reject Null Hypothesis, indicating having a higher inspection grade does lead to having a higher number of Yelp reviews received, though the effect size is small.<br><br>
**3. Does the type of cuisine influence how many Yelp reviews the restaurant will receive?**
    <br>--> Reject Null Hypothesis, indicating some cuisine types do impact the number of Yelp reviews received.<br><br>
**4. Is there a relationship between the Inspection Grade and the Neighborhood, Price, or Cuisine Type?**
    <br>--> Reject Null Hypothesis, as some price levels can have an impact on the inspection grade. However, we fail to reject the null hypothesis when looking specifically the impact of the neighborhood or the cuisine type on inspection grade. <br><br>

### Part 5: Machine Learning

Machine Learning is a way to run data analyses by using automated analytical models that have the capability to learn. For this analysis, I have used classification, a type of supervised machine learning that uses labelled data to help predict which group a data point should be classified into. I ran 5 different classification models in order to determine which of the five would be most accurate and therefore should be utilized. Prior to running each model, I first used a grid search function to determine the optimal hyperparameters for each individual model. This step helps ensure we get a more accurate model. The five classification models used are:

1. Decision Tree
2. Randoom Forest
3. Adaboost
4. XGBoost
5. Logistic Regression

As seen below, the Random Forest model was our best model as it had the highest accuracy and F1 score (note: F1 score is a combination of precision and recall).
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Classification%20Results.png)

Within the Random Forest model, here are the most important features:
![alt text](https://github.com/rspiro9/NYC-Restaurant-Yelp-and-Inspection-Analysis/blob/main/Images/Important%20Features.png)


## Conclusions

### Hypothesis Test Conclusions:
Based on these results, there are a few recommendations I would give to current or prospective restaurant owners:
- Consider a 4 dollar sign price level rather than a 2 dollar sign price level as these types of restaurants often receive better inspection grades. 
- Ensure your restaurant is up to code and has minimal violations so that you are more likely to receive a better inspection grade, which likely will lead to a greater number of Yelp reviews, which in turn can draw more customers into your restaurant.
- Ensure customers have an enjoyable experience at your restaurant so that they will not only give a high Yelp rating, but will also leave a positive review which can encourage other potential customers to try your restaurant.
- When trying to ensure a strong inspection grade, cuisine type and neighborhood do not play a significant factor, so no limitations need to be considered in respect to these two aspects.

### Machine Learning Conclusions:
From this analysis, a couple of things I would recommend to current or prospective restaurant owners include:
- If possible, consider opening a restaurant in Midtown West or Greenwich Village as restaurants in these neighborhoods tend to receive higher Yelp ratings and higher ratings can lead to drawing in more customers. Additionally, avoid opening a restaurant in Morningside Heights as these tend to receive lower Yelp ratings.
Avoid receiving a critical violation flag in an inspection as having one of these violations likely leads to lower Yelp ratings, while not having a critical violation flag likely leads to higher Yelp ratings.

## Watchout

One watchout I want to mention is that the data for this analysis was pulled in August 2020, during the Coronavirus pandemic. This pandemic has hit restaurants especially hard, with many restaurants temporarily or permanently closing down. It seems as though Yelp has done a descent job of identifying resturants that are open vs. closed, though I would guess that the data is not 100% accurate as restaurants' statuses were constantly changing during this time. Therefore, the dataset may include some restaurants that are no longer in business, or may have falsely excluded some restaurants that were in business.

## Next Steps and Recommendations
A few things to look into next:
- Does the price impact the number of Yelp reviews?
- Does having a critical violation impact the number of Yelp reviews or the inspection grade?
- Do certain neighborhood have significantly higher Yelp ratings?
- Do certain neighborhoods have significantly higher inspection grades?
- Improve the machine learning model to make it even more accurate. One way to do so would be by testing out additional hyperparameters in addition to the ones we have included here.
- Expand this analysis beyond Manhattan and into the other NYC boroughs to see if the trends seen in Manhattan shift or remain the same in other buroughs.
