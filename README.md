# Movie Prediction Using Pre-Production Factors
## Central Questions and Goals
This project centered around the discernment of the factors leading to a film's commercial success. Could these factors be determined prior to production, and if so, to what extent? What are relatively important and unimportant factors? To what extent can they be used in combination to explain the varience in box office of different films? My goal was to answer these questions and come up with new ones as I worked through this project and began exploring my data.
## Data & Cleaning
This project began with the [MovieTweetings](https://github.com/sidooms/MovieTweetings) dataset, as it helped me with a few of my early challenges. First, as this project was primarily concerned with the commercial success of movies, experimental and highly niche projects were of limited interest to me. MovieTweetings allowed me to pre-filter for some of these factors, thus preventing me from having to query too many projects that were unrelated to my central questions. Second, MovieTweetings could provide an exciting avenue of future exploration for this project, allowing me to investigate what factors lead to continued interest and success after release.

After my initial filtering, I used the IMDB film ids to query [OMDB](http://www.omdbapi.com/) where I obtained several fields relating to movie characteristics including plot summary, actors, directors, writers, as well as box office information. I obtained additional data from IMDB lists so I could create collections of movies with certain characteristics, such as films based on prior media. As I would be predicting movie grosses across years, I performed an inflation adjustment on the domestic gross using Consumer Price Index data from [FRED](https://fred.stlouisfed.org/categories/9). After obtaining the data I had to perform a number of transformations to make it usable, which can be seen below(please note that while I did have data that would be post-production or post-release, I only used it for my own analysis and it was not used as a feature for prediction).
![cleaning functions](images/cleaning.jpeg?raw=true "Title")

## Plot Summary and NLP
I was particularly interested in using story as a predictor for box office beyond as broad a category as genre or even subgenre, as it is often the first element of a picture that will be known. In order to do this, I needed to find a way to transform the plot summaries I had obtained from IMDB in such a way that they could be used as features for prediction. I ended up using spaCy doc vectors for each plot summary as my numeric representation of plot. I breifly considered using a Count or TF-IDF vectorization as well but as my dataset was not too extensive I opted for the more computationaly expensive choice of using word vectors for their relative accuracy of semantic representation. My process was standard: tokenization, lemmatization, stop-word removal, then vectorization. I used NLTK and spaCy for this process.
![cleaning functions](images/doc_creation.jpeg?raw=true "Title")
After this process, I also used the doc vectors to examine the relationship between plot and box office directly, as well as been the plots themselves. I performed the dimensionality reduction using PCA for the resulting visualizations.
![cleaning functions](images/box_story.png?raw=true "Title")
![cleaning functions](images/story.png?raw=true "Title")

## Model and Results
This project made extensive use of dimensionality reduction, as my initial dataset after the one-hot encoding of categorical variables was over 5000 dimensions. Because of this the effectiveness of my features was primarily assessed by directly examining the effect of their inclusion or exclusion on the performance of my model and my evaluation metrics (RMSE, r-squared, explained variance score, mean absolute error, median absolute error). In an effort to maintain intrepretability, I generated a correlation heatmap of the relationships between my final features and my target, with the caveat that PCA and SVD were needed to generate it and this is not an accurate representation of the dimensionality of the dataset used for prediction.
![cleaning functions](images/corr_map.png?raw=true "Title")
I used XGBoost for the final model as it was the best performer in intial experiments. The final tuned values are as follows:
![cleaning functions](images/regressor.jpeg?raw=true "Title")
And the results:
![cleaning functions](images/pva.png?raw=true "Title")
![cleaning functions](images/residual_plot.png?raw=true "Title")
