# Movie-Recommender-system
### Description
The movie recommender system aims to provide personalized movie recommendations to users based on ratings. By using data on movie genres and ratings, the system will predict and suggest movies that users are likely to enjoy.
The system will use collaborative filtering (user - based) to make suggestions for each user.

### Business problem
Users finds it difficult to choose content that matches thier preferences. Lack of personalized recommendations leads to users disatisfaction.

The recommender system will:
 - Help users to discover new content that they may enjoy based on their unique preferences.

 - Enhance user experience by providing personalized movie recommendations.

 - Ensure viewers are exposed to a variety of relevant movies

 ### Objectives
 - Peformance avaluation of the models

- Personalization

- User engagement

### Shareholders
- `End users` - these are the people who consume the content and are relying on reccomendations.

- `Content creators` - these are people who are producing the content

- `marketing team` - these team will rely on the system to come up with a marketing strategy

- `product team` - these are for integrating the system into the platforms

### **Data loading and Data understanding**

### Necessary libraries

`pandas`:data manipulation and analysis

`numpy`: For numerical computing

`surprise`: For building and evaluating recommender systems

`Dataset from surprise`: To load datasets for recommendation tasks in surprise liblary

`Reader from surprise`: To define the format and stracture of datasets

`KNNWithMeans,KNNBasic,KNNBaseline,`: K-Neighbors algorithims designed for building collaborative filtering recommendation systems.

`cross_validate`: to perform cross-validation on a given model

`GridsearchCV`: used for hyperparameter tuning.

`SVD`: a matrix factorization technique used for building recommendation systems.

`matplotlib.pyplot and seaborn`: for creating interactive visualizations.

### Data description
 Data use for this recommender system is the https://grouplens.org/datasets/movielens/latest/ sourced from the GroupLens research lab at the University of Minnesota. The `small` dataset containing 100,000 user ratings and a subset of the data i.e `movies` and `ratings` was used.

 Tha data contains columns:

 - ` movieID` - uniquely identifies a movie 

- ` title` - name of a movie

- `genres` - genre to which a movie belongs

- `userID` - uniquely identifying each user

- ` rating` - a rating given to movies by users

- `timestamp` - time the movies were released

### Visualization

ratings distribution.

![My Image](https://github.com/Mash-bot/Movie-Recommender-system/blob/main/images/download.png)

genre popularity.

![My Image](https://github.com/Mash-bot/Movie-Recommender-system/blob/main/images/genre%20popularity.png)

rating distribution by genres.

![My Image](https://github.com/Mash-bot/Movie-Recommender-system/blob/main/images/rating%20distribution%20by%20genres.png)

top 10 movies by average rating.
![My Image](https://github.com/Mash-bot/Movie-Recommender-system/blob/main/images/top%2010%20movies%20by%20average%20rating.png)

### **Data Preparation for Surprise** 

- Colums `userId, movieId, rating` are used 

reader = Reader (rating_scale= (0.5, 5.0))
data = Dataset.load_from_df(movie_rating[["userId", "movieId", "rating"]], reader)

- training using `dataset = data.build_full_trainset()`

- Performing a user-based system

### **Modeling**

1. SVD with GridSearch;
- ` rmse` for the SVD is around 86.93%
- ` gridsearch` best parameters are `n_factors = 30` and `reg_all = 0.03`
2. Cross validation with KNNBasic;
- `test_rmse` for this model is around 97.36 % an improvement from SVD

- `pearson` is the similarity metrics 

- ` KNNBasic` for computing the similarity scores and making predictions

- ` User_based = TRUE` a type of filtering for user_based system
3.  Cross validation with KNNBaseline;
- ` test_rmse` for this model is around 87.67

4. Cross validation with KNNWithMeans;
- ` test_rmse` for this model is around 89.74%

`RMSE` is used to evaluate the accuracy of predicted ratings. It measures the average difference between predicted and actual ratings (how close or far apart the predictions are from the actual values).

`Lower RMSE` indicates a better fit of the model
From the above outputs, SVD seems to be the best performing model with a test RMSE of around `86.85 %`

We will therefore use SVD to make predictions

### **Using SVD and to make predictions**

svd = SVD(n_factors= 30, reg_all=0.05)
svd.fit(dataset)

creating a function user rating that allows a user to rate a certain number of movies on a scale from 1 to 5 or skip movies they haven’t seen

### Making predictions with the new rating

user_ratings = pd.DataFrame(user_rating)
new_ratings_df = pd.concat([movie_rating, user_ratings], axis=0)
new_data = Dataset.load_from_df(new_ratings_df,reader)

- Creating a DataFrame (user_ratings) from a list of dictionaries (user_rating).

- Concatenate the movie_rating DataFrame  with the newly created user_ratings DataFrame to form a DataFrame (new_ratings_df).

- Load the concatenated DataFrame (new_ratings_df) into the surprise library’s Dataset format using Dataset.load_from_df()

### Model training with the new data

svd_ = SVD(n_factors= 30, reg_all=0.05)
svd_.fit(new_data.build_full_trainset())

- the model (svd_) is trained and ready to make predictions about how a user might rate movies they haven’t seen yet

## Predcitions for the user from the highest to the lowest

- The ranked_movies list contains a sorted list of tuples, where each tuple consists of a movie ID and the predicted rating for user 1000. The list is ordered from the movie with the highest predicted rating to the lowest.

### **Making Recommendations**

`Recommendation # 1`: The code looks for the movie title that corresponds to movieId 659, which is "The Godfather, The (1972)".

`Recommendation # 2`: It then looks for movieId 277, which corresponds to "The Shawshank Redemption (1994)".

`Recommendation # 3`: Next, for movieId 2226, it finds "Fight Club (1999)".

`Recommendation # 4`: For movieId 2462, the title is "The Boondock Saints (2000)".

`Recommendation # 5`: Finally, for movieId 906, it finds "Lawrence of Arabia (1962)".