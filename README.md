# IZABEL CAPSTONE PROJECT README

### Big Idea Project :airplane:
Using a machine learning model that recommends content, this project aimed to generate a Vancouver attraction recommendation system based on user input and historical data. This is an opportunity for travellers to get quick attraction suggestions on where to go next without significant research. This is also an opportunity for tourism businesses to understand how customers review their attractions. 

---

## Table of Contents
- [Goals](#goals)
- [Data](#data)
- [Work](#work)
- [Results](#results)
- [Future Direction](#future-direction)

---

<a id="goals"></a>
### Goals :mag:
This project aimed to create a Vancouver attraction recommendation system based on the user's location and preference alongside historical data. The user would input their current location and what attractions interest them. Their current location would be an address. For historical data, the attraction's Google rating, number of reviews, location, and categories will be explored. The attraction's Google rating and number of reviews will help decide what places are recommended from other customer experiences. The attraction's latitude and longitude will help determine how far customers will go from their current location. The attraction category will help dictate where customers will go tailored to their interests. The intended output is the top 5 recommended attractions.   

---

<a id="data"></a>
### Data :books:
Data was collected from Google Map Extractor and Geoapify.

**Google Map Extractor** is a free resource on Git Hub that extracts business profiles from Google Maps. Query the desired data as you do under Google Maps and it will return ~120 Search Results that can be downloaded as a CSV file. 

Interacting with the Google Map Extractor is similar to using the Google Map search engine. I kept all the fields to the default and searched attractions by their type and location.

![image](https://github.com/user-attachments/assets/c1dac5b9-9eb5-4efc-b9d9-a53df01126c3)

The application takes a few minutes to collect and format the data. From the search result, I can scroll down the application to download the CSV file. Each CSV file contains ~100 rows of attractions.  

![image](https://github.com/user-attachments/assets/2a15e079-140c-409c-a855-0ae19da19326)

The CSV file contains 21 columns containing extracted information on attractions that best match the query according to Google Maps. 

|Column Name| Description|
|---|---|
|`place_id`|generated identification made by Google Maps Extractor|
|`name`|name of attraction|
|`description`|details of description as seen on Google Maps|
|`reviews`|number of reviews for attraction|
|`competitors`|similiar attractions by Google Caps|
|`website`|link to attraction website|
|`phone`|phone number of attraction|
|`can_claim`|N/A|
|`owner_name`|owner of attraction|
|`owner_profile_link`|link to owner of attraction|
|`featured_image`|image displayed on google map|
|`main_category`|main category assigned to attraction by Google Maps|
|`categories`|categories assigned to attraction by Google Maps|
|`rating`|average rating of attraction|
|`workday_timing`|operating hours of attraction|
|`is_temporarily_closed`|N/A|
|`closed_on`|closing day(s) of attraction by Google Maps|
|`address`|address of attraction|
|`review_keywords`|frequent words in review for attractions by Google Maps|
|`link`|link to google map about attraction|
|`query`|user input on search engine of Google Maps Extractor|

_NOTE: The free version only allows 20 searches per month. Unlimited searches for a lifetime require upgrading to the Pro Version._

More details of Google Map Extractor: https://github.com/omkarcloud/google-maps-scraper?tab=readme-ov-file

**Geoapify's Online Geocoding Tool** is a free resource that takes a list of addresses and returns more detailed columns about the address such as their longitude and latitude that can be downloaded as a CSV file.

How to Use (from the Official Website):
1. Upload an Excel, CSV, or text file that contains addresses to be geocoded or copy&paste addresses to a text area
2. Map columns to the address components (house number, street, city, etc.)
3. Geocode addresses with Geoapify Geocoder
4. Download the CSV file with the results

_NOTE: The tool only allows a maximum of 500 rows. Larger datasets need to be split._

More details of Geoapify's Online Geocoding Tool: https://www.geoapify.com/tools/geocoding-online/

---

<a id="work"></a>
### Work (models, analysis, EDA, etc.) :bar_chart:
The final sprint switched gears from a clustering algorithm to a content-filtering recommendation. The formation of the recommendation required modifying the `review_keywords` in the attractions dataset to a numeric value. Two transformations were performed on the `review_keywords` to compare review word frequency: CountVectorizer and TfidfVectorizer. Ultimately, the texts from TfidfVectorizer were used for forming the recommendation model. 

The Content-Based Filtering recommendation system would take in user input and historical data of review keywords to return the top 5 recommended attractions. The necessary user inputs are the current location of the user, the attraction interest of the user, and the name of the attraction similar to their interest. The number of reviews threshold is set to 10 but can be modified by the user. The recommendation function will filter attractions by interest, calculate cosine similarity, and order by the distance between the user and attraction. The cosine similarity measures the similarity of the review keywords to other attractions. The distance difference will be calculated by using the latitude/longitude of attractions and user location to the haversine function for simplicity. The haversine function doesn't consider established road directions. The following interest options are limited to the following and are case-sensitive: park, restaurant, shopping, tourist. An assumption made was that `similar_attraction_name` has the same category as `interest`. 

Lastly, a sentiment model was performed using logistic regression to predict if the average review is positive or negative based on its keywords. The model used the TF-IDF vectorized texts. Simplify the sentiment analysis problem by creating a new set of ratings where less than or equal to 3.5 will count as 0 (bad), and greater than 3.5 will count as 1 (good).

---

<a id="results"></a>
### Results :eyes:
**Text Analysis on Review Keywords - CountVectorizer vs. TfidfVectorizer**

![image](https://github.com/user-attachments/assets/1705f6b7-d828-4744-bb1f-387a793fe78a)

*Key Findings for CountVectorizer*
- Across all attraction types, Google Maps reviewers visiting Vancouver frequently highlight their outdoor experience (ex. sunset, trail, patio, court) and accessibility (ex. walk, parking, hour).
- Google Map reviewers take into consideration the attraction's price and community.

![image](https://github.com/user-attachments/assets/8ca1674b-9320-4f63-a880-91bc01341890)

*Key Findings for TfidfVectorizer*
- Google map reviewers visiting Vancouver frequently highlight their outdoor experience (ex. sunset, trails, picnic, court, tennis, park) and accessibility (ex. walk, parking, space).
- Google map reviewers take into consideration the attraction's price.

**Advanced Modelling - Cosine Similarity and Content-Based Filtering - Proof of Concept Evaluation**

The following test cases and expected results will be explored:
- Good: Similiar attraction and interest are close to the user --> Ella the Explorer
    - Ella is currently located in 200 Burrard Street (ie. Tim Hortons in Waterfront Centre)
    - Ella is interested in park attractions similar to Stanley Park
- Bad: Similiar attraction and interest are not close to the user --> Vicky Sullivan
    - Vicky is currently located at 5000 Canoe Pass Wy (ie. Tsawwassen Mills)
    - Vicky is interested in shopping attractions similar to Metropolis at Metrotown
- Bad: Recommendations are not similar to the attraction name and interest

![image](https://github.com/user-attachments/assets/5fae42bf-1515-41e7-ae6d-600d9a9c2a3b)

![image](https://github.com/user-attachments/assets/31bbbbd3-f25a-4af3-b28a-165071e713ec)

*Interpretations for Recommendation Model Evaluation*

Most of the top 5 recommendations have a cosine-similarity score ranging from 0.2 to 0.4 which means that the current model can make low-moderate attraction recommendations to users. The model would require the full text of reviews rather than review keywords to improve model performance.

**Sentiment Modelling - Predict Average Positive / Negative Reviews**

![image](https://github.com/user-attachments/assets/15f9186c-b175-4926-a516-d82d8a5fed85)

![image](https://github.com/user-attachments/assets/ab51672e-52c8-4375-bda6-c042002815d7)

![image](https://github.com/user-attachments/assets/4f5557c5-404c-4495-a3d4-bf7ff9d270b0)

*Actionable Insights from Logistic Regression*
From Positive Reviews
- Words such as "walk", "parking", and "space" suggest that Google Map reviewers on average value accessibility in their attraction experience.
    - Businesses supporting tourism should consider spacious pedestrian paths towards their attraction if that hasn't been implemented yet. 
    - Businesses supporting tourism should advertise multiple ways to reach their attraction by driving, commuting, or walking directions from another popular destination.  
- Words such as "picnic", "trails", and "tennis" suggest that Google Map reviewers on average value outdoor spaces or activities in their attraction experience. 
    - Businesses supporting tourism

Due to the lack of negative reviews, the current logistic regression model displays a poor representation of words associated with negative sentiment. A future direction for this model would be to query more attractions with negative reviews on average.  

*Sentiment Model Evaluation - Confusion Matrix*

![image](https://github.com/user-attachments/assets/6e9d2e6e-9fa0-4a6f-ba83-fd92798fe6c3)

From the confusion matrix, the current logistic regression model failed to categorize negative reviews. For future direction, more negative reviews should be collected for a better performance of sentiment modelling.

--- 

<a id="future-direction"></a>
**Future Direction**
- Optimize Scheduling
    - Ask the User the Duration of the Vancouver Visit --> Arrival Date, Departure Date 
    - Collect data on Attraction's operation/opening hours and days
- Optimize Money Spent
    - Ask the User their Budget
    - Collect data on Attraction's admission fees  
- Map User’s current location and recommended attraction locations
- Collect more attractions with an average negative rating (ie. less than 3.5 out of 5) for an even distribution and a better performance of sentiment modelling

### Party! :tada:
