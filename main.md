## # 1
- Hello, ladies and gentlemen, sir.
- I'm Hwang In Taek, a Korean Navy Petty Officer second, on a duty of military service in the Naval Future Innovation Research Group.
- I'm honored to introduce our project and recent experimental results to you guys.

## # 2
- The order of this presentation is like that.

## # 3
- For more than 10 years, big tech companies, such as OpenAI, Meta, Google, have been sparing no expense for developing Large Language Models, LLMs, as one of the generative AI.
- And now, LLM has undoubtedly become one of the most powerful technologies, changing the greater part of interfaces in most industries.
- Following the trend, from December 2021, we have started to construct the Naval Regulation Chatbot system, in which users can input what they wanna ask about regulation and get the answer from the LLM.
- As a matter of fact, the current naval regulation search system only depends on keyword-based search algorithm which cannot find suitable regulations unless words in a query and regulations are exactly matched each other.
- Therefore, to increase accessibility to regulations and utilization of them, our project aims to make the Naval Regulation Chatbot which can analyze a query semantically and contextually and response to the query in a dialog form.
- This system can contribute to improving work efficiency and providing useful guidelines for junior officers.

## # 4
- However, current LLMs are struggling with hallucinating problem which means that they sometimes generate text that factually incorrect or nonsensical.
- This problem is more critical to the regulation chatbot, because both writing official documents and handling tasks are on the basis of regulations, so wrong information about regulations can let officers to get confused and cause poor communication between them.
- Thus, to address this problem, we designed a chatbot architecture like that, in which an information retrieval model, so-called "Retriever", searches relevant regulations and articles, so-called "조문" in Korean, based on similarity with a given query.
- Then the LLM can receives search result from retriever and generate a more reliable answer.

## # 5
- In order to find more exactly relevant regulations and articles, we've decided to make a knowledge base for Korean naval regulations by extracting data from raw pdf files and organizing them in a structured form.
- So, in the presentation, we suggest a data pipeline in which raw regulation data is extracted, transformed, and loaded into the knowledge base.
- And then, we put the knowledge base to the test by applying to the latest open-source retriever to check out how well the knowledge base has been constructed.

## # 6
- Actually, there are many structured formats we can utilize for constructing the knowledge base, such as RDBMS, excel, etc.
- we chose the HTML format among them, because we can easily view non-text data like tables and images just through web browsers as well as plain text.
- And data can be marked up by tags hierarchically, which make it suitable for legal data by nesting sub-information.
- Finally, We can build a self-made question-answering dataset by extracting data corresponding to each query.
- And later, the dataset can be used in improving the performance of retrievers and LLMs.

## # 7
- So, let's move on to a brief trend in retriever.
- Classically, as a statistical approach for searching documents, Bag-Of-Words algorithm is widely used.
- Before receiving queries, the algorithm give weight to each words in all documents based on word frequency.
- At that time, basically, the more often a word appears, the more weight it has.
- But words like personal pronouns or prepositions appears across all documents, so that its weight would be reduced, because they're used so usually that they don't have to do with specific documents. 
- On the other hand, words about some topics often appears in specific documents, so that they can get more weight because they're likely to implicit topics of the documents.
- So, when we get a query, we can create query vector and document vectors based on the words in the query.
- Finally we can find relevant documents that have vectors in the proximity of the query vector.

## # 8
- But, the classical approach has two limitations. 
- The fact is, It needs the vocabulary that includes information about words in all documents.
- And when making vectors, it refers only stuff about words in the vocabulary, as ignoring unknown words out of the vocabulary.
- Moreover, even if a query has synonyms of words in the vocabulary, the algorithm cannot answer properly because it still perceives the synonyms as unknown words.
- From last years, however, Deep Learning has been approved as one of the powerful algorithms and it starts to be applied to resolve the limitations.
- As you guys can see on the right, Dense Vector model, as a deep Learning approach, receives queries and documents and returns their vectors directly.
- At that time, in addition to resolving the limitations, it also comprehends contextual meaning of words, which can facilitates creating optimal vectors of long-text documents.
- Eventually, we selected E5 model, as the state-of-the-art model by Microsoft, to verify quality of the knowledge base and the performance of the retriever in a military regulation domain.

## # 9
- So, we got 269 regulation pdf files from Korean Navy's intranet and designed a preprocessing pipeline in which these raw files can be converted to the cleansed and organized text files.
- At that time, to be honest, we preprocessed only text data as putting off table and image data for shortening the preprocessing period.
- We're gonna deal with this issue in conclusion.
- Consequently, It took us about 3 months to go through the pipeline.
- And we succeeded in implementing automation in preprocessing except for two or three manual steps, through which we hope that it's much easier and faster to construct knowledge bases for other military regulations afterward.

## # 10
- Next, for automation in HTML construction, we check out regulation writing rules written by Korean Navy.
- Then following the rules, we established some strategies, and created tag definitions for clauses like this table on the right, and finally implemented these things with Python.
- Accordingly, we could create 269 HTML files in just a few seconds.

## # 11
- As you guys can see, regulation data is stored in the structured form with customized HTML tags.
- And meta data of clauses like titles, revision dates, and orders of clauses, is embedded as attributes of the each tag.
- Besides, when opening these HTML files on a web browser, like Chrome or Microsoft Edge, we can read contents in the files as easily as we do in the pdf files.

## # 12
- In the knowledge base, we've structured 10,379 articles in 269 naval regulations.
- And then, removing 476 articles deleted by revisions earlier, we can now utilize 9,903 articles, in which the retriever can find query-related articles.

## # 13
- Then using the knowledge base, we made some experiments to make sure quality of the knowledge base and performance of the retriever in a military regulation domain.
- To do that, we decided to make a simple question-answering dataset consisting of 1,000 pairs of query and related article as one-to-one mappings.
- And then, we assigned the number of queries to each regulations in proportion to the amount of the regulation, so that we could make more queries about bigger regulations.
- 1,000 pairs were made manually by 3 people for about 2 weeks, including declarative and imperative queries as well as interrogative queries.
- We also included 80 diagram queries that can be answered based on tables or images in query-related articles and 50 higher-law queries that can be answered based on upper regulations referred by query-related articles.

## # 14
- And then, as I said, we utilized the multilingual version of E5 model as the retriever with some required options.
- We also selected three metrics to evaluate ranking lists of documents created by the retriever.
- MRR at K calculates depending on what the ranking of the most query-related document is in the top K documents inferred by the retriever.
- The closer the document's ranking is to top, the closer the score is to 1, on the other hand, the farther the document's ranking is from top, the closer the score is to 0.
- Next, Recall at K calculates depending on the number of documents found by the models among the correct top K query-related documents.
- The more the model finds documents in the correct documents, the closer the score is to 1, on the other hand, the less the model does, the closer the score is to 0.
- We set K to only 1, because in our simple dataset every query has one relevant regulation or one relevant article.
- Lastly, Top at K Accuracy takes account of whether the most query-related document is in the top K query-related documents inferred by the model.
- And then, it counts the number of cases in which model's output meets this condition and calculates the proportion of them.

## # 15
- HTML basically uses tags and attributes through which we contained meta information of sentences or paragraphs in regulations.
- The next step is to convert the data in the knowledge base to the input data of the retriever for creating vector representations.
- This step is so crucial to the performance of the retriever, because the vector representations are finally used to calculate the similarity with queries.
- Thus we put 5 converting options, also known as embeddings.
- First one and second one make use of customized tags which are mapped to clauses in regulations respectively.
- And first inserts attributes into the tags, whereas second doesn't.
- Third one and fourth one make use of standard tags which are already defined as web standards.
- And third inserts attributes into the tags, whereas fourth doesn't.
- Last embedding option is to remove all tags and attributes leaving the plain text in the tags.
- In addition, we're gonna compare the results of these options to the performance of the same retriever on another question-answering dataset for evaluating the quality and difficulty of our dataset.

## # 16
- Let's take a look the results. 
- In Regulation MRR at 10 and 20, the retriever shows the highest performance with about 90 out of a hundred, when we convert the text with standard tags and attributes to vector representations.
- On the other hand, considering the fact that the same retriever with another dataset shows much lower performance, we can suppose that it's so easier to find query-related regulations than documents in another dataset.
- On the right, in Regulation TOP at 5, 10, and 20 Accuracy, the retriever shows the highest performance with near perfect score, when we convert the text with custom tags and attributes to vector representations.
- But it seems more adequate to use the text with standard tags and attributes, because in Recall at 1, its score is higher than that of the text with custom tags and attributes.
- Besides, There are very small differences between them in other Accuracy metrics.

## # 17
- Next, in Article MRR at 10 and 20, the retriever shows the highest performance, when we utilize the text with standard tags and attributes.
- It's about 72 out of a hundred.
- It's more challenging than Regulation MRR, because it's difficult to find the most query-related article among about 10,000 articles and put it closer to the highest in the ranking list.
- Nonetheless, as you guys can see that the retriever on our dataset shows the higher score than that on another dataset, we confirm that the quality of the knowledge base is high enough to be used.
- Meanwhile, on the right side, the retriever shows lower performance when receiving the queries that can be answered based on tables or images.
- It's because we left tables and images as black boxes at the earlier preprocessing step.
- It also doesn't do very well when receiving the queries that can be answered based on upper regulations like the Ministry of Defense Instruction.
- It's because the articles related to the queries usually has such a short sentence that it's hard to get high similarity between the articles and the queries.

## # 18
- In Article TOP at 1, 5, 10, and 20 Accuracy, we can get the highest performance, when using the text with standard tags and attributes.
- It's obvious that Article TOP Accuracy is more challenging than Regulation TOP Accuracy, especially in Recall at 1.
- In spite of that, it shows the prominent performance at TOP 10 and 20 Accuracy by being above 90 out of a hundred.
- And, in the graph on the right, as it does in Article MRR, it's hard to answer the diagram queries and the upper-law queries.

## # 19


