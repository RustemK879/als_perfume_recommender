# perfume_recommender
**This project is under construction (some files are messier than they should be - okay to use for general reference but i will clean them later). General goal - create a recommender system for fragnances (WOMEN ONLY) for the collected data by using Item-item CF and explicit user ratings.**

The motivation: Perfumes are one of the greatest and yet risky gifts. They are great because, as humans, we tend to associate many things with fragnances and such gift is usually viewed as personalized (and even intimate). They are risky, since each person has a unique taste - finding the 'right one' can be really painful and often is as hard as finding a needle in the haystack. 

My mother loves perfumes - I really wanted to gift her one but there are two problems:
1) I am no expert at choosing perfumes;
2) I am allergic to perfumes.

However, I have an access to the perfumes that she likes. I still want to surprise her - what are the possible solutions?

**First solution (brute force):**
1) Go to the perfume store, ask an assistant to recommend 10 perfumes and rank them from most to least likely;
2) Go to another perfume store, do the same;
3) Wait for some time (1 week, month, year), start from the store 1 again -> if the assistant has changed, do step 1 again, otherwise go to another store etc;
4) Repeat this for 5000 times and get 10 of the most recommended items through all iterations;
5) Select the first item and gift.

**Second solution (reviews):**
1) See the most popular perfumes across multiple stores based on online reviews;
2) Compute the most popular and buy.

**Third solution (reviews + math):**
1) Scrape the data on users and reviews from website and construct a user-item matrix;
2) Using the knowledge on matrix factorization, approximate the initial matrix and make relevant predictions.

**While the above part is mostly comical, I chose to use the third solution.**

Using fragrancenet.com as a reference point, I scrape user reviews and perfume info from the website into a dataset. I then clean this dataset, do EDA and use this dataset to derive perfume recommendations for the unseen users. 


**Then, the model is wrapped into a telegram bot -> user rates perfumes from the given list -> gets 10 recommendations in return.**

Essentialy, this project is equivalent to creating a recommendation system for a particular website.

**merged_after_nlp.csv contains the dataset used to construct a recommender - it has already been cleaned and the missing reviews were restored.**

**Problems that I have faced so far:**
1) To access the reviews, we need to click buttons on the website - **solved with Selenium**
2) That website doesn't like to be scraped - if you use vanilla Selenium, you will consistently get captcha on the first click - **Solved with UC (undetected chromedriver)**
3) 9000+ items to scrape - **Optimized scraping - scrape items with at least one review** - now we are down to ~2300
4) Scraping takes ~ 8 hours - **Solved by batching links on multiple machines and optimizing scraping code - down to 1:30-2 hours**
5) Some parts of the data did not scrape correctly - **solved during data cleaning**
6) Data has some non-perfume items and anonymous reviews - **solved during data cleaning**
7) 2500 Reviews do not have star rating but have text reviews - **Solved with transfer learning, simple transformers (RoBERTa)**
8) Star ratings are heavily skewed towards 5 - RoBERTa learns to predict any review as 5-star - **solved by calibrating weights in multiclass**
9) Most of the users leave only one review (87%) - **solved by using hybrid recommender** (no incentive to leave reviews, business problem)

