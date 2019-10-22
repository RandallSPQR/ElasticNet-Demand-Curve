# ElasticNet Demand Curve
Ecommerce companies suffer from an over abundance of data which comes from different sources, is complied using different methodologies, and rarely tell the same story. In this analysis we focus on a mid-sized ecommerce company that is in the Women's Apparel Space. 

Context - Most ecommerce companies have Seasonal Sales (Black Friday/Cyber Monday) and clearances End of Season, Sample Sales. These sales have a dramatic effect on purchase volumes since prices can drop upto 50% off of MSRP. Further, we can safely assume that women are aware of this practice and adjust their purchase behavior accordingly.
 
This has two implications -
1.     Price and Discount strategies have big impacts on overall profitability. A 1% increase in profitability equates to well over $200K in incremental revenue.
2.     As a company runs media campaigns to build awareness, people will delay their time of purchase to align with a sale cycle. Knowing how a sale will perform given a final price is a key piece of media planning 

## Business Problem 
There are two elements to this problem. 
1.	 How to best account for the effectiveness of media and its KPIs
2.	Can we implement a better price strategy that will increase profitability.

__Approach__ - A Model to Predict Sales (Revenue or Orders) based on Price (List Price, Discount and Transaction Price) and Media Levels. That model can then be used to estimate a demand curve, to understand the impact of an increase in prices on quantity demanded and profit margin. 

Data comes from 4 Sources: 
1. _Google Analytics_ - The worldwide standard for Web Analytics 
    * Last Click Attribution Data by Media Source includes Email, Direct, Organic Social and Search.
    
2. _Google AdWords_ - Google's Advertising products are used by every ecommerce company in the world. s
    * Paid Search, Google Display, and Product Listing Ads (PLA) by Day, By Creative
    
3. _Facebook_ - The Social Media Giant allows brands to reach over 1B People. 
    * Facebook and Instagram Ad Performance Data by Day, By Creative
    
4. _Customer Transaction File_ - This includes actual sales, and data on List Price, Discounts, and Prices Paid. 
    * This is the master file of everyone who purchased, by date, and name.
 
### Stats Model Estimation
This data will be used to estimate a best fit predictive model, which we can then interpret its coefficients to inform the Client of which channels, and which KPIs are the most important. Answering our first Question. 

### Demand Model Estimation
Using that same Best Fit Model, we can use Bootstraps to estimate the demand curve by rerunning the model under different pricing conditions. By Mixing the interplay between MSRP, Discounts, and the final Transaction Price we can understand what is truly driving consumer purchase behavior. 

> H0 - Homo Economicus: People only care about the final 'out-of-pocket' price. They Rationally Perceive Prices.

> H1 - Homo Bargainus: People want to 'get a deal'. EG the utility derived from a 'savings' is greater than their utility from an incremental dollar. EG "I just saved \$100 on this \$500 item"

***
Data Cleaning Steps Taken before import:
1. Facebook
    1. Correct Date dtype to be pandas DateTimes
    2. Df.fillna(0) for all NaN Values. This is the correct value, as Facebook leaves no actions as blanks, but its equivalent to say if no action was taken then 0 actions were taken, this is also how Google presents its data.
    3. Create labels for Target (Audience eg TOF, Mid, Ret)
    4. Create labels for Source (Facebook, Instagram, DPA)
2. Ad Words
    1. Correct Date dtype to be pandas DateTimes
    2. Change Column Names
    3. Create labels for Target (Audience eg TOF, Mid, Ret)
    4. Create labels for Source (Brand, Product, PLA, Display, etc)
3. Analytics
    1. Correct Date dtype to be pandas DateTimes
    2. Change Column Names
    3. Change Source Names
4. Customer File
    1. Drop all PID
    2. Remove Internal Test Rows
