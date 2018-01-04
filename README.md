# Build-a-Recommendation-Engine-in-Python

Recommendation engines are nothing but an automated form of a “shop counter guy”. You ask him for a product. Not only he shows that product, but also the related ones which you could buy. They are well trained in cross selling and up selling. So, does our recommendation engines.

The ability of these engines to recommend personalized content, based on past behavior is incredible. It brings customer delight and gives them a reason to keep returning to the website.

 I will cover the fundamentals of creating a recommendation system using GraphLab in Python.
 
## Topics Covered
  1. Type of Recommendation Engines
  2. The MovieLens DataSet
  3. A simple popularity model
  4. A Collaborative Filtering Model
  5. Evaluating Recommendation Engines
  
### Type of Recommendation Engines
#### Recommend the most popular items
A simple approach could be to recommend the items which are liked by most number of users. This is a blazing fast and dirty approach and thus has a major drawback. The things is, there is no personalization involved with this approach.

Basically the most popular items would be same for each user since popularity is defined on the entire user pool. So everybody will see the same results. It sounds like, ‘a website recommends you to buy microwave just because it’s been liked by other users and doesn’t care if you are even interested in buying or not’.

Surprisingly, such approach still works in places like news portals. Whenever you login to say bbcnews, you’ll see a column of “Popular News” which is subdivided into sections and the most read articles of each sections are displayed.

#### Using a classifier to make recommendation
We already know lots of classification algorithms. Let’s see how we can use the same technique to make recommendations. Classifiers are parametric solutions so we just need to define some parameters (features) of the user and the item. The outcome can be 1 if the user likes it or 0 otherwise. This might work out in some cases because of following advantages:
  * Incorporates personalization
  * It can work even if the user's past history is short or not available
But has some major drawbacks as well because of which it is not used much in practice:
  * The features might actually not be available or even if they are, they may not be sufficient to make a good classifier
  * As the number of users and items grow, making a good classifier will become exponentially difficult
  
#### Recommendation Algorithms
Now lets come to the special class of algorithms which are tailor-made for solving the recommendation problem. There are typically two types of algorithms – Content Based and Collaborative Filtering. You should refer to our previous article to get a complete sense of how they work. I’ll give a short recap here.
  ##### Content based algorithms:
  *Idea: If you like an item then you will also like a “similar” item
  *Based on similarity of the items being recommended
  *It generally works well when its easy to determine the context/properties of each item. For instance when we are recommending the same kind of item like a movie recommendation or song recommendation.
  ##### Collaborative filtering algorithms:
  * Idea: If a person A likes item 1, 2, 3 and B like 2,3,4 then they have similar interests and A should like item 4 and B should like item 1.
  * This algorithm is entirely based on the past behavior and not on the context. This makes it one of the most commonly used algorithm as it is not dependent on any additional information.
  * For instance: product recommendations by e-commerce player like Amazon and merchant recommendations by banks like American Express.
  * Further, there are several types of collaborative filtering algorithms : 
      1. User-User Collaborative filtering: Here we find look alike customers (based on similarity) and offer products which first customer’s look alike has chosen in past. This algorithm is very effective but takes a lot of time and resources. It requires to compute every customer pair information which takes time. Therefore, for big base platforms, this algorithm is hard to implement without a very strong parallelizable system.
      2. Item-Item Collaborative filtering: It is quite similar to previous algorithm, but instead of finding customer look alike, we try finding item look alike. Once we have item look alike matrix, we can easily recommend alike items to customer who have purchased any item from the store. This algorithm is far less resource consuming than user-user collaborative filtering. Hence, for a new customer the algorithm takes far lesser time than user-user collaborate as we don’t need all similarity scores between customers. And with fixed number of products, product-product look alike matrix is fixed over time.
      3. Other simpler algorithms: There are other approaches like market basket analysis, which generally do not have high predictive power than the algorithms described above.
  
 ## The MovieLens DataSet
 I will be using the MovieLense dataset for this purpose. It has been collected by GroupLens 100K dataset can be downloaded from 
 here: https://grouplens.org/datasets/movielens/100k/
 It consists of:
    1. 100,000 ratings (1-5) from 943 users on 1682 movies.
    2. Each user has rated at least 20 movies.
    3. Simple demographic info for the users (age, gender, occupation, zip)
    4. Genre information of movies.
 There are many files in the ml-100k.zip file which we can use. above file recommend_by_popularity.py, i have loaded the three most importance files to get a sense of the data. I also recommend you to read the readme document which gives a lot of information about the difference files.
 
## A Simple Popularity Model
Lest start with making a popularity based model, i.e. the one where all the users have same recommendation based on the most popular choices. We'll use the graphlab recommender function popularity_recommender for this.

we can train a recommendation as:

popularity_model = graphlab.popularity_recommender.create(train_data, user_it = 'user_id', item_id = 'movie_id', target = 'rating')

Arguments
  * train_data: the SFrame which contains the required data
  * user_id: the column name which represents each user ID
  * item_id: the column name which represents each item to be recommended
  * target: the column name representing scores/ratings given by the user
Lets use this model to make top 5 recommendations for first 5 users and see what comes out:
  popularity_recomm = popularity_model.recommend(users = range(1,6), k = 5)
  popularity_recomm.print_rows(num_rows = 25)

    
