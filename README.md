# MovieReviewsNLP

## Revealing Movie Review Trends Using NLP in Python

### Introduction
The rise of streaming services has caused a mountain of movie options and too many to choose from on-demand. As a result, movie reviews have become a necessary part of the movie-watching process. In this project, I look at how movies are judged and how that affects what I choose to watch. Specifically, I want to answer what makes a movie “bad” vs. “good” — are there common characteristics, praises, and criticisms across similarly rated movies?
This project is designed to analyze the information provided by a dataset that looks at Rotten Tomatoes, a review-aggregation website for film and television. This specific dataset is especially insightful due to the partitioning of the data into one file for individual reviews and another for individual films. With the results from our analysis, I hope to find correlations between rotten reviews and fresh reviews and the films they describe.

### Dataset
Our data, “Rotten Tomatoes movies and critic reviews dataset,” was posted by Stefano Leone on Kaggle. It features over 17,000 movies and their related critic reviews which were scraped directly from Rotten Tomatoes as of October 31, 2020. There are two datasets within the larger dataset, one referencing movies and the other referencing critics.
The critic reviews dataset is compiled where each row is a critic review while the columns are made up of the Rotten Tomatoes link, the name of the critic who rated the movie, whether the review was made by a top critic, the name of the publisher for which the critic works, whether the review was fresh or rotten, the review score provided by the critic, date of the review, and the content of the review itself.
Before moving forward with the analysis, I cleaned the data and got it into a corpus. I cleaned the critic review dataset and condensed it into only the review type (fresh vs. rotten), when the review was written, and the written review content. I originally intended on using the actual rating rather than just “rotten” and “fresh,” but the data for review ratings were unusable and not actually necessary for our analysis anyway. Then, I partitioned the data into two data frames with one data frame containing all of the rotten reviews and the other containing all of the fresh reviews.

### Methods
For our project, I conducted several different analyses including Text Analysis, TF-IDF, LDA topic modeling, and Sentiment Analysis. In order to do this, I imported the Python libraries Pandas, NumPy, scikit-learn, and the Natural Language Toolkit (nltk). Pandas was used to read the data in from a csv, clean the data, and in TF-IDF. I used NumPy for TF-IDF, LDA, and sentiment analysis. Scikit-learn for TF-IDF and sentiment analysis. And nltk for text analysis, TF-IDF, LDA, and sentiment analysis. I started by running frequency distribution for each of the three groupings (all of the reviews, fresh reviews, and rotten reviews) in order to display the 20 most frequent words within each corpus and see a quick overview of each corpus.
First, I performed text analysis. Using some of the words found from a frequency distribution, I then ran the functions “similar” and “common contexts”. The function “similar” was run to look at words that appear in similar contexts to a specific word and further the contextual analysis. Then, I ran common contexts to analyze common contexts between a list of words and the words that typically surround those words. I also ran lexical dispersion, lexical diversity, and collocations. The lexical dispersion plot was created in order to display the location of specific words throughout the corpus and each time that word is used. I then used lexical diversity to calculate the percentage of all of the words that are unique words. The last part of the text analysis I ran was collocations to select specific phrases that happen unusually often with the corpus and will most likely select specific film titles, directors, or actors.
Next, I utilized TF-IDF in order to find the most important words within each corpus of fresh and rotten reviews. This pointed to words that appear the most frequently within the corpora as inversely proportional to the number of documents in which they appear. This produced a list of ten terms with the highest TF-IDF scores for both rotten reviews and for fresh reviews.
Then, I used LDA, or Latent Dirichlet Allocation, for topic modeling. This model allowed us to allocate words to topics in the reviews. Doing so, allowed us to discover thematic information in both the “rotten” corpora and “fresh” corpora. In other words, I was able to see that each review covers a number of different topics, to different degrees. By analyzing this, I can get a sense of what the critics and audience reviewers value in a movie or dislike in a movie.
Finally, I used sentiment analysis to see if I can predict whether a given movie review is positive or negative. I ran both a rule-based classifier (using the VADER lexicon) and a learning-based model (using a Multinomial Naive Bayes model) to compare how the model type changed our ability to predict sentiment.

### Results

#### Text Analysis
Text Analysis allowed us to see an overview of the text as well as important meanings derived which I will build on later on when I do TF-IDF, LDA, and Sentiment Analysis. The first thing I examined was Frequency Distribution. An analysis of the frequency distribution of the fresh reviews concluded that what distinguishes fresh reviews from all of the general reviews corpora are the terms “great”, “funny”, “makes”, and “action”. An analysis of the frequency distribution of the rotten reviews concluded that the distinguishing terms included “would”, “bad”, “could”, and “feels”. All three data groupings featured the words film, “movie”, “one”, “like”, “story”, “much”, “even”, “good”, “comedy”, “time”, “make”, and “way”. The top 5 for each data corpus stayed roughly the same. The top 5 for both ‘all reviews’ and ‘fresh’ were the exact same featuring film, movie, one, like, and story. The rotten data like takes over the third spot pushing one to fourth and much moves up to the fifth spot pushing the story down to sixth.
Using this information, I moved on to using the similar and common contexts functions. I analyzed the word “film” using the function “similar”. After analyzing the similar terms of film I found that all three associate film with movie, story, comedy, picture, and script. Fresh reviews, in particular, highlighted the words "world", "treat", "heart", and "message". But I believe that a lot of the words that are associated with film do not have much significance. Words such as “sequel” and “gates” tend to be in a number of reviews with a negative tilt and humor tends to be seen in a lot of reviews with a positive tilt.
From the analysis with common contexts, it seems like film and movie have many common contexts that are true across all three or just two of the corpora. The fresh and rotten corpuses both have a lot of contexts that are not in the other. It appears that, according to this, a (film or movie) with, works, or about tend to be in positive reviews. However, the words the (film or movie) seems, itself, or looks tend to be associated with negative reviews.
From the lexical dispersion plots, I can analyze if some words have been used more over time than others. I can see that film and movie are both used a lot across all twenty years of reviews and there is not much difference between the users in the different corpora. The same thing applies to characters, actors, and, surprisingly, good. The main differences come in comedy and fun. Fun is used a lot less overall in the rotten corpus. Comedy is fairly evenly distributed across time in the fresh corpus. However, in rotten, comedy is concentrated in about four time periods within the lexical dispersion plots. This likely means that comedy comes in and out of style to describe a bad review, though it could also indicate times when comedy movies were perceived more negatively as a genre. In terms of Lexical Diversity, the “all reviews” corpus has a lower lexical diversity at 0.012 but one closer to the fresh corpus at 0.015 than the rotten corpus’s lexical diversity of 0.021. This means reviews are more creative with their word choice when writing a negative review compared to a positive one.
A lot of the collocations I see in the comments are understandably names of movies or actors/actresses. Collocations within the fresh review descriptions, based on this analysis, also include “special effects”, “feel like”, “romantic comedy”, “emotional immediacy”, and “worth seeing.” Some of the rotten review descriptions include “naughty roots”, “pretty much”, “worked better”, and “gross-out humor.”

#### TF-IDF
To allow for more insights regarding the characteristics contributing to the dichotomy of rotten and fresh reviews, I ran TF-IDF on the segmented corpora. After attempting to run Professor Lalor’s tfidf_vectorizer function on each full corpus of 300,000+ reviews, our Colab session failed due to exceeding its allocated RAM. I attempted to reduce the review count by pulling only reviews from 2015, 2016, etc. through 2019, but I still encountered the same RAM failure and session restart on each iteration. Finally, I settled on pulling a sample of 25,000 reviews from the full clean data frame before segmenting this sample into two fresh and rotten corpora. This allowed us to successfully run the tfidf_vectorizer given the computational limitations while maintaining a representative sample of the initial dataset.
Pulling the top ten TF-IDF scoring terms from each corpus provided some insight into what were the most important terms within fresh and rotten reviews. I see that the term for fresh reviews is “99,” likely referring to the movie’s numerical rating on the site (out of 100). Other notable top terms are “holding,” “breath,” “felt,” and “suspense,” pointing to the encompassing emotional experience that reviewers crave when they go to a movie theater.
In contrast, looking at the top-ranked words for the rotten corpus, I see “courant,” likely referring to films that fail to be au courant or “with the times.” A trend emerges in the top ten words for rotten reviews with terms like “nihilistic,” “deranged,” “savage,” and “killer.” This suggests that movies with particularly violent or aggressive depictions might be penalized in reviews.

#### LDA
LDA analysis allowed us to see the degree to which different topics were discussed within the critic and audience reviews. By breaking the data into two different corpora, fresh and rotten, I was able to see the topics associated with these reviews. Each corpus was broken into 5 different topics, and the top 5 words from each topic were extracted. Essentially, each topic provides a major reason why reviews are positive or negative.
In analyzing the “fresh” corpora, or movies that were highly rated, many of the top categories included positive words like “best”, “great”, “like” and “love”. In addition, top words included “story”, “characters”, “performance”, “entertaining”, and “funny” (Appendix, Figure 1). This can tell us that audiences value movies and rate them higher if they have good storylines, characters, etc.
On the other hand, the “rotten” corpora, or movies that were rated poorly, included words like “narrative”, “comedy”, “acting” and “features”. Additionally, they included words like “better”, “easily”, “early” and “left” (Appendix, Figure 2). This allows us to infer that these movies had poor narratives, acting, or comedy, and hence many of those characteristics could have been “better”. Or, words like “left” may suggest the audience thought things were left out of the movie, or worse, that they left the movie early. Ultimately, LDA topic modeling allows us to analyze the movie reviews in order to give insight into what the audience does and does not like.
Sentiment Analysis
The main goal of Sentiment Analysis was to see if I could identify whether a given review was positive or negative, or in the case of our data, fresh or rotten. I saw fairly successful results, with an accuracy of around 80% when using our learning model and 63.7% when using the VADER lexicon rules. Scoring worse on a rules-based model makes sense since many reviews might use negative words in a positive context (for example: “that horror movie was distressingly violent and dark” has a lot of negative words but may very well be a positive review). Essentially, the rules-based model had difficulty understanding the complexity of sarcasm present in many movie reviews.
A closer look at our accuracy for the learning model (Appendix, Figure 12) indicates I misclassify positive reviews as negative at a much higher rate than I classify negative reviews as positive. This could be for a number of reasons including higher levels of sarcasm, shorter reviews, and less extreme positivity (vs. negativity for negative reviews) among positive reviews.
Ultimately, I found that I could identify the sentiment of reviews with pretty good accuracy.
Future Work
The dataset I used for this project has a lot of potential for future research as well. On the higher-end side of things, I could use analysis of these reviews to identify what makes an ideal movie — a box office hit — and write a script that captures those key qualities. While I worked on a super high level (good vs. bad), looking more closely at the data and doing similar analysis across different genres, years, and even directors would provide more interesting insight into the movie industry. I also see potential to use the dataset’s “top critic” column to see if top critics write similar reviews to those of the public (are they more critical, use different words, focus on different themes, etc.).

### Conclusion
Throughout this project, I focused on one simple yet broad and important question: what makes a movie good? And I think I have a better understanding of that now. Good movies make us feel something — they connect with us emotionally. They have us holding our breath in suspense, excited for and surprised by what comes next. Audiences love a good story, well-written characters, and a performance — they appreciate a well-done special effect. Bad movies are wild, nihilistic, and disorganized. They are missing an interesting plot, good acting, or falling flat with their jokes. They may come across as tongue in cheek, insincere, or flat-out distasteful. While I may not know the exact formula for a good or bad movie, I do know it is some combination of plot, acting, comedy or drama, suspense, and entertainment value. And that’s exactly what makes movies so great — I love being surprised by a movie.
 
  
