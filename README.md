# WeRateDogs-Twitter-Account-Dog-Rating-Analysis

## Data 
We gathered three separate tables at first. 
1. `archive` includes tweet ID, tweet time and the content of the tweet, ...etc. information.
2. `img_df` includes the dog type prediction results and probabilities of each dog picture in the Tweet.
3. `tweet_df` includes Tweet info queried from the Tweeter API includes retweet numbers, like numbers and length of the Tweet

## Wrangling
In the wrangling process, I listed some issues in these dataframes and fixed them.
#### Quality

`archive` table
    1. Timestamp is a datetime not an object
    2. Missing "in_reply_to_status_id", "in_reply_to_user_id", "retweeted_status_id",   "retweeted_status_user_id", "retweeted_status_timestamp" and "expanded_urls". (can't fix yet)
    3. Tweet_id 835246439529840640 rating_denominator = 0 and rating_numerator = 960. I would assume it's a typo of 96/100 
    4. Since we only want original ratings (no retweets) that have images, so delete those tweets with retweet_id
    5. Some animal names not capitalized
    6. After deleting rows with retweet_id, we can delete "retweeted_status_id", "retweeted_status_user_id" and "retweeted_status_timestamp" since they are useless for the analysis
    7. Some dogs have more than one dog "stage". Ex: tweet_id: 817777686764523521 (can not fix)
`img_df` table
    1. Delete those rows with "p1_dog" == False, since our analysis only want to focus on dogs rating
    
    
    
#### Tidiness
`archive` table
    1. One variable, dog stage, in four columns (doggo, floofer, pupper, puppo)
    2. Overall_rating can be calculated with rating_numerator/rating_denominator, combining two columns into one
    3. Animal breed "p1" from img table can be added to archive table since it's also a part of info of the tweet
    4. tweet_df table can be merged with archive table with tweet_id

## Insights 

1. Dog rating in the WeRateDogs account has no relationship with the popularity, which can be defined with number of retweet and numebr of like, of the Tweet. The correlation coefficient between rating and number of retweet is only 0.015 and the correlation coefficient between rating and number of like is only 0.014. Even if the dog photo is rated with high score by WeRateDogs, the tweet might not be more popular than other dogs with low rating. 

2. 1/3 of the dogs in archive Tweet data have `N` as the first character of their names. There is a trend to name your dog starting with `N` <br>

3. Tweeter is getting more and more popular from 2015 to 2017, so the retweet number and like number are growing dramastically within the three years. <br>

4. Like number is highly related to retweet number. The correlation coefficient is 0.927.

After cleaning issues listed above, I combined the three dataframes into one dataframe `df`. <br>
Though there are some missing data and wrong documented dog stages can not be fixed, we can still use the dataframe to do some analysis.

## Visualizations

1. 700+ Dogs names are starting with `N`. The frequency of having other first character is less than 1/6 of the chance of starting with `N`.
<img src="First Character of Dog Name.png">

2. Retweet number and like number are growing. Tweets posted in 2017 are having more likes and retweets than Tweets posted in previous time, which means this account is being more and more popular and people love to retweet the tweets to share with their friends.
<img src="Time Trend of Retweet and Like Number.png">

3. The average of retweets and likes of dog species. We can see from the plot that the top five dog species that received most likes are `Saluki` , `Flat-coated Retriver` , `French Buildog` , `Black and Tan Coonhound` and `Afghan Hound`. <br>
Among the top five species, though `Saluki` is not the one who has the most retweets, it's still the most popular species that received the most likes. <br>
There is a clear trend that the more retweets a tweet has, the more likes it has.
<img src="Number of Retweet and Number of Like Relationship between Dog Species.png">
