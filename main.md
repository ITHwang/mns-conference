## # 1
- Hello, ladies and gentlemen, sir.
- I'm Hwang In Taek, a Korean Navy Perry Officer second, on a duty of military service in the Naval Future Innovation Research Group.
- I'm honored to introduce our project and recent experimental results to you guys.

## # 2
- The order of this presentation is like that.

## # 3
- For more than 10 years, big tech companies, such as OpenAI, Meta, Google, have been sparing no expense for developing Large Language Models, LLMs, as one of the generative AI.
- And now, LLM has undoubtedly become one of the most powerful technologies, changing the greater part of interfaces in most industries.
- Following the trend, from December 2021, Korean Navy has started to construct the Naval Regulation Chatbot system, in which users can input what they wanna ask about regulation and get the answer from the LLM.
- As a matter of fact, the current naval regulation search system only depends on keyword-based search algorithm which cannot find proper regulations unless words in a query and regulations are exactly matched each other.
- Therefore, to increase accessibility to regulations and utilization of them, our project aims to make the Naval Regulation Chatbot which can analyze a query semantically and response to the query in a dialog form.
- This system can contribute to improve the efficiency of work and providing useful guidelines for junior officers.

## # 4
- However, current LLMs are struggling with hallucinating problem which means that they sometimes generate text that factually incorrect or nonsensical.
- This problem is more critical to the regulation chatbot, because both writing official documents and handling tasks are on the basis of regulations, so wrong informaiton about regulations can let officers to get confused and cause poor communication between them.
- Thus, as you guys can see, we designed a chatbot arhitecture like that, in which an information retrieval model, so-called "Retriever", searches relevant regulations and articles, so-called "조문" in korean, based on similarity with a given query.
- Then the LLM can receives search result from retriever and generate a more reliable answer by referring it.

## # 5
- In order to find more exactly relevant regulations and articles, we've decided to make a knowledge base for korean naval regulations by extracting text from raw pdf files and organizing them in a structured form.
- So, in the presentation, we suggest a data pipeline in which raw regulation data is extracted, tranformed, and loaded into the knowledge base.
- And then, we put the knowledge base to the test by applying to the latest open-source retriever to check out how well the knowledge base has been constructed.

## # 6
- Actually, there are many structured formats we can utilize for constructing the knowledge base, such as RDBMS, excel, etc.
- we chose the HTML format among them, because we can easily view non-text data like table and image as well as text just through web browsers.
- And data can be marked up by tags hierarchically, which make it suitable for legal data by nesting sub-information.
- Finally, We can build a self-made question-answering dataset by extracting data corresponding to each query from the unit of sentence to the unit of regulation.
- And later, the dataset can be used in improving the performance of retrievers and LLMs.

## # 7
- So, let's move on to a brief trend in retriever.
- Classically, as a statistical approach for searching documents, Bag-Of-Words algorithm is widely used.
- Before receiving queries, the algorithm give weight to each words in all documents based on word frequency.
- At that time, basically, the more often a word appears, the more weight it has.
- But if the word appears across all documents, its weight would be reduced, because it is used so usually that it may not have to do with specific documents such as personal pronouns and prepositions.
- On the other hand, if a word often appears in specific documents, it can get more weight because the word is likely to implicit topics the documents have.
- So, when we get a query, we can create query vector and document vectors by using words in the query.
- Finally we can find relevant documents that have vectors in the proximity of the query vector.

## # 8
- But, the classical approach has two limitations. 
- Actually, when it performs, It creates a vocabulary.
- And when making vectors, it refers only stuff about words in the vocabulary, as ignoring unknown words out of the vocabulary.
- Besides, even if a query has synonyms of words in the vocabulary, the algorithm cannot answer properly because it still perceives the synonyms as unknown words.
- From last years, however, Deep Learning has been approved as one of the powerful algorithms and it starts to be applied to resolve the limitations.
- As you guys can see on the right, Dense Vector model, as a deep Learning approach, receives queries and documents and outputs their vectors directly.
- At that time, in addition to resolving the limitations, it also comprehends contextual meaning of words, which can facilitates creating optimal vectors of long-text documents.
- Eventually, we select E5 model, as the state-of-the-art model by microsoft, to verify quality of the knowledge base and performance of the retriever.

## # 9
- So, we got 269 regulation pdf files from Korean Navy's intranet.
- And in order to construct the HTML-formated knowledge base, we firstly had to go through a preprocessing pipeline in which raw pdf files can be converted to cleansed and organized text files.
- It took us about 3 months to go through the pipeline.
- At the same time, we succeeded in implementing automation in preprocessing and HTML construction except for one or two manual steps, through which we hope that it's much easier and faster to construct knowledge bases for other regulations in military afterward.

## # 10
- Fo automation in HTML construction, we first check out regulation writing rules written by Korean Navy.
- Then following the rules, we established some strategies, and based on them we created definitions for clauses like this table on the right, and finally wrote python code for the automation.
- Actually, as I said, the automation is following the regulation writing rules strictly.
- Therefore, it also was helpful in finding out mistakes writers had made before, as well as in creating 269 HTML files in just a few seconds.


