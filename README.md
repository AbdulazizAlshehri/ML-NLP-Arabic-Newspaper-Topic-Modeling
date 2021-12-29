# ML-NLP-Arabic-Newspaper-Topic-Modeling

# Table of Content
- [Code - with Stemming](https://github.com/AbdulazizAlshehri/ML-NLP-Arabic-Newspaper-Topic-Modeling/blob/main/LDA_Stemming.ipynb)
- [Code - without Stemming](https://github.com/AbdulazizAlshehri/ML-NLP-Arabic-Newspaper-Topic-Modeling/blob/main/LDA_NoStemming.ipynb)
- [Data Sample](https://github.com/AbdulazizAlshehri/ML-NLP-Arabic-Newspaper-Topic-Modeling/tree/main/Data)
- [Presentation](https://github.com/AbdulazizAlshehri/ML-NLP-Arabic-Newspaper-Topic-Modeling/blob/main/Presentation/NLP%20Presentation.pdf)

# About
The idea of this project is to apply topic modeling on Arabic newspaper articles, that can be used to build a predictive model to be applied on different sources of data such as social media, users comments. The advantage of using newspaper articles is that it has a good coverage of general topics: politics, local news, sport, economy, etc. Morover, it gurantees a good distribution of topics. <br>

In this project we applied Latent Dirichlet Allocation method to group documents per topic.Furthermore, a comparison between word stemming, and different word vectorizer was done.

![](Graphs/about.JPG?raw=true "")


# Dataset:
Over 300k articles (1.3 GB) scrapped from Alriyadh.com, a famous local newspaper in Saudi Arabia. The articles were scrapped along with thier meta data such as: Title, Genre, Sub category, Author, Publish date,, etc.


# Pipeline:
![pipeline](Graphs/pipeline.JPG?raw=true "Pipeline")

# Cleaning:

### 1) Removing short and long articles:
It was observed that some of the records contains a short comment, or tweet like articles. while other contains a very long list of names such as people enlisted in goverment employment. <br>
Therefore, we've filtered articles that only contains 150 to 500 words to ensure topics modeling was done on similar sized documents.

![](Graphs/cleaning_articles_(before).png?raw=true "")

![](Graphs/cleaning_articles_(after).png?raw=true "")

### 2) Removing special characters:
Some html encoded special characters were removed from the dataset. 

![preprocessing_special_character](Graphs/preprocessing_special_character.JPG?raw=true "preprocessing_special_character")


### 3) Non-Arabic alphabet characters:
Numbers, qoutes, parenthesis was also removed from the dataset

![preprocessing_non_arabic_alphabit](Graphs/preprocessing_non_arabic_alphabit.JPG?raw=true "preprocessing_non_arabic_alphabit")


### 4) Reducing unnecessary words variations:
Formal arabic contains different characters that give it more specific pronounciation, and usually skipped when written informally. this include words such as "أحمد" and "احمد"
"وزارة" and "وزاره" 
<br> some of these letters variations were captured and replaced with one prominent letter. 

![preprocessing_reduce_variation](Graphs/preprocessing_reduce_variation.JPG?raw=true "preprocessing_reduce_variation")

# Challenges:
### 1) Arabic encoding problem:
during modelling we realized that there are different encoding for the same letter, specially hamza "ء"  and "أ". where the first was considered a puntuation and was replaced by a space during tokenization.

![challenge_01](Graphs/challenge_01.JPG?raw=true "challenge_01")

### 2) Arabic NLTK stop words:
Arabic NLTK stop words list was limited, and does not include variations such as "قد" , "وقد". <br>
we iteratively added more stop words, to exclude it from topics.

![challenge_02](Graphs/challenge_02.JPG?raw=true "challenge_02")

### 3) Memory issues:
LDA and preprocessing takes up a lot of Ram memory. Although LDA algorithm can be configured to optimize memory, we found that using higher spec PC, or apach spark would be a better option to quickly experiment different configurations.<br>

![RAM_Usage](Graphs/RAM_Usage.png?raw=true "RAM_Usage")<br>


# Number of Topics:
Coherence measure was used to identify the best number of topics. It measures the degree of semantic similarity between topics. As shown in graph below 40 topics had the highest coherence.<br>
![best_number_of_topics](Graphs/best_number_of_topics.png?raw=true "best_number_of_topics")


# Comparing Steming and without stemming:
Unlike other languages, Stemming in Arabic does not follow specific process, hence it does not produce a good result. 
<br> below is a sample of resulted topics with stemming. <br>
![Stemming](Graphs/stemming.JPG?raw=true "Stemming")<br>
Although it had a higher coherence value of 0.58 than without stemming version 0.46, The results are difficult to interpret and can mean different things. such as "نسب" which could be derived from  "نسبه" or "النسب"

# Results:
![results](Graphs/results.JPG?raw=true "results")
<br>
from selected topics, we could easily tell the meaning of the topic. However, some of LDA topics are of the same general topic. Further words examining can tell us more if they are the same topic, or a sub category of topic.<br>
for example topics 3, and 26 are both talking about energy. Nevertheless, the first is more related to Energy - Geopolitical, while the other is more to Energy- Economic (pricing,Markets).


</br>
This project was done to fulfill SDAIA T5 Bootcamp requirements.

*Contributors [Ibrahim Alzuhairi](https://github.com/ibalzuhairi) and [Abdulaziz Alshehri](https://github.com/AbdulazizAlshehri)*

