# llm-lobbyist

## Large Language Models as Corporate Lobbyists

### Links

- **[Paper](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4316615)**
- **[Code](https://github.com/JohnNay/llm-lobbyist/blob/main/gpt_corporate_lobbying.ipynb)**
- **[Data](https://github.com/JohnNay/llm-lobbyist/blob/main/legislation_relevance_dataset_for_llm_evaluation_unbalanced.csv)**

### Abstract

> We demonstrate a proof-of-concept of a large language model conducting corporate lobbying related activities. An autoregressive large language model (OpenAI’s `text-davinci-003`) determines if proposed U.S. Congressional bills are relevant to specific public companies and provides explanations and confidence levels. For the bills the model deems as relevant, the model drafts a letter to the sponsor of the bill in an attempt to persuade the congressperson to make changes to the proposed legislation. We use hundreds of ground-truth labels of the relevance of a bill to a company to benchmark the performance of the model, which outperforms the baseline of predicting the most common outcome of irrelevance. We also benchmark the performance of the previous OpenAI GPT-3 model (`text-davinci-002`), which was state-of-the-art on many language tasks until `text-davinci-003` was recently released. The performance of `text-davinci-002` is worse than simply always predicting that a bill is irrelevant to a company. These results suggest that, as large language models continue to exhibit improved core natural language understanding capabilities, performance on corporate lobbying related tasks will continue to improve. If AI begins to influence law in a manner that is not a direct extension of human intentions, this threatens the critical role that law as information could play in aligning AI with humans. This paper explores how this is increasingly a possibility. Initially, AI is being used to simply augment human lobbyists. However, there may be a slow creep of less and less human oversight over automated assessments of policy ideas and the written communication to regulatory agencies and Congressional staffers. The core question raised is where to draw the line between human-driven and AI-driven policy influence.

### Citation 

If you use, please cite:

John Nay, *Large Language Models as Corporate Lobbyists* (January 2, 2023). Available at SSRN: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4316615.

Bibtex:

```
@article{nay2023llmlobbyist,
  author = {John Nay},
  archivePrefix = {arXiv},
  eprint = {2301.01181},
  primaryClass = {cs.CL},
  title = {Large Language Models as Corporate Lobbyists},
  year = 2023,
  keywords = {language models, alignment, policy},
  url = {https://arxiv.org/abs/2301.01181}
}
```

### Overview

We use autoregressive large language models to systematically:

1.	Summarize bill summaries that are too long to fit into the context window of the model so the model can conduct steps 2 and 3.
2.	Using either the original bill summary if it was not too long, or the summarized version, assess whether the bill may be relevant to a company based on a company’s description in its 10K filing. Provide an explanation for why the bill is relevant or not. Provide a confidence level to the overall answer.
3.	If the bill is deemed relevant to the company by the model, draft a letter to the sponsor of the bill arguing for changes to the bill.

The model is provided with the following data, which is embedded in the prompts programmatically:

- Official title of bill `{official_title}`
- Official (or model-generated if too long) summary of bill `{summary_text}`
- Official subjects of bill `{subjects}`
- Company name `{company_name}`
- Company business description `{business_description}` (the business description in the company’s SEC Form 10-K filing)

We expect much higher accuracy of the model’s predictions if we were to provide it with more data about a bill, and especially if we provide it with more data about a company. This paper was focused on the minimal amount of data a model could leverage in order to compare across models.
Here is the prompt provided to the model for each prediction:

```
You are a lobbyist analyzing Congressional bills for their potential impacts on companies. 
Given the title and summary of the bill, plus information on the company from its 10K SEC filing, it is your job to determine if a bill is at least somewhat relevant to a company (in terms of whether it could impact the company if it was later enacted). 
Official title of bill: {official_title}
Official summary of bill: {summary_text}
Official subjects of bill: {subjects}
Company name: {company_name}
Company business description: {business_description}
Is this bill potentially relevant to this company? 
Answer in this format:
ANSWER: 'YES' or 'NO' (use all caps). EXPLANATION: the step-by-step reasoning you undertook to formulate a response. CONFIDENCE: integer between 0 and 100 for your estimate of confidence in your answer (1 is low confidence and 99 is high)
```

Here is an example of the model output:

```
ANSWER: YES. 
EXPLANATION: Alkermes Plc is a biopharmaceutical company that develops and commercializes products designed to address unmet medical needs of patients in major therapeutic areas, including addiction and schizophrenia. This bill requires the Centers for Medicare & Medicaid Services (CMS) to negotiate with pharmaceutical companies regarding prices for drugs covered under the Medicare prescription drug benefit, which could potentially impact Alkermes Plc's products.
CONFIDENCE: 95
```

For all bills that are deemed relevant, we prompt the model to write a letter on behalf of the company. Here is an example letter:

```
Dear [Sponsor of the Bill],

I am writing on behalf of Alkermes Plc, a fully integrated, global biopharmaceutical company that applies its scientific expertise and proprietary technologies to research, develop and commercialize pharmaceutical products that are designed to address unmet medical needs of patients in major therapeutic areas.

We are writing to express our support for the Medicare Negotiation and Competitive Licensing Act of 2019. We believe that this bill is an important step in ensuring that Medicare beneficiaries have access to the medications they need at a price they can afford.

We are particularly supportive of the provisions in the bill that would require the Centers for Medicare & Medicaid Services (CMS) to negotiate with pharmaceutical companies regarding prices for drugs covered under the Medicare prescription drug benefit. We believe that this will help to ensure that the prices of these drugs are fair and reasonable.

We are also supportive of the provisions in the bill that would allow for competitive licensing of drugs if the CMS is unable to negotiate the price of a drug. This will help to ensure that the prices of these drugs are kept in check and that Medicare beneficiaries have access to the medications they need.

At Alkermes, we develop and commercialize products designed to address the unmet needs of patients suffering from addiction and schizophrenia. We have two key marketed products, ARISTADA and VIVITROL, which are used to treat these conditions. We believe that the provisions in the bill will help to ensure that our products are available to Medicare beneficiaries at a price they can afford.

We would like to suggest that the bill be amended to include provisions that would provide additional incentives for pharmaceutical companies to negotiate with the CMS. We believe that this would help to ensure that the prices of drugs are kept in check and that Medicare beneficiaries have access to the medications they need.

We thank you for your consideration and look forward to working with you to ensure that the Medicare Negotiation and Competitive Licensing Act of 2019 is passed in its amended form.

Sincerely,

[Name],
General Counsel
Alkermes Plc
```


