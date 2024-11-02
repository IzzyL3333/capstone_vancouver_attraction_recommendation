# IZABEL CAPSTONE PROJECT README

### Big Idea Project :airplane:
Using a machine learning model that recommends content, this project aims to generate a custom Vancouver itinerary based on user input and historical data. This is an opportunity for travellers to save time researching where to go and in what order. This is also an opportunity for tourism businesses to understand how customers are exploring Vancouver and where to optimize their resources at a given time of day. 

---

### Goals :mag:
The goal of this project is to create a recommended Travel Itinerary of Vancouver based on user's input and preferences alongside historical data. 

The user will input where they are located, their duration of stay in Vancouver, what types of attractions interest them, and at what speed they want to explore. For the duration, it will determine how many days the schedule will produce. For types of attractions, it will determine where they will go by location and their interests. For speed, it will determine the frequency and duration of activities per day. The speed 'slow' will produce 3 activities per day, 2.5 hours per activity. The speed 'medium' will produce 4 activities per day, 2 hours per activity.
The speed 'fast' will produce 5 activities per day, 1.5 hours per activity. 

For historical data, the attraction's Google rating, number of reviews, location, and categories will be explored. The attraction's Google rating and number of reviews will help decide what places are recommended from other customer experiences. The attraction's location will help decide how far customers will go from their input location. The attraction category will help decide where the customer will go to their interests. 
       
The intended output is a recommended schedule and interactive map of activities and restaurants from 8:00 am to 6:00 pm. Breakfast, Lunch, and Dinner will start and end at 8-9 am, 12-1 pm and 5-6 pm respectively. That leaves the remaining hours to contain activities based on where the customer is located and preferences. Optional or alternative activities can recorded at the end of each day should the customer change their minds.  

---

### Data :books:
Data was collected from Google Map Extractor and Geoapify.

**Google Map Extractor** is a free resource on Git Hub that extracts business profiles from Google Maps. Query the desired data as you do under Google Maps and it will return ~120 Search Results that can be downloaded as a CSV file. 

_NOTE: The free version only allows 20 searches per month. Unlimited searches for a lifetime require upgrading to the Pro Version._

More details of Google Map Extractor: https://github.com/omkarcloud/google-maps-scraper?tab=readme-ov-file

**Geoapify's Online Geocoding Tool** is a free resource that takes a list of addresses and returns more detailed columns about the address such as their longitude and latitude that can be downloaded as a CSV file.

_NOTE: The tool only allows a maximum of 500 rows. Larger datasets need to be split._

More details of Geoapify's Online Geocoding Tool: https://www.geoapify.com/tools/geocoding-online/

---

### Work (models, analysis, EDA, etc.) :bar_chart:
The initial EDA explored null values, duplicated columns/rows, the number of Unique Values in the Main Category, and the Distribution of Rating and Review Data for further data collection and cleaning. Where the null values are located and how much there are to the total number of rows could negatively impact the model evaluation. Duplicated columns/rows should be deleted to remove redundancy and incorrect trends in the data. How Google Maps Extractor identifies the main category could affect the attraction's relationship with other variables like the number of reviews and ratings. The Distribution of Rating and Review Data displays how Google's recommendation systems cater their responses to previous queries.  

The proposed baseline model would be to explore Unsupervised Learning, specifically Clustering as there are no known target variables in this dataset.  

---

### Results :eyes:
The data requires further collection and cleaning before proceeding to use the baseline model.

The columns containing description, website, and review_keywords are where the data has null values. Since those values won't be used in the baseline model, there is no need to replace them at this point. Those columns are meant to give additional insight to users about the attraction.    

Almost 40% of the data contains duplicated rows. The reason for this duplicated data is from the querying from Google Maps Extractor. The more you query specific types of tourist attractions, the more likely the results tend to overlap. Tourist attractions can fall under many types. For example, Stanley Park is a result of being a landmark and nature type of tourist attraction. The next steps would be to drop the duplicated data and expand the scope of the queries to go beyond Vancouver such as North Vancouver, West Vancouver, Burnaby, and Richmond.

From the main_category, there are 75 unique values. For the next steps, those that have one or a few counts of a category can be added to a larger category type. From 75 unique main categories to 5: Landmarks, Nature, Shopping, Food Markets, Other. For example, Playgrounds can be under the collected Nature category.  

The distribution of data for rating and reviews is skewed to a certain range which could impact model evaluation. The rating for attractions is skewed right towards a range of 4 to 5 stars out of 5. The reason for the skewed ratings can come from how the search engine for Google Maps and their recommendation model. The number of reviews for attractions is skewed left towards 0-5000 reviews. The next step would be to transform the data before the baseline model to ensure the columns are treated equally.

Data visualization for attractions based on location is the next step to determine trends and relationships between the attraction's ratings to their address. 

### Party! :tada:
