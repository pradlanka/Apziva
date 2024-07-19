# Potential Talents
# Background
The goal of this project is to develop a machine learning-powered pipeline to identify and rank potential candidates based on their fitness for a given role. The system should adapt and improve its rankings through supervisory signals provided by starring candidates, allowing the re-ranking of the candidate list based on real-time feedback.

## Data Description
The dataset consists of anonymized candidate profiles from our sourcing efforts, each identified by a unique numeric ID. Key attributes include the candidate's job title, geographical location, and number of connections (with 500+ indicating over 500 connections). The target variable is a fitness score, representing the candidate's suitability for the role on a scale from 0 to 1. The dataset also includes keywords relevant to the roles being filled, such as "Aspiring human resources" or "seeking human resources." Data is available [here](https://docs.google.com/spreadsheets/d/117X6i53dKiO7w6kuA1g1TpdTlv1173h_dPlJt5cNNMU/).

| job_title                                                                         | location                              | connections | fit |
|-----------------------------------------------------------------------------------|---------------------------------------|-------------|-----|
| 2019 C.T. Bauer College of Business Graduate (...                                 | Houston, Texas                        | 85          | NaN |
| Native English Teacher at EPIK (English Program...                                | Kanada                                | 500+        | NaN |
| Aspiring Human Resources Professional                                             | Raleigh-Durham, North Carolina Area   | 44          | NaN |
| People Development Coordinator at Ryan                                            | Denton, Texas                         | 500+        | NaN |
| Advisory Board Member at Celal Bayar University                                   | İzmir, Türkiye                        | 500+        | NaN |

# Methodology
### Fit score calculation
To estimate the fit score, we match or identify similar words between the search term and candidate job title from their profile. Additionally, location information and connections are used to enhance the ranking. Here's the detailed process:

1. **Text Processing**: 
   - **Tokenization and Stemming**: We use SpaCy to tokenize and stem the words in the job titles.
   - **Vectorization**: We use a TF-IDF vectorizer to vectorize the words.
   - **Similarity Calculation**: Similarity between the search term and candidate job title is calculated using Jaccard similarity or cosine transformation.

2. **Connections Encoding**:
   - We encode the number of connections into ordinal classes:
     - <100: 0 (low)
     - 101-200: 1 (medium)
     - 201-500: 2 (high)
     - 500+: 3 (very-high)

3. **Location Matching**:
   - We extract city, state, and country information from the 'location' column.
   - Matching scores are assigned as follows:
     - Country match: 1
     - State match: 2
     - City match: 3
     - No match: 0

4. **Similarity Approaches**:
   - **Word Matching**: Calculate similarity through Jaccard similarity based on matching or similar words between "search term keywords" and "job profile".
   - **N-Gram Models**: Use N-grams vectorized with TF-IDF and calculate similarity using cosine transformation, incorporating location and connection scores.
   - **Word Embeddings**: Utilize Word2Vec from Gensim to calculate pairwise cosine similarity of word embeddings. The mean of the top 20% pairwise cosine values is used to account for semantic similarity.
   - **Sentence Embeddings**: Use Sentence-BERT (SBERT) to generate sentence-level representations and calculate similarity scores using cosine similarity.

5. **Integration of Scores**:
   Combine cosine similarity scores with location and connection scores using a weighted sum to determine each candidate's overall fit.
   - Rank candidates in descending order of fit scores.

The SBERT model provided the most relevant results, correctly ranking candidates based on the search keywords, ensuring that top results closely matched the desired criteria, such as location and job relevance.

| CandidateID | job_title                                                                      | location                     | connections | fit      |
|-------------|--------------------------------------------------------------------------------|------------------------------|-------------|----------|
| 7           | HR Senior Specialist                                                          | San Francisco Bay Area       | 500+        | 1.907255 |
| 23          | Nortia Staffing is seeking Human Resources, Pa...                             | San Jose, California         | 500+        | 1.875520 |
| 31          | HR Manager at Endemol Shine North America                                     | Los Angeles, California      | 268         | 1.690532 |
| 11          | Human Resources Coordinator at InterContinenta...                             | Atlanta, Georgia             | 500+        | 1.542001 |
| 26          | Human Resources Generalist at Schwan's                                        | Amerika Birleşik Devletleri  | 500+        | 1.452684 |
| 36          | Human Resources Management Major                                              | Milpitas, California         | 18          | 1.409303 |
| 3           | People Development Coordinator at Ryan                                        | Denton, Texas                | 500+        | 1.389546 |
| 32          | Human Resources professional for the world lea...                             | Highland, California         | 50          | 1.316694 |
| 13          | Seeking Human Resources Opportunities                                         | Chicago, Illinois            | 390         | 1.311758 |
| 52          | Director Of Administration at Excellence Logging                              | Katy, Texas                  | 500+        | 1.268445 |


### Updating ranking based on starred candidates
If any profile is starred, we update the reference search vector as a weighted sum of the keywords search vector and the starred candidate's job title vector. This adjustment increases the cosine similarity between the updated reference search vector and similar candidates. We use a parameter alpha = 0.3 to modify the reference search vector by weighting the starred candidate. If the initial ranking is given below:

| CandidateID | job_title                                                                      | location                     | connections | fit      |
|-------------|--------------------------------------------------------------------------------|------------------------------|-------------|----------|
| 7           | HR Senior Specialist                                                          | San Francisco Bay Area       | 500+        | 1.007255 |
| 23          | Nortia Staffing is seeking Human Resources, Pa...                             | San Jose, California         | 500+        | 0.975520 |
| 31          | HR Manager at Endemol Shine North America                                     | Los Angeles, California      | 268         | 0.923865 |
| 36          | Human Resources Management Major                                              | Milpitas, California         | 18          | 0.909303 |

If candidate ID 31 is starred, the updated fit scores and rankings are as follows, with candidate 31 having the highest fit score:

| Candidate ID | job_title                                                                      | location                     | connections | fit      |
|--------------|--------------------------------------------------------------------------------|------------------------------|-------------|----------|
| 31           | HR Manager at Endemol Shine North America                                     | Los Angeles, California      | 268         | 1.175953 |
| 7            | HR Senior Specialist                                                          | San Francisco Bay Area       | 500+        | 0.979793 |
| 23           | Nortia Staffing is seeking Human Resources, Pa...                             | San Jose, California         | 500+        | 0.927448 |
| 11           | Human Resources Coordinator at InterContinenta...                             | Atlanta, Georgia             | 500+        | 0.894896 |

# Summary
This project focuses on developing an automated system to rank potential candidates for roles based on their fit scores. Using text processing techniques like tokenization, stemming, and TF-IDF vectorization, we calculate the similarity between candidate job titles and search terms. We enhance these scores by incorporating location and connection information. When a candidate profile is starred, the system updates the reference search vector to increase the relevance of similar candidates, using a weighted sum approach with a parameter alpha = 0.3. This method improves the accuracy of candidate rankings, as demonstrated by updating the fit scores and rankings when a specific candidate is starred. The process ensures that the most suitable candidates are prioritized, streamlining the talent-sourcing process.
