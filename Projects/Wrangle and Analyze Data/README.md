# Introduction
Gathering, assessing and cleaning of data is called data wragnling. Using Python and its libraries we will gather data from a variety of sources and in a variety of formats including SQL. In this project we will use Jupyter Notebook for data wrangling, analysis and visualization.

## Data Source
* WeRateDogs Twitter archive *twitter_archive_enhanced.csv* 
* The tweet image predictions *image_predictions.tsv*
* Download the entire set of tweet's in JSON format using Python's Tweepy library.

## Project details
### wrangle_act.ipynb
* Assess the data for quality and tidiness issues using using visual and programming techniques.
* Clean the data to get high quality and tidy master pandas DataFrame.
* Stored the clean in a SQLite database *twitter_archive_master.db* 
* Unit test
### act_report.ipynb
* Generate various insights and visualization from the database using Jupyter Notebook 