# IZABEL CAPSTONE PROJECT README

### Big Idea Project :airplane:
Using a machine learning model that recommends content, this project aims to generate a custom Vancouver itinerary based on user input and historical data. This is an opportunity for travellers to save time researching where to go and in what order. This is also an opportunity for tourism businesses to understand how customers are exploring Vancouver and where to optimize their resources at a given time of day. 

---

## Table of Contents
- [Goals](#goals)
- [Data](#data)
- [Work](#work)
- [Results](#results)
- [Next Steps](#next-steps)

---

<a id="goals"></a>
### Goals :mag:
This project aims to create a recommended Travel Itinerary of Vancouver based on users' input and preferences alongside historical data. 

The user would input their logging location, duration of stay in Vancouver, what attractions interest them, and at what speed they want to explore. For the duration, it will determine how many days the schedule will produce. For types of attractions, it will determine where they will go by location and their interests. For speed, it will determine the frequency and duration of activities per day. The 'slow' speed option would produce 3 activities per day, 2.5 hours per activity. The 'medium' speed option would produce 4 activities per day, 2 hours per activity. The 'fast' speed option would produce 5 activities per day, 1.5 hours per activity. 

For historical data, the attraction's Google rating, number of reviews, location, and categories will be explored. The attraction's Google rating and number of reviews will help decide what places are recommended from other customer experiences. The attraction's location will help determine how far customers will go from their logging location. The attraction category will help dictate where customers will go tailored to their interests. 
       
The intended output is a recommended schedule and interactive map of activities and restaurants from 8:00 am to 6:00 pm. Breakfast, Lunch, and Dinner will start and end at 8-9 am, 12-1 pm and 5-6 pm respectively. That leaves the remaining hours to contain activities based on customer's location and preferences. Optional or alternative activities can recorded at the end of each day should the customer change their minds.  

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
The second round of data collection and cleaning expanded the geographical scope, collected geographical details, and reduced the main categories. Queries expanding beyond downtown Vancouver to include Burnaby, Richmond, West Vancouver, and North Vancouver. After the geographical scope expansion and removing duplicates produced 961 rows of data. Given the new dataset, their addresses were used to extract more graphical details in Geoapify. Latitude and Longitude data were collected as a location-based feature for the upcoming model. The original amount of main categories by the Google Maps Extractor was 161. Using One-HotEncoding for the main category, the total amount of unique values was reduced to four as the following columns: MC_park, MC_restaurant, MC_shopping, MC_tourist.

For the preprocessing model, four major transformations were performed.

**1. Log Transform Latitude and Longitude**

After removing location outliers, log transformation was performed on latitude and longitude to align to a normal distribution and for better model performance. 

**2. CountVectorize on Review Keywords**

To perform text analysis on review keywords, CounterVectorizer transformed the data to determine the most frequent words to describe attractions. 

**3. Log Transform and Scale Ratings and Reviews**

Log transformation was performed on ratings and reviews to align to a normal distribution and for better model performance. 

**4. PCA Transform for Final Model Data**

PCA transformation was performed on collected data including the attraction's location (ie. Latitude and Longitude), average rating, count of reviews, and review keywords to account for high dimensionality.

For the baseline model, the following cluster models were performed against the data and evaluated by silhouette score 
- K-Means 
- Gaussian Mixture Model
- Hierarchical Clustering (Agglomerative)

---

<a id="results"></a>
### Results :eyes:
**Preprocessing**
1. Log Transform Latitude and Longitude
![Screenshot 2024-11-24 163824](https://github.com/user-attachments/assets/047870c8-3a2d-4afd-8d61-9bfb9da6f042)

Key Findings
- The longitude data resembles less like a normal distribution and more like a bimodal distribution. The current longitude data are all negative and cannot be transformed by log due to its negative values.
- From the log transformation, the distribution for latitude didn't improve. For the baseline model, the latitude data before the log transformation will be used.

2. CountVectorize on Review Keywords
![Screenshot 2024-11-24 163428](https://github.com/user-attachments/assets/d94ece35-f217-4d02-af2f-3834d1d95ce0)

Key Findings
- Across all attraction types, Google Map reviewers visiting Vancouver frequently highlight their outdoor experience (ex. sunset, trail, patio, court) and accessibility (ex. walk, parking, hour).
- Google map reviewers take into consideration the attraction's price and community.

3. Log Transform and Scale Ratings and Reviews
![Screenshot 2024-11-24 163703](https://github.com/user-attachments/assets/83c90c97-fbaa-4c9d-8d4c-d1b75754f629)

Key Findings
- The log transformation and scaling improved the distribution of Customer Reviews and will be used for the cluster model.
- Since log transformation and scaling did not improve the distribution for Customer Ratings, it will not be used for the cluster model.
- The original `rating` and scaled `reviews` columns will be used for the cluster model. 

4. PCA Transform for Final Model Data

PCA Transform on Unscaled data

![Screenshot 2024-11-24 164231](https://github.com/user-attachments/assets/751546f6-46ec-4dbb-a21d-e9d9b3c6c41c)

PCA Transform on Scaled data

![Screenshot 2024-11-24 164352](https://github.com/user-attachments/assets/3f3a02db-762c-48cb-84a2-67944e796dc0)

Key Findings
- From the 3d graphs, the PCA should be performed BEFORE generating the cluster model on the scaled data. There is no distinct segmentation for scaled data after PCA transformation
- For the upcoming baseline model, the scaled dataset will be used. 

**Baseline Model**

![Screenshot 2024-11-24 165224](https://github.com/user-attachments/assets/1f820fd3-2fab-47dd-839b-970c50b965e5)

- Clustering before PCA transformation results in the best-performing model based on silhouette score is **KMeans(n_components = 2)** with **~0.075**.
- Clustering after PCA transformation results in the best-performing model based on silhouette score is **GaussianMixture(n_components = 4)** with **~0.365**.

Baseline models before and after PCA transformation produce weak silhouette scores from the nature of dimensionality. Given the large dimensionality of the data, it becomes difficult to achieve high values because of the curse of dimensionality, as the distances become more similar.

--- 

<a id="next-steps"></a>
**Next Steps**
- Feature Engineering
  - `review_keywords` transformed with TF-IDF and Word2Vec
  - `rating` transformed with different scales (ie. MaxAbsScaler, MinMaxScaler, Normalizer) 
  - research transformations relating to the bimodal distribution of data for `lon`
- Parameter tuning for GaussianMixture Model
- Research and create sample users to act as user input in the recommendation system and review the model's performance
- Use content-based filtering to recommend items with the highest feature similarity to the user's profile 

### Party! :tada:
