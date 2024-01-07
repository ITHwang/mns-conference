## # 1
- Good afternoon! I'm Hwang In Taek, a Korean Navy Petty Officer second, on a duty of military service in the Naval Future Innovation Research Group.
- I'm honored to introduce our project.

## # 2
- This is the order of presentation.

## # 3
- Nowadays, LLM has undoubtedly become one of the most powerful technologies, changing the greater part of interfaces in most industries.
- OpenAI uses LLM to create a ChatBot in its platform to interact with users seamlessly.
- So, What is LLM?
- LLM stands for Large Language Model. 
- It's a machine-learning model that can understand and create human language using neural networks.
- Using LLM, we have started to develop the Naval Regulation Chatbot, in which users can search for regulation in a conversational style. 
- Actually, the current regulation search system only depends on keyword-based algorithms that make it inflexible on human language.
- That's why our team is developing the regulation chatbot, hoping that it helps users find the contents of regulations more efficiently and accurately.

## # 4
- However, current LLMs are struggling with a hallucinating problem: a case when the model generates text that is factually incorrect or nonsensical.
- This problem is more critical to the regulation chatbot because most official documents are fully based on regulations.
- Thus, to address this problem, we designed a chatbot architecture in the picture, in which an information retrieval model, the so-called "Retriever", searches relevant regulations and articles based on similarity with a given query.
- Then the LLM receives search results from the retriever and returns a more reliable answer.

## # 5
- For better search performance, We made the retriever into a data-centric AI that focuses on the quality of the input data rather than its intelligence.
- So, in the presentation, we suggest a data preprocessing pipeline in which raw regulation data is extracted, transformed, and stored as cleansed data.
- Using the data, we share some strategies for building a knowledge base for Korean naval regulations.
- Finally, we conduct experiments with the knowledge base and the latest open-source retriever to evaluate the quality of the data.

## # 6
- Actually, there are many structured formats we can utilize for building the knowledge base.
- Our team chose HTML, in which non-text data like tables and images can be easily viewed just through a web browser as well as plain text.
- And data can be marked up by HTML tags hierarchically, which makes it suitable for legal data.
- Lastly, using the data, we can build question-answering datasets which can help upgrade retrievers and LLMs later.

## # 7
- Let's move on to a brief trend in retrievers.
- Classically, as a statistical approach, the Bag-Of-Words algorithm is widely used.
- The algorithm calculates word frequency in all documents to give weight to the frequently used words.
- In particular, it gives higher weight to meaningful keywords.
- But it gives lower weight to grammatical words that don’t have semantic meaning.
- After measuring the weights, it creates document vectors and estimates similarity with a query vector.
- As the picture on the right shows, the document with the highest proximity will be the most relevant.

## # 8
- However, this approach has two limitations.
- Firstly, it ignores unseen words out of vocabulary when creating a query vector.
- Secondly, it doesn't recognize synonyms, which means users have to input the same words in documents to search for the documents they want.  
- To overcome the limitations, our team used the Dense Vector model to create query and document vectors, without depending solely on word frequency.
- Also, this model can understand contextual meaning, which helps create optimal vectors.
- Among the dense vector models, we chose the state-of-the-art open-source model, called E5 created by Microsoft.

## # 9
- To construct the knowledge base, we converted 269 regulation PDF files to preprocessed text files.  
- We succeeded in automating the data pipeline except for a few manual steps, through which it'll be easier and faster to build knowledge bases of other military regulations.

## # 10
- Next, to automate HTML construction, our team had a deep dive into the structure of regulations and regulation rules, and established construction strategies including how to customize HTML tags and how to insert inner clauses into outer clauses, and so on.

## # 11
- As the picture shows, regulation data is stored in an HTML file.
- And metadata of clauses like titles, revision dates, and clause numbers, is embedded as attributes of HTML tags.
- Besides, when opening these files on a web browser, we can read the contents as easily as we do in the PDF files.

## # 12
- In the knowledge base, we have structured 10,379 articles from 269 naval regulations.
- By removing 476 articles deleted by revisions, we can now utilize 9,903 articles, in which the retriever can find query-related articles.

## # 13
- And then we conducted experiments to make sure the quality of the knowledge base as well as the performance of the retriever in the military domain.
- To do so, we created a simple question-answering dataset consisting of 1,000 pairs of queries and articles.
- We also included 80 diagram queries that can be answered based on tables or images.
- And 50 higher-law queries that can be answered based on upper regulations like the Ministry of Defense Instruction which is "국방부 훈령" in Korean.

## # 14
- We utilized the multilingual version of the E5 model as the retriever.
- The retriever finds the relevant documents by sorting all the documents in similarity order.
- So, we chose three metrics to evaluate the ranking lists of documents created by the retriever.
- MRR at K is calculated based on the ranking of the most query-related document.
- Recall at K is measured by how many documents the retriever finds among the correct top K documents.
- Lastly, Top at K Accuracy counts the number of cases when the most query-related document is in the top K documents arranged by the model.

## # 15
- The next step is to convert the knowledge base to the input data for the retriever to create document vectors.
- It's crucial to the retriever's performance because the vectors are used for scoring the similarity with queries.
- There are 5 converting options, also known as embedding options.
- The first one and the second one make use of customized tags.
- The first includes attributes, but the second doesn't.
- The third one and the fourth one make use of standard tags.
- The third includes attributes, but the fourth doesn't.
- The last embedding makes use of the plain text by removing all tags and attributes.
- Furthermore, we're going to compare these options to the same retriever on another dataset, which helps evaluate the quality and difficulty of our dataset.

## # 16
- So, let's take a look at the results.
- In Regulation MRR at 10 and 20, the retriever shows the highest performance with about 90 out of 100, when choosing the text with standard tags and attributes.
- Moreover, this retriever outperforms the same retriever with another dataset, which means it's relatively easy to find query-related regulations.
- In Regulation TOP at 5, 10, and 20 Accuracy, the retriever shows a near-perfect score, when choosing the text with custom tags and attributes.

## # 17
- Next, in Article MRR at 10 and 20, the retriever shows the highest performance, when choosing the text with standard tags and attributes.
- It's more challenging than Regulation MRR because it's difficult to find the most query-related article among about 10,000 articles.
- Nonetheless, the retriever on our dataset outperforms the same retriever on another dataset, which means the quality of the knowledge base is high enough to be used for our chatbot.
- Meanwhile, on the right side, the retriever shows lower performance on diagram queries, because tables and images were ignored at the preprocessing step.
- It also doesn't do very well for higher-law queries, because these articles usually have such short sentences that it's hard to get a high similarity with a given query.

## # 18
- In Article TOP Accuracy, we can get the highest performance, when choosing the text with standard tags and attributes.
- As the graph on the left shows, Article TOP Accuracy is more challenging than Regulation TOP Accuracy.
- Despite that, it shows an outstanding performance at TOP 10 and 20 Accuracy by exceeding 90 out of 100.
- And, in the graph on the right, it's still hard to answer the diagram queries and the higher-law queries.

## # 19
- So, I’d like to wrap up the presentation by sharing two further issues and two expected effects.
- The first issue is about ignored non-text data.
- To improve the retriever's ability to answer the diagram queries, tables and images in the regulations should be preprocessed and transformed into HTML tags.
- And the next step is to attach the retriever to an LLM.
- However, at this moment, the Defense Integrated Data Center, also known as DIDC, doesn't have enough GPUs for LLM.
- To address the problem, the DIDC is currently establishing the Defense Intelligent Platform with KT, the Korean telecommunication company.

## # 20
- Consequently, the preprocessing pipeline and the strategies for the Knowledge base will help construct those of other Korean military regulations afterward.
- In particular, the Ministry of Defense Instruction should be established, because many Korean military regulations are strongly following the instruction.
- Last but not least, the Ministry of Defense is planning to make standard datasets to adapt AI models to military domains.
- Knowledge bases of military regulations can help build the standard question-answering dataset.
- The retriever can also contribute to reducing the time to build the dataset, by finding query-related documents much faster than humans.

## # 21
- Thank you for listening.
- Does anyone have any questions?

## # 22
-   Thank you so much!