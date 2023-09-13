# AYA-BACKEND-PROJECT-4
## You are tasked with building a recommendation system for a video streaming service. How would you model user behavior and preferences to generate accurate recommendations? What algorithms and data structures would you use to build this system?

 Have you ever got stunned, or wondered on a scenario whenever you click on a streaming platforms such as YouTube video where you see spontaneous video suggestions, changing  instantaneously or how does Amazon swiftly switch your recommendations based on your activity of the past few clicks/searches? All these are made possible with the help of design systems called 'Recommendation system'. Where companies personalise their suggestions for your profile based on your interactions thus far.
 
 ### MY DESIGN MODEL SYSTEM FOR A STREAMING SERVICE.
 My Design system model approach to solving this will be a 'REAL TIME RECOMMENDATION SYSTEM'.
 
 ### Why my choice for real-time recommendations?
 real-time recommendations, are a lot easier to implement and they are also computationally cheaper. Besides this, The true power of real-time recommendations comes to light when your system depends on context.
 
 #### Stages of my Implementations
 * Part 1: Vector generation
 * Part 2: Vector search/Candidate set retrieval
 * Part 3: Ranking
 * Part 4: System Design

1)  Vector generation: In the first stage, I would need to break down the videos in our catalogue into condensed embeddings. For multi-modal use-cases, using CLIP by OpenAI which is always a good place to start (the model encodes both text and images into the same vector space). CLIP (Contrastive Language-Image Pre-Training) is a neural network trained on a variety of (image, text) pairs. One of the major reasons Why I chose CLIP is because it has excellent zero-shot capabilities and if it doesn’t exactly fit the use-case, I can always fine-tune it. The video can be broken down into images, by extracting the most important frames and these can then be converted into image embeddings using CLIP.


![0_Zo-057DBvZthcmAY](https://user-images.githubusercontent.com/99263767/220308356-948c68b0-357b-42bd-9e66-b191d9fc9271.png)

2)  Vector search/Candidate set retrieval: When have I have successfully decomposed our videos to vectors, I will efficiently store and retrieve these vectors. For this task I could take the help of multiple vector search engines that are readily available in the market. To which Milvus is my preference. 

###### But first Before I continue let me explain to you what Milvus is?
Milvus is an open-source vector database built to power embedding similarity search and AI applications. Milvus makes unstructured data search more accessible and provides a consistent user experience regardless of the deployment environment. Once the data has been inserted into Milvus, we can efficiently retrieve similar videos using Approximate Nearest Neighbour (ANN) search algorithm.

3) Ranking: This a fast process where I will trade accuracy for efficiency and reduce the search space. Ranking is a more meticulous process to select and sort the top suggestions. Here I can use user features (age, gender, category affinity, engagement statistics, etc.) and video features (duration, type, language, number of impressions, etc.) to formulate it into a classification problem (whether the user will click on the video or whether the user will watch it for at least 30 seconds, etc. depending on the use case). The probabilities of this classification problem now serves as the ranking metric for our candidate set. XGBoost or LightGBM might be a good enough algorithm to start while building a classification model.
 
  ![0_mIeB8Uh1HJvZkVo9](https://user-images.githubusercontent.com/99263767/220325805-9cd79b99-90b2-4ac1-beb5-77f08c2d356a.png)

  
  Now I have successfully used the user and the video metadata to sort and pick the top recommendations for our user. After this if needed additional business logic can be added. To evaluate the quality of the recommendations I can use metrics such as recall@k, precision@k, NDCG and MRR and after I am are satisfied with the results I can do an online A/B test to validate our system. 
  
  ### My System Design
  * Workflow: Starting from the user end, the information of the interactions done by the user are transferred using Kafka and the last 5/10 interacted video ids are stored in Redis for all the respective users. Whenever there is an API call to fetch recommendations for a user, these cached video ids are then used to create the average embedding as discussed previously. Once I have the query embedding I can do a vector search on our vector database to get our candidate set.

  * API: Now the user and video metadata has to be stored in a feature store for fast availability. We can use Redis as a feature store but you can experiment with other alternatives. The user and video metadata is now used by our classification model to generate probabilities for our defined task. The final recommendation set is sorted accordingly and then returned to the user. We can use FastAPI to build all the underlying APIs at every step of the process (You can also use Go to build the APIs since latency is of major concern here). These APIs can then be deployed on Kubernetes so that our system is scalable.

![0_zRalJwIcw01nKmWl](https://user-images.githubusercontent.com/99263767/220319062-0c828279-cf50-44e2-9c3b-20dcb883e838.jpg)

### Algoritms/Data structure Implementation 
My design Algorithm is 'Artificial Neural Network (ANN) algorithm' 

#### Importance of Using ANN algorithms
* Firstly, it helps us understand the impact of increasing / decreasing the dataset vertically or horizontally on computational time.
* Secondly, it helps us understand the situations or cases where the model fits best.
* Thirdly, it also helps us explain why certain model works better in certain environment or situations.

#### Formulation of Neural network
I will start with understanding formulation of a simple hidden layer neural network. A simple neural network can be represented as shown below:

![ANN](https://user-images.githubusercontent.com/99263767/220321642-ed6d8392-8aaf-4d2f-bebc-367adf4c5a15.png)

#### Pseudocode for my Algoritm/Datastructure implementation

![Flowchart-for-Artificial-Neural-Network-ANN](https://user-images.githubusercontent.com/99263767/220323236-56b29864-2130-40b6-b666-4609d0c4953f.png)

