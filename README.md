# Machine Learning Project Report - Hana Octavia Trinida Malo

## Project Domain

A recommendation system is a system for making suggestions to *customers* based on various pieces of information with the goal of enhancing product sales. On *online* buying sites like eBay, Alibaba, and OLX, which sell clothing, electronics, home appliances, and other items, recommendation systems can be found. There are additional recommendation systems on websites that only offer recommendations to their users, such as the MovieLens website and the Internet Movie Database (IMDb), which give its users suggestions for movies to watch.[1]

Different readers in the library have different reading preferences; some only want to read the works of authors they enjoy. According to how well readers who have already read the book feel about it, there are also readers who have read the book. The reader is more eager to read a book the more favorable reviews it receives.

Currently, the library has about 250 thousand books available for reading. Because there are so many books available, readers frequently find it challenging to decide which one to read, which discourages them from doing so. Building a suggestion system can be the ideal way to increase the number of readers since it will make it simpler for readers to read books in accordance with their preferences.


## Business Definition

A library contains a variety of reader information, a catalog of books, and reader ratings for those books. The recommendation system that this library hopes to implement will be able to offer suggestions to readers depending on the author of the book they have already read and their rating.

### Problem Statement

Based on the background described above, the problems that must be resolved include:

- Based on data about books and readers, how to make a personalized recommendation system using content-based filtering techniques?
- With the rating data you have, how can the library recommend other books that users might like?

### Objective

To answer these questions, create a recommendation system with the following objectives:

- Generate a number of personalized book recommendations for users with content-based filtering techniques.
- Generate a number of book recommendations according to user preferences with collaborative filtering techniques.

### Solution

Solutions to achieve the above goals are:

- implementing content based filtering in creating a recommendation system, which uses the author's name to recommend to readers
- apply collaborative filtering in making recommendation systems, which use rating data to recommend to readers

## Collaborative Filtering

Collaborative filtering is a recommendation method that can provide recommendations to users based on items liked by other users who have similar preferences [2].

The weakness of the content-based filtering technique is that it can only recommend items that are similar so that the recommended items are limited [3].

### Data Understanding

The dataset that I use is a dataset that has data on books, readers, and ratings, which I download from the following link: [Book Recommendation Dataset](https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset)

In this dataset there are 3 files namely books.csv, rating.csv, and user.csv(readers). The following is the amount of data from the 3 files:

```
Number of book data:  271360
Number of user data:  278858
Number of rating data :  95513
```

#### The variables in the Book Recommendation Dataset are as follows:

The variables in the Book Recommendation Dataset are as follows:

- books : is a list of books in the library containing the ISBN number, book title and book author
- user : is a list of users
- rating: is a list of users and ratings given to books that have been read

The books and rating variables will be used in the recommendation model to be built.

#### Exploratory Data Analysis

#### Unvariate variabel

**Exploratory Variabel books**

Table 1 shows a list of the variables in the books. But what we use are only the variables ISBN, book title, and book author.
Explanation of variables :
- ISBN : ISBN number of the book
- BookTitle : is the book title data
- BookAuthor : is the book author's data

| Column            | Non-Null Count        | Dtype      |
| ------------------|----------------------|----------|
|   ISBN             |  271360 non-null | object    |
|   BookTitle         | 271360 non-null | object    |
|  BookAuthor         |271359 non-null | object |
|  YearOfPublication | 271360 non-null  |object|
|   Publisher         | 271358 non-null | object|
|  ImageURLS        |  271360 non-null | object|
|  ImageURLM        |  271360 non-null | object|
| ImageURLL         | 271357 non-null  |object|

Table 1. variables in the books variable

The table below is the data in the book variable. In the table below, only the top 4 data are shown

|ISBN	      |BookTitle	            |BookAuthor	       |YearOfPublication |Publisher	               |
| ----------|----------------------|------------------|------------------|-------------------------|
|	195153448	|Classical Mythology	  |Mark P. O. Morford|	2002	            |Oxford University Press	 |
|	2005018	  |Clara Callan	Richard  |Bruce Wright	     |2001	             |HarperFlamingo Canada	   |
|	60973129	 |Decision in Normandy	 |Carlo D'Este	     |1991		            |HarperPerennial	          |
|	374157065	|Flu: The Story of ...	| Gina Bari Kolata	|1999		            |Farrar Straus Giroux      |
|	393045218	|The Mummies of Urumchi|E. J. W. Barber	  |1999		            |W. W. Norton &amp; Company |

Table 2. Books variable data

The total number of books available is as shown in the figure below

![Screenshot (463)](https://user-images.githubusercontent.com/86582130/192175319-d3f12775-ef5d-4e26-9753-fbf31ea3e720.png)

Figure 1. number of books

Figure 1 shows the data of the books we have totaling 271360

**Exploratory Variabel user**

Based on table 3, it shows that the user variable has 3 variables namely
- UserID : contains user ID
- location : contains the user's location
- age : the age of the user

|UserID	 |    Location	                      |    Age|
| ------|------------------------------------|-----|
| 1	   |nyc, new york, usa	                 |NaN   |
| 2	   |stockton, california, usa	          |18.0   |    
| 3	   |moscow, yukon territory, russia     |	NaN   |
| 4	   |porto, v.n.gaia, portugal	          |17.0   |
| 5	   |farnborough, hants, united kingdom  	|NaN   |

Table 3. User variable data


**Exploratory Variabel rating**

|UserID	|ISBN 	       |BookRating|
| -----|-----------|-----------|
|276725	|	034545104X  |	0        |
|276726	|	155061224  |	5        |
|276727	|446520802    |	0        |
|276729	|	052165615X  |	3        |
|276729	|	521795028	  |6        |

Table 4. Rating data

Table 4 shows the number of readers who have rated as many as 95513 readers
From the rating.head() function, we can see that the rating data consists of 3 columns. The columns include:

- userID, is the identity of the user.
- ISBN, is the identity of the book, which contains the book's ISBN number.
- Rating, is rating data for books

### Data Preprocessing

**Combine rating dataframe with books based on ISBN**

To combine the two dataframes, we use the merge() function based on the same variables from the two dataframes, in this case the ISBN.

Table 5 shows that our data has been successfully combined

|UserID |ISBN	      |BookRating	  |BookTitle             |BookAuthor	    |YearOfPublication |Publisher	                  |
| ------|-----------|-------------|-----------------------|--------------|------------------|-----------------------------|
276725	|034545104X	 |0	           |Flesh Tones: A Novel  |	M. J. Rose    |2002	              |Ballantine Books	            |
276726	|155061224	 |5	            |	Rites of Passage	    |Judith Rae	    |2001	              |	Heinle		                    |
276727	|446520802	 |0		           |The Notebook	         |Nicholas Sparks|1996	              |	Warner Books		              |
276729	|052165615X	|3	            |	Help!: Level 1	      |Philip Prowse	  |1999	              |	Cambridge University Press  |
276729	|521795028	 |6		           |The Amsterdam Connection...|Sue Leather|2001		              |Cambridge University Press  |

Table 5. Dataframe combined results

### Data Preparation

**1. Check missing values**

To find out whether or not there is a missing value in the combined dataframe, we use the isnull() function and use the sum() function to calculate the number, the results are as shown in table 6 :

|UserID             |       0|
| ------------------|-----------|
|ISBN               |       0|
|BookRating          |      0|
|BookTitle          |  107463|
|BookAuthor         |  107464|
|YearOfPublication  |  107463|
|Publisher          |  107465|
|ImageURLS          |  107463|
|ImageURLM          |  107463|
|ImageURLL          |  107467|

Table 6. Variables contain missing values

**2. Overcoming missing values**

Variables detected are null values BookTitle, BookAuthor, YearOfPublication, Publisher, ImageURLS, ImageURLM, ImageURLL as many as 107467. Use the dropna() function to delete rows that have missing values

The amount of data we now have is 941105. Next we can see the number of unique books based on ISBNs. From this data we have 257808 book data.

**3. Counting and Seeing How many times the book has been rated by readers**

At this stage, we will count how many times a book has been rated by readers. The groupby('BookTitle') function is used to group data by book title and count() is used to calculate the number of ratings

|BookTitle	                                        |jumlah_rating|
| --------------------------------------------------|-------------|
|A Light in the Storm: The Civil War Diary of ...	 |4            |
|Always Have Popsicles	                             |1            |
|Apple Magic (The Collector's series)	              |1            |
|Beyond IBM: Leadership Marketing and Finance ...	 |1            |
|Clifford Visita El Hospital (Clifford El Gran...	  |1            |

Table 7 number of ratings per book title

From table 7 we can see that the book entitled Always Have Popsicles was assessed 1 time.

**4. Count and see how many times the book has been rated by readers**

At this stage, we will calculate the average rating. The function groupby('BookTitle') is used to group data by book title and mean() is used to calculate the average rating

From table 8 we can see that the book entitled Always Have Popsicles has an average rating of 0.

|BookTitle	                                       |BookRating|
| ------------------------------------------------|-----------|
|A Light in the Storm: The Civil War Diary of ...	|2.25      |
|Always Have Popsicles	                           |0.00      |
|Apple Magic (The Collector's series)             |	0.00      |
|Beyond IBM: Leadership Marketing and Finance ...	|0.00      |
|Clifford Visita El Hospital (Clifford El Gran...	|0.00      |

Table 8 average rating per book title

**5. Merging the jml_rating dataframe with the rt_rating dataframe based on book title**

After we have counted the number of times a book has been rated by readers and calculated the average rating for each book, we will combine the two dataframes above using the merge() function. This merger aims to create a new dataframe that will be used to filter which books are popular based on ratings.

**6. Retrieving popular book data**

In this process we will retrieve data that has been rated more than 500 times. Then we sort the data based on the largest data with the sort_values() function and set ascending = False


**7. Merge popular_books with dataframe books**

Recombine the popular_books data above with the existing books dataframe with the merge() function then drop duplicate book titles using the drop_duplicates() function


**8. Convert data series to list**

At this stage we will convert the ISBN, Book Title and Book author variables into a list using the tolist() function, then we count the number with the len() function.

The results will show when the variable has 28 data

**9. Create a dictionary on data**

At this stage we will create a dictionary to determine *key-value* pairs in the book_id, book_title, and book_author data that we prepared earlier.
Use the DataFrame() function to create a new dataframe based on a predefined *key-value*

### Modeling

**1. Implementing TF-IDF Vectorizer**

At this stage, we'll build a simple recommendation system based on the author of the book. At this stage we will do 3 things, namely:
 
- Initialize TfidfVectorizer by creating new variable 'tf'
- Performs idf calculations on author data with the fit() function

- Mapping array from integer index feature to feature name with get_feature_names() function

**2. Performing fit and transformation into matrix form**

![Screenshot (471)](https://user-images.githubusercontent.com/86582130/192153035-d4b7208f-a80d-4f96-9951-cc12dcadc52e.png)

Figure 2. Matrix transformation results

Look at figure 2, the matrix we have is (28, 40). The value of 28 is the data size and 40 is the author name matrix.

**3. Generate tf-idf vector in matrix form**

Convert the tf-idf vector in matrix form with the todense() function

![Screenshot (472)](https://user-images.githubusercontent.com/86582130/192153137-c3c6e914-ca0f-4f7b-9278-3ec01654807f.png)

figure 3. tf-idf vector

**4. View the tf-idf matrix for several book titles and author names**

	
|book_name						|					shapero	|michael|	anita|	diamant|	martel|	melissa|	amy	|rowling|	rebecca	|judy	|sue|	mclaughlin	|crichton	|monk|	
|-------------------------------------------------------|-----------------------------------------------|---------|----------|-----------|------------|---------|-------------------|--------|--------------|---------|---------|--------------|---------|---------|
|The Nanny Diaries: A Novel|	0.0	|0.000000	|0.000000|	0.000000	|0.0	|0.0	|0.000000|	0.0	|0.0|	0.0|	0.000000	|	0.0	|0.0|	0.0|
|Where the Heart Is (Oprah's Book Club (Paperback))|	0.0	|0.000000|	0.000000|	0.000000|	0.0|	0.0	|0.000000|	0.0|	0.0|	0.0|	0.000000|	0.0	|0.0|	0.0|
|Angels &amp; Demons|	0.0|	0.000000	|0.000000|	0.000000	|0.0|	0.0|	0.000000|	0.0|	0.0|	0.0|	0.000000|	0.0	|0.0|	0.0|	
|Bridget Jones's Diary|	0.0	|0.000000|	0.000000|	0.000000	|0.0|	0.0	|0.000000|	0.0|	0.0|	0.0	|	0.000000	|	0.0	|0.0|	0.0|
|Harry Potter and the Sorcerer's Stone (Harry Potter (Paperback))|	0.0|	0.707107	|0.000000|	0.000000|	0.0|	0.0|	0.000000	|1.0|	0.0|	0.0	|0.000000|	0.0	|0.0|	0.0|
|The Pilot's Wife : A Novel|	0.0|	0.000000|	0.664679	|0.000000|	0.0	|0.0	|0.000000	|0.0|	0.0	|0.0|	0.747129|	0.0	|0.0|	0.0|

table 9. matrix transformation results

Table 9 is the tf-idf matrix output showing the book 'Harry Potter and the Sorcerer's Stone (Harry Potter (Paperback))' written by 'rowling. The matrix shows that the book was written by Rowling. This can be seen from the matrix value of 1.0 on Rowling's author.

**5. Calculating the degree of similarity**

At this stage we will calculate the degree of similarity between one book and another to produce candidate books to be recommended. This process will produce output in the form of a similarity matrix in the form of an array.

**6. See the similarity matrix for each book**

Table 10 shows that Shape (28, 28) is a measure of the similarity matrix of the data we have. That is, we identify the level of similarity in 28 titles. The image above selects 10 book titles on the vertical row and 5 books on the horizontal axis

|book_name	|Summer Sisters	|The Pilot's Wife : A Novel|	Bridget Jones's Diary	|Divine Secrets of the Ya-Ya Sisterhood: A Novel|The Pelican Brief and Fishing|
|------------|--------------|---------------------------|------------------------|-------------------------------------------------|-----------------------------|
|The Brethren|	0.0	|0.000000	|0.0	|0.0	|0.0|
|Harry Potter and the Sorcerer's Stone |	0.0|	0.000000|	0.0	|0.0	|0.0|
|The Da Vinci Code |	0.0|	0.000000|	0.0	|0.0	|0.0|
|The Testament	|0.0	|0.000000|	0.0	|0.0	|1.0|
|A Painted House	|0.0	|0.000000|	0.0	|0.0|	0.0|
|House of Sand and Fog|	0.0|	0.000000|	0.0|	0.0|	0.0|

Table 10. Similarity matrix for each book

The number 1.0 marked with a red box indicates that the books in column X (horizontal) are the same as the books in row Y (vertical). For example, The Testament book is identified as similar (similar) to The Pelican Brief.

**7. Get Recommendations**

At this stage we will create a book_recommendations function with the following parameters:

- book_name : The title of the book
- Similarity_data : Dataframe regarding similarity that we have defined before.
- Items: Names and features used to define similarities, in this case are 'book_name' and 'author'.
- k : There are many recommendations that you want to give.

Use the argpartition() function to retrieve the highest k values from our data similarity

Retrieving data from the highest to the lowest similarity level with the column[index[-1:-(k+2):-1]] function

The below line of code is used to prevent unrecommended book titles from appearing with the drop() function

|book_name	|author    |
|----------|-----------|
|The Summons	|John Grisham|
|The Testament	|John Grisham|
|A Painted House|	JOHN GRISHAM|
|Life of Pi	|Yann Martel|
|Where the Heart Is (Oprah's Book Club|Billie Letts|

Table 10. recommendation results

Look at table 10, 'The Firm' was written by author John Grisham. We managed to provide 5 book recommendations, 3 of which were written by John Grisham

### Evaluation

To evaluate the results of our recommendations, use metric compression. Precision is the ratio of positive correct predictions compared to the overall positive predicted outcome.

Precision = number of relevant recommendation items / number of recommended items

From the recommendations above, it is known that 'The Firm' was written by author John Grisham. Of the 5 recommended items, 3 items have been written by author John Grisham(similar). Then the Precission calculation becomes as follows


Precission = 3/5
Precission = 60%

So that the recommendation system uses our content based learning to produce a precision value of 60%

## Content Based Filtering
Content Base Filtering is a recommendation technique that provides recommendations to users by understanding the user's needs, preferences, and constraints. This information and the user's previous interactions are then created in such a way as to build a user profile. The advantage of this technique is that it can provide recommendations to users even though the recommended item is a new item without having to look at the rating of the new item [2]

The advantage of this method approach is that it can produce quality recommendations. The drawback of this method is that the complexity of the calculation will increase along with the increase in system users, the more users use the system, the longer the recommendation process.

### Data Understanding

At this stage we import the required libraries and define then read the dataset. The dataset that we use is the rating dataset, which we defined earlier.


|UserID	|ISBN 	       |BookRating|
| -----|-----------|-----------|
|276725	|	034545104X  |	0        |
|276726	|	155061224  |	5        |
|276727	|446520802    |	0        |
|276729	|	052165615X  |	3        |
|276729	|	521795028	  |6        |

table 11 rating data

From table 11 we know that our dataset is 1048575 and has 3 variables namely UserID, ISBN, and rating


### Data Preparation

**1. Encode the variables 'user' and 'ISBN' into an integer index**

- Changed 'userID' and 'ISBN' to list without the same value using tolist() function
- Encode 'userID' and 'ISBN' using enumerate() function
- Perform the process of encoding numbers to 'userID' and 'ISBN' enumerate()

**2. Map userID and ISBN to related dataframes**

- Mapping userID to user dataframe using map() function
- ISBN mapping to book dataframe using map() function

**3. Check the amount of data and change the rating value**

Check the amount of data such as the number of users, the number of books, then change the rating value to float.
```
95513
322473
Number of Users: 95513, Number of books: 322473, Min Rating: 0.0, Max Rating: 10.0
```
From the output above we can find out the number of users is 95513 and the number of books is 322473

**4. Randomize datasets**

This process is done so that the data distribution becomes random

**5. Sharing datasets**

In this stage, we will first map user and book data into one value, then create rating data on a scale of 0 to 1 so that it is easy to carry out the training process. Finally, we can divide our dataset into train and validation data with a composition of 80:20. The division of this dataset aims to make it easier for us in the process of evaluating model performance

### Modeling

**Calculating Match Score**

At this stage, the model calculates the match score between the user and the book using the embedding technique.
First, we embed the user and book data. Apart from that, we can also add biases for each user and book.

The line of code below is the process for adding a bias for each book with the Embedding() function.

Then we call the embedding layer that was created before. Finally use the sigmoid activation function to set the match score set on a [0,1] scale

**Model compilation**

This model uses Binary Crossentropy to calculate the loss function, Adam (Adaptive Moment Estimation) as the optimizer, and root mean squared error (RMSE) as evaluation metrics.

**Starting training**

Conduct model training with the fit() function, initialize the x and y variables to retrieve the train data, then set epoch = 10, and batch_size = 128 so that the training process runs quickly


### Getting Book Recommendations

To get book recommendations, first we take a random sample of users and define the book_not_read variable, which is a list of books that have never been read. This sample will be a book that we recommend to readers.

Previously, readers have rated several books that have been read before, we will use this rating to make book recommendations to readers.

To get book recommendations, use the model.predict() function.

Table 12 is a book recommendation for users with ID 224349

Showing recommendations for users: 224349
books with high ratings from user

Top 10 books recommendation

|Book Title                                                     |Book Author          |
|---------------------------------------------------------------|----------------------|
|Harry Potter and the Chamber of Secrets (Book 2)                 | J. K. Rowling       |              
|Harry Potter and the Sorcerer's Stone (Harry Potter (Paperback)) |J. K. Rowling       |       
|Where the Heart Is (Oprah's Book Club (Paperback))                |Billie Letts        |             
|Life of Pi                                                      | Yann Martel        |
|The Notebook                                                      | Nicholas Sparks   |                                 
|Bridget Jones's Diary                                            | Helen Fielding   | 
|The Pilot's Wife : A Novel                                       | Anita Shreve    | 
|The Summons                                                      | John Grisham    | 
|Summer Sisters                                                    | Judy Blume     | 
|The Firm                                                         | John Grisham    | 

Table 12. Recommendations for 10 books

### Evaluation

The metric used for model evaluation is Root Mean Squared Error (RMSE). Root Mean Squared Error (RMSE) is a metric used in predictive accuracy whose calculation concept is similar to the MAE metric, but the squaring of errors results in more emphasis on errors than using the MAE metric [4]. In short, RMSE basically measures the mean squared error of our predictions.

Based on the results of the training model, the root_mean_squared_error value is 0.3949 and the val_root_mean_squared_error value is 0.4060. This score is good enough for our recommendation system.

Here is a visualization of metrics with plots
![Screenshot (478)](https://user-images.githubusercontent.com/86582130/192162604-78a85e19-a54d-48cd-b65c-11411ca8cae8.png)

Figure 4. Metrics visualization

## Conclusion

Based on the results of *development* the recommendation system uses 2 methods, namely Content-Based Filtering and Collaborative Filltering and evaluation using the RMSE metric for collaborative filtering and precision for content-based filtering, showing that these two methods can build recommendations well, indicated by the ability to recommend books based on authors and top rating

## Reference

[1] Ritdrix, A. H. & Wirawan, P. W., 2018. Sistem Rekomendasi Bukumenggunakan Metode Item-Basedcollaborative Filtering. *Jurnal Masyarakat Informatika*, 9(2), pp. 24-32.

[2]Devi, A. A. P. & Tonara, D. B., 2015. Rancang Bangun Recommender System dengan Menggunakan Metode Collaborative Filtering untuk Studi Kasus Tempat Kuliner di Surabaya. *JUISI*, 1(2), pp. 102-112.

[3]Mondi, R. H., Wijayanto, A. & Winarno, 2019. Recommendation System With Content-Based Filtering Method For Culinary Tourism In Mangan Application. *Jurnal Ilmiah Teknologi dan Informasi*, 8(2), pp. 65-72.

[4] M. Nilashi, K. Bagherifard, O. Ibrahim, H. Alizadeh, L. A. Nojeem, and N. Roozegar, 2018. Collaborative filtering recommender systems. *Research Journal of Applied Sciences, Engineering and Technology*, 5(16), pp. 4168–4182.



