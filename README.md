# llm-lobbyist

## Large Language Models as Corporate Lobbyists

Paper: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4316615

**Abstract:** 
> We demonstrate a proof-of-concept of a large language model conducting corporate lobbying related activities. We use OpenAI’s GPT-3.5 model ("text-davinci-003") to determine if proposed bills are relevant to specific companies. Then, for the bills the language model deems as relevant, the model drafts a letter to the sponsor of the bill in an attempt to persuade the congressperson to make changes to the proposed bill. We use ground-truth labels of the relevance of a bill to a company to benchmark the performance of the model, which outperforms the baseline of predicting the most common outcome of irrelevance. However, we test the ability to determine the relevance of a bill with the previous OpenAI GPT-3 model ("text-davinci-002"), which was state-of-the-art until text-davinci-003 was released on November 28, 2022, and the performance is worse than simply always predicting that any bill is irrelevant. These results suggest that, as models improve, performance on corporate lobbying related tasks will continue to improve. We then discuss why this could be problematic. 

**Code**: https://github.com/JohnNay/llm-lobbyist/blob/main/gpt_corporate_lobbying.ipynb

**Data**: https://github.com/JohnNay/llm-lobbyist/blob/main/legislation_relevance_dataset_for_llm_evaluation_unbalanced.csv

**Citation**: John Nay, Large Language Models as Corporate Lobbyists (January 2, 2023). Available at SSRN: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4316615.
