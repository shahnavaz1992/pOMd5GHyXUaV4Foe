# Potential Talent (NLP)

# Background:
Our objective is to enhance the efficiency of talent sourcing and management company by developing an intelligent system that utilizes machine learning to identify and rank potential candidates based on their fitness for specific roles. Additionally, we seek to incorporate a supervisory signal in the form of candidate starring during the manual review process, allowing for the re-ranking of candidate lists based on valuable feedback from the selection team. 

Ultimately, our goal is to reduce manual efforts and human bias, save time, and optimize the talent acquisition process.

# Data Description
Data comes from the company sourcing efforts; it contains text attributes from the candidate profiles. The dataset has undergone careful data cleaning to protect personal information. Each candidate is represented by a unique identifier and several attributes.

## Attributes (Features):

    id: A unique numerical identifier for each candidate.
    job_title: The job title associated with the candidate.
    location: The geographical location of the candidate.
    connections: The number of connections a candidate has, where "500+" implies more than 500 connections.

## Output (Label):

    fit: This numeric value represents how suitable a candidate is for a specific role, ranging from 0 to 1.

# Goal(s):
* Our primary goal is to predict the fitness of a candidate for a particular role, as represented by the fit variable.
* Success Metric(s): 1) Rank Candidates: Rank candidates based on a fitness score, making it easier to identify the most suitable candidates. 2) Re-rank Candidates: Re-rank candidates when a candidate is "starred" to account for new information or actions.
  
## Bonus(es)
* Filtering Candidates: Efficiently filter out candidates who should not be on this list in the first place.
* Universal Cut-off Point: Determine a cut-off point that works effectively for various roles without excluding high-potential candidates.
* Bias Mitigation: Develop automated procedures to reduce human bias in the candidate selection process.
  
**(Our objective is to build a robust algorithm that not only predicts candidate fitness but also adapts to changes and promotes fairness and efficiency in the selection process.)**

# Solution:

## Data Preprocessing:

As you can see, the data contains symbols, duplicates and acronyms. We need to preprocess them first. Using custom functions and regular expressions, we have enabled data preprocessing.

![image](https://github.com/kuzhuppillil/hRxAyOCqFYmEZwY5/assets/25860818/9c4f639e-8467-4477-a24d-d11be716236c)

We are seeking candidates who show interest in human resources, which can be identified through the presence of keywords such as **"Aspiring human resources" or "Seeking human resources"**. Therefore, we will use either of these phrases as our search keyword.

## Word Embedding Techniques Implemented and Their Effectiveness:

### TF-IDF Weighted Keywords:
* Despite using TF-IDF weighted keywords, it missed identifying some key terms, especially the most frequent search keywords. Due to its reliance on rare words, while capturing word importance, it proved ineffective for our specific objective.

### Word2Vec's Word Embeddings:
* Provided a better fit score by encoding semantic similarity between keywords and job titles, addressing the limitations of Bag-of-Words and TF-IDF models.

### Pretrained BERT Sentence Encoder:
* Improved the fit score further by encoding the contextual meaning of the entire job title text. For instance, it successfully captured titles like "people development" that were missed by other models. However, it also exhibited high fitness scores for non-relevant titles.

**Note: We utilized cosine similarity to calculate the fitness score.**

  ![image](https://github.com/kuzhuppillil/hRxAyOCqFYmEZwY5/assets/25860818/73900b4e-c65f-40fa-8b12-a0e75851767a)


## Ranking candidates:
*  we can see clearly that Bert is the best model till now. And this is not arbitrary, as Bert is pretrained on a large corpus of unlabelled text including the entire Wikipedia(that’s 2,500 million words!) and Book Corpus (800 million words). Indeed, pre-training step is half the magic behind BERT’s success.

## Reranking candidates:
* Finally reranking by starring ideal candidates helps surface other potentials similar to that profile, this refines the ranking further using the starred candiate attributes as a baseline.

![image](https://github.com/kuzhuppillil/hRxAyOCqFYmEZwY5/assets/25860818/9ef9db4b-3367-47a7-98a2-ed54667886cb)

* As the Bert model was the best, so we will use it for this step. In this step, we will use the learning to rank (LTR) technique, a machine learning sub-field applicable to a variety of real-world problems that are related to ranking prediction or candidate recommendation. There are 3 main known models: RankNet, LambdaRank and LmabdaMart. In our wase, we will try to use the RankNet model first and then LamdaRank.

## USE Large Language Model:

* Last part uses the necessary libraries and sets up OpenAI's GPT-3.5 for ranking employee job titles based on relevance to a specific keyword. A PromptTemplate is used to create a structured prompt asking the GPT-3.5 model to rank employees by their job titles and relevance to the provided keyword. The llm object sends the formatted prompt to the GPT-3.5 model, which generates the ranking result. You can replace "Your_OpenAI_Key" with your actual OpenAI API key to run this code with GPT-3.5.
* Hugging Face’s Mixtral model was employed to rank employee job titles based on relevance to a specific keyword. It defines a prompt template that takes a list of employee job titles and a keyword, formats it, and generates a ranking using the Hugging Face pipeline for text generation. The model is explicitly set to truncate the input and pad appropriately. The dataframe data is used to extract the top 12 job titles for the ranking task.

## Conclusion:

* From the steps that I followed in this project, I can infer that BERT model was the best to find the similarity between our data and the targeted keyword/phrase. As for the second part of Re-Ranking, I run the based model of LTR techniques which is the RankNet model. The best model was with the optimizer SGD and a learning rate equal to 0.2. Thus, gives a loss score of 0.44%.





