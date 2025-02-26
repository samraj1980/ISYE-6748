#                                   Content Based Recommendation Engine for Health Care Providers 

[![](https://img.shields.io/badge/authors-%40Sam%20Raj-blue)](https://www.linkedin.com/in/sam-raj-anand-jeyachandran-7b273a6/)
[![](https://img.shields.io/badge/authors-%40Minkyu%20Choi-blue)]
[![](https://img.shields.io/badge/authors-%40Shubhankgi%20Waghere-blue)]

## Abstract
A recent evolution of big data technology enables people to handle huge amounts of data which have been collected in healthcare databases representing patients' health states (e.g., as laboratory results, treatment plans, medical reports). Hence, digital information available for patient-oriented decision-making has increased drastically but is often scattered across different data sources. Yet, expert-oriented language, complex interrelations of medical facts and information overload in general pose major obstacles for patients to understand their own record and to draw adequate conclusions. 
In this context, recommendation systems may supply patients as well as physicians with additional laymen-friendly information helping to better comprehend their health status and get answers to commonly known topics. However, such systems must be adapted to cope with the specific requirements in the health domain in order to deliver highly relevant information for physicians. They are referred to as recommendation systems. 


## 1.0 Introduction
InpharmD is a service provider for doctors and pharmacists (healthcare providers) to have better access to better health information, so they can make better decisions. The primary benefit of the service is connecting questions from healthcare providers to customized, evidence-based responses from a nationwide network of academic drug information centers, optimized with advanced machine learning algorithms and search mechanisms. In this project, our team has built a recommendation system to provide meaningful medical articles and references to questions of healthcare providers.


## 2.0 Objective
The goal of a recommendation system is to supply InpharmD users with medical information, which is meant to be highly relevant to the question the healthcare providers are looking for. By Implementing this recommendation system, InpharmD is looking to provide its user base with highly relevant medical information in real time based on similar past responses w/o having to wait for a research analyst to analyze and get back to users after a few hours or days. This will provide a quicker turnaround time, enhance user experience and free up the time of the analysts. 

Depending on the questions, matching healthcare documents in InpharmD would extracted from the repository and rendered in the Web application real time. The system in the backend would execute AI NLP based models, rank existing documents based on similarity to the posed question, and display in a format, which is comprehensible to the physician or Pharmacist.

Data entries in a recommendation database (DB) constitute the medical research documents from InpharmD source database. Such items originate from InpharmD health knowledge repositories and will be displayed as recommendations when a user is about to post a new question online. 

Thus, it is possible to compute and deliver potentially relevant information documents from the recommendation system leveraging the breakthrough innovations in the field of Natural Language processing algorithms. Based on the similarity of the new question with those already present in the repository, the top ‘n’ relevant documents can be rendered.


## 3.0 System Architecture
In this project, we are going to use various content-based recommendation algorithms to come up with recommendations. Based on the results from the three algorithms, we will be evaluating and choosing the model, which renders the best recommendation. We will also be exploring an ensemble approach to determine the common recommendations for the specific question across all the trained models.

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_1.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 1: System Architecture       </a>
        </div>
   </td>
  </tr>
</table>

## 4.0 Data Engineering 

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_2.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 2: Data Engineering Flow     </a>
        </div>
   </td>
  </tr>
</table>

### 4.1 DATA SOURCE
InpharmD provided a Json extract from their Medical Repository, which is stored in heroku cloud. This repository contains all the past answered questions posed by health care providers. The Json extract contains about 5,223 documents which is the data we will be using for this project


### 4.2 DATA EXPLORATION 
Please refer to Appendix A where the data attributes and a sample record from the raw file is displayed for reference. 

### 4.3 DATA MINING
After initial analysis of data, we found that there are multiple documents, which were test documents and did not contain any relevant information. We wanted to filter them off before proceeding further with data wrangling. Removed all the documents whose title element has no data. Post this step the number of documents to be considered for further processing dropped down to 4,167 documents. 
We also found that the information that is really required to train the models are present in the following metadata elements. 
1.	id
2.	question
3.	title
4.	custom_response_text
5.	view_everyone
6.	shareable_url
Except for the above-mentioned elements, the rest of the elements dropped. 

### 4.4 DATA WRANGLING 
Before doing feature engineering, the data had to be transformed to a usable format. The following sequence of steps were performed. 
1.	New metadata element called ‘document’ was created and it is essentially a concatenation of title, question, custom response text and background. 
2.	On the document metadata, the following operations were performed. 
a.	Removed all URLs
b.	Removed all punctuations
c.	Converted the entire text to lowercase
d.	Removed all English & Spanish stopwords
e.	Eliminated a set of repeating words which did not add any value 
f.	Normalized some of the keywords as they were represented in different ways (eg: covid19, covid-19, covid, etc) 
g.	Removed words with less than 2 characters 
h.	Removed New line characters
i.	Removed Non printable characters
j.	Removed words with only numbers
k.	Removed all non-alpha-numeric characters in string 

### 4.5 DATA VISUALIZATION 
Word cloud result after Data Wrangling and optimization (removal of non-relevant or conflicting words) was created. 

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_3.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 3: Word cloud post Data Wrangling     </a>
        </div>
   </td>
  </tr>
</table>

Top 20 most commonly occurred word pairs (Two Gram) were also analyzed

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_4.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 4: Frequently occurring word pairs       </a>
        </div>
   </td>
  </tr>
</table>

## 5.0 Feature Engineering 
The entire corpus of documents was vectorized using the Term Frequency Inverse Document frequency (TFIDF) vectorizer.  This essentially converts a collection of raw documents to a matrix of TF-IDF features. 

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_5.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 5: TF-IDF workings       </a>
        </div>
   </td>
  </tr>
</table>

The output from the TFIDF vectorizer is a sparse matrix where each row is a document, the features or columns are the words occurring in them, and the value is the TF-IDF weights assigned to these words. The total number of features that were derived from this step was 41,683.

## 6.0 MODEL ENGINEERING 

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_6.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 6: Model Lifecycle        </a>
        </div>
   </td>
  </tr>
</table>

### 6.1 COSINE SIMILARITY ALGORITHM
Cosine similarity is a metric used to determine how similar the documents are irrespective of their size. Consider a new user question; we first transform/Vectorize the user question using the same trained TF-IDF vectorizer. 
We then calculate the cosine of the angle between two vectors projected in a multi-dimensional space. In this case, the angle between the vectorized user question and each of the vectors (documents) in the sparse matrix is calculated. The smallest cosine angles are extracted and those represent the documents, which is very similar to the user question.  

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_7.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 7: Cosine Similarity        </a>
        </div>
   </td>
  </tr>
</table>

### 6.2 K-NEAREST NEIGHBORS (KNN)
The trained TF-IDF vectorizer is used to train the K-nearest neighbor’s algorithm. The user question is first transformed/vectorized using the trained TF-IDF vectorizer. This vector is then fed to the trained KNN algorithm and the closest neighbors are calculated by measuring the Euclidean distance. The documents with the smallest Euclidean distances are then rendered as recommendations to the user.

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_8.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 8: KNN visualization of Documents in the Corpus       </a>
        </div>
   </td>
  </tr>
</table>

### 6.3 TOPIC MODELING (LDA)
Topic modeling is an unsupervised machine learning modeling approach, which can scan a set of documents, detect words, and phrase patterns. The power of this approach is automatic clustering word group/similar expressions that best characterize a set of documents. Algorithm used for this approach is Latent Dirichlet Algorithm (LDA)
The entire corpus of documents was segmented into topics and the number of topics was adjusted based on the coherence score, which is a method for hyper tuning the model. 

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_9.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 9: Topics Segmented by LDA Algorithm        </a>
        </div>
   </td>
  </tr>
  <tr> 
  </tr> 
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_10.png">
    </td>
  </tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 10: Documents tagged to topics in the corpus     </a>
        </div>
   </td>
  
</table>

After the documents are segmented by topic, we can feed the vectorized user question as input to the trained LDA model to get the topic this question is assigned to and then the cosine similarity between the vectorized user question and the subset of corpus which are associated to the same topic is performed to get the documents similar to the user question. 

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_11.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 11: Topic distribution for a User question         </a>
        </div>
   </td>
  </tr>
</table>

### 6.4 TRUNCATED SVD
Truncated Singular Value Decomposition (SVD) is a matrix factorization technique that factors a matrix M into the three matrices U, Σ, and V. This is very similar to PCA, except that the factorization for SVD is done on the data matrix, whereas for PCA, the factorization is done on the covariance matrix. The truncated SVD works well on sparse matrix and gives us the ability to control the number of features or dimensions we want this to be reduced. 
In this case, the total number of features decreased from the original 41,683 to a fraction of about 3000 dimensions. We can feed the vectorized user question as input to the trained Truncated SVD model to get the transformed vector with only 3000 dimensions. Finally, we can apply the cosine similarity between the vectorized user question and the corpus to get the documents similar to the user question.

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_12.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 12: Determine Optimal Number of Dimensions in TruncSVD         </a>
        </div>
   </td>
  </tr>
</table>


## 7.0 EXPERIMENTS

### 7.1 SIMILARITY 
There are different approaches on how to calculate a similarity between two vectors or matrices. Regardless of the models, either content-based filtering or collaborative filtering algorithms, we need to use similarity measures to find how equal two vectors of users or items are in between them. It will be the distance between vectors. 

#### 7.2.1 Cosine Similarity
According to Wikipedia, “Cosine similarity is a measure of similarity between two non-zero vectors of an inner product space it is defined to equal the cosine of the angle between them, which is also the same as the inner production of the same vectors normalized to both have length 1.” In our experiment, we’ve seen that cosine similarity works better than Jenson-Shannon because the vector/matrix needs to be  probability distributions. However, the feature matrix and question matrix are more geometrical.
#### 7.2.2 Jenson-Shannon
According to Wikipedia, “the Jensen–Shannon divergence is a method of measuring the similarity between two probability distributions. The square root of the Jensen–Shannon divergence is a metric often referred to as Jensen-Shannon distance.” We used it for our LDA model. However, it couldn’t beat the Cosine similarity. The Densities of two matrices are too sparse and they were not meant to be probability distributions. 

### 7.2 COSINE SIMILARITY & TOPIC MODELING (LDA)
Firstly, we’d like to build a based model with a simple algorithm, which is TF-IDF & Cos. Similarity. This is a supervised, non-parametric machine learning approach, which uses the similarity scores to determine the class to which the new point belong. In the context of NLP based recommendation, we can use this approach to find K documents, which are similar to the new one and render them as recommendations based on similarity of content. Involves two steps: 
a.	Vectorize each document in the corpus using Term Frequency Inverse Document frequency (TFIDF) and perform a dimensionality reduction to retain only significant words / features.
b.	Leverage Cosine similarity function to find the K documents (vectors) which are most similar to the one being examined. 

As a similar approach, we developed a Topic modeling with LDA model. Data should be processed in similar ways. However, we applied LDA to assign topics to each document. Unlike, we query a question against an entire document. With LDA, we first assign a topic to question and then search for similar documents among the topics. In other words, we use an additional feature, a topic. It’s hard to say one is better than the other, but we could have similar results from both models. 

### 7.3 MATRIX FACTORIZATION APPROACH (TRUNCATED SVD)

Matrix factorization is a class of collaborative filtering algorithms used in recommendation systems. Matrix factorization algorithms work by decomposing the user-item interaction matrix into the product of two lower dimensionality rectangular matrices.

The idea behind matrix factorization is to represent users and items in a lower dimensional latent space. We used truncated SVD for this approach.
Algorithms used are:
●	Singular value decomposition (SVD)
●	Stochastic gradient descent (SGD)
●	Alternating least squares (ALS)

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_13.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 13: Workings of Matrix Factorization         </a>
        </div>
   </td>
  </tr>
</table>

## 8.0 MODEL EVALUATION AND TESTING

Defining evaluation, Metrics for Recommendation Systems is a very important step, which is going to decide the quality of the results being produced and validating those results. For a content-based approach, attributes of the question and the responses are important from the documents. To find similarity between responses we will have to consider all the related data attributes from the data.
Furthermore, we can extract features like sentiment score and tf-idf scores from responses and questions. The tf-idf score of a word reflects how important a word is to a document in a collection of documents. 
Secondly, to compute similarity between question and the responses, we simply take the cosine similarity between the question vector and the response vector. We are also going to explore a model-based approach and matrix factorization model is the best example of that approach. High-level plan is to create the utility matrix using matrix decomposition techniques and calculate the prediction rating. Using the calculated prediction rate, we decide which documents best answer the given question.

## 9.0 RECOMMENDATION WEB APPLICATION

The InpharmD recommendation web application enables a user to enter the new user query and choose the recommendation algorithm to be used for rendering the closest recommendations.
This web application is built on python-flask-jinja2 technology stack and it invokes a restful Web API built on Python-flask technology, which uses the trained ML models to render the recommendation results to the user portal. 

A. Link to the Rest API: 
 https://inpharmd.pythonanywhere.com/recommendation?method=01&question=??&username=??&password=??

B. Link to the Web Application: 
https://inpharmdwebapp.pythonanywhere.com/


<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/Screenshot_14.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 14: Recommendation Web Application         </a>
        </div>
   </td>
  </tr>
</table>

## 10. CONCLUSION
Overall, tested various different models to provide a recommendation based on a question provided by healthcare workers. It’s hard to tell which model outperforms others because the results are relatively similar to each other. However, it generates acceptable recommendations from users and a CTO of InpharmD verified its usefulness. 

To make a more reliable recommendation system, first, we need more data to train a model. Also, we need to spend more time wrangling our data. It was unclear which word we should remove or not because there are domain specific languages in documents. If we clean the data with a regular approach, we might lose some very important medical words. 

To sum up, In this project, we leveraged the documents from the InpharmD data to train various ML algorithms and then use the same to render recommendations for a new user query based on similarity metrics. 

### 11. REFERENCES
1. Comparing Twitter and Traditional Media Using Topic Models, European Conference on
Information RetrievalECIR 2011: Advances in Information Retrieval pp 338-349, Wayne
Xin ZhaoJing Jiang, Jianshu Weng, Jing He, Ee-Peng Lim, Hongfei Yan, Xiaoming Li

2. Latent Dirichlet Allocation David M. Blei, Andrew Y. Ng, Michael I. Jordan; 3 (Jan): 993-1022,2003.

3. Probabilistic author-topic models for information discovery::M Steyvers, P Smyth, M Rosen-Zvi… - Publication: KDD '04: Proceedings of the tenth ACM SIGKDD international conference on Knowledge discovery and data mining. August 2004 Pages 306 315 https://doi.org/10.1145/1014052.1014087

4. Empirical Study of Topic Modeling in Twitter :Liangjie Hong and Brian D. Davison Publication: SOMA '10: Proceedings of the First Workshop on Social Media Analytics July
2010 Pages 80–88 https://doi.org/10.1145/1964858.1964870

5. https://nlp.stanford.edu/IR-book/html/htmledition/latent-semantic-indexing-1.html

6. An Introduction to Latent Semantic Analysis: Thomas K Landauer Department of Psychology University of Colorado at Boulder, Peter W. Foltz Department of Psychology New Mexico State University Darrell Laham Department of Psychology University of Colorado at Boulder, Landauer, T.K., Foltz, P. W., & Laham, D. (1998). Introduction to Latent Semantic Analysis. Discourse Processes, 25, 259-284.

7.https://www.analyticssteps.com/blogs/introduction-latent-semantic-analysis-lsa-and-latentdirichlet-allocation-lda

8. Nonnegative matrix factorization for interactive topic modeling and document clustering Da
Kuang and Jaegul Choo and Haesun Park 

9. Spatial Aggregation Facilitates Discovery of Spatial Topics: Aniruddha Maiti Temple University Philadelphia, PA-19122, USA aniruddha.maiti@temple.edu Slobodan Vucetic Temple University Philadelphia, PA-19122, USA vucetic@temple.edu
10. Performance Analysis of Topic Modeling Algorithms for news articles: T.Rajasundari1, P.Subathra2, P.N.Kumar3. Amrita University, India

## APPENDIX A 
### Network Analysis of Documents Using Gephi 
We analyzed the entire corpus of documents provided by InpharmD from a network analysis standpoint. After creating the adjacent matrix fed to Gephi and allowed the system to form communities using modularity. 

<table>
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/screenshot_15.png">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 15: Document Communities visualized using Gephi      </a>
        </div>
   </td>
  </tr>
  <tr>
  </tr> 
  <tr>
     <td>
      <img src="https://github.com/samraj1980/ISYE-6748/blob/main/Images/screenshot_16.bmp">
    </td>
  </tr>
  <tr>
  <td>
        <div class="text-purple">
          <a href="#" class="text-inherit">       Fig 16: Analyzing a specific community and related documents    </a>
        </div>
   </td>
  </tr>
</table>




