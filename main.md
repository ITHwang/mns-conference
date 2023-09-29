## # 1
- Hello, ladies and gentlemen.
- I'm Hwang In Taek, a Korean Navy Perry Officer second, on a duty of military service as a Reserach and Development Soldier in the Naval Future Innovation Research Group.
- I'm honored to introduce our project and recent experimental results to you guys.

## # 2
- The order of this presentation is like that.

## # 3
- For more than 10 years, big tech companies, such as OpenAI, Meta, Google, have been sparing no expense for developing Large Language Models, LLMs, as one of the generative AI.
- And now, LLM has undoubtedly become one of the most powerful technologies, changing the greater part of interfaces in most industries.
- Following the trend, from December 2021, Korean Navy has started to construct the Naval Regulation Chatbot system, in which users can input what they wanna ask about regulation and get the answer from the LLM in the system.
- As a matter of fact, the current naval regulation search system only depends on keyword-based search algorithm which cannot find proper regulations unless words in a query exactly match with words in regulations.
- Therefore, to increase accessibility to regulations and utilization of them, our project aims to make the Naval Regulation Chatbot which can analyze user's query semantically and response to the query in a dialog form.
- This system can contribute to heightening the efficiency of work and providing useful guidelines for junior officers.

## # 4
- However, current LLMs are struggling with hallucinating problem which means that they can generate text that factually incorrect or nonsensical.
- This problem is more critical to the regulation chatbot, because both writing official documents and handling tasks are on the basis of regulations, so wrong informaiton about regulations can let officers to get confused and cause poor communication between them.
- Thus, as you guys can see, we are supposed to construct a chatbot arhitecture like that, in which an information retrieval model, so-called "Retriever", searches regulations or parts of them that have high similarity with a given query.
- Then the LLM can receives search result from retriever and generate a more reliable answer based on it.

## # 5
- 

