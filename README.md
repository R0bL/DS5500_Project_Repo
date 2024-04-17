# Decoding Corporate Narratives: A RAG-Based Approach to Navigating 10-K Filings

Robert Lalani 

This project is an attempt to understand the shifting messaging ESG messaging in 10-K filings using a RAG (Retrival Augmented Generation Pipeline). 

## The Landscape

ESG metrics encompass three fundamental pillars: 

1. The "E" dimension evaluates a company's management of natural resources and its environmental impact, encompassing both direct operations and supply chain activities.

2. The "S" aspect pertains to social factors, assessing a company's effectiveness in navigating social trends, labor practices, and political dynamics.

3. The "G" component of ESG focuses on governance factors, examining decision-making processes from governmental policy-making to the allocation of rights and responsibilities within corporations. This includes scrutiny of governance structures involving the board of directors, managers, shareholders, and stakeholders

ESG ratings play a role in guiding trillions of dollars in investments worldwide. For example, the investment strategy of the Maine Public Employees Retirement System Pension Fund is guided by ESG criteria, and they highlight the significance of integrating sustainability factors for long-term investment success.

In recent years, ESG-related shareholder proposals—an official vehicle through which shareholders can interface with the board of directors—have become more prominent.

#### Number of Shareholder Proposals by Sub-Categories, 2022-2023
![image-31](https://github.com/R0bL/Project_Initiation_DS5500/assets/133535059/089b5330-35f6-46f3-9d53-eef8d96d84f4)

#### The Challenges 


1. Lack of transparency among the ESG raters on how the scores are assigned.
2. Lack of standards on how a particular concept is measured.
3. Questionable tradeoffs: high scores in one domain may offset very low scores in another area.
4. Absence of an overall score combining performance scores along Environmental, Social, and Governance (ESG) axes; weights given to the "E," "S," and "G."
5. Lack of acknowledgement of stakeholder expectations.



## The Data

This analysis specifically examines the 966 US equities in which the Norwegian Sovereign Wealth Fund has invested. The $1.4 trillion fund, managed by the Norwegian government, originated from oil and gas resources discovered in the late 1960s on the Norwegian continental shelves. Serving as a strategic financial reserve, the fund holds stakes in about 9,000 companies worldwide, owning approximately 1.4 percent of every listed company globally. Firm count by sector: 

| Sector                  | Number of Companies |
|-------------------------|---------------------|
| Basic Materials         | 63                  |
| Communication Services | 35                  |
| Consumer Cyclical       | 96                  |
| Consumer Defensive      | 38                  |
| Energy                  | 58                  |
| Financial Services      | 153                 |
| Healthcare              | 108                 |
| Industrials             | 157                 |
| Real Estate             | 87                  |
| Technology              | 134                 |
| Utilities               | 37                  |

I have chosen to focus on this subset of publicly traded companies to benchmark US firms against European long-term sustainable investing strategies, specifically examining the Norwegian Sovereign Wealth Fund's voting guidelines. This approach aims to compare the governance and sustainability practices of American corporations with those promoted by one of Europe's most significant investors, known for its adherence to ESG principles.

#### **Investors’ Support for “Pro-ESG” Shareholder Proposals at S&P 500 Companies**
![image-32](https://github.com/R0bL/Project_Initiation_DS5500/assets/133535059/7cc9803a-65cb-459c-8174-2a8bd0f39bf4)


## The Goal
The goal of this project is to equip large language models (LLMs) with domain-specific data derived from the 10-K disclosure filings of 968 publicly traded firms, as well as the Norwegian Wealth Fund's voting patterns on shareholder proposals. Enabling the LLMs to tailor their outputs, drawing context from authoritative sources concerning environmental, social, and governance (ESG) messaging and Corprate Goverannce. 

## An Overview: 

This project is broken down into a few steps. 

#### 1. Data Collection from Norwegian Sovereing Wealth Fund to get the list of US equities:
   
A link to the API can be found here: [Norges Bank Investment Management API](https://www.nbim.no/en/responsible-investment/voting/our-voting-records/api-access-to-our-voting/) 

The code used to collect data can be found: ``` Step_1_DataCollection.ipynb```


#### 2. Data Collection from SEC EDGAR System to get Corprate 10-K filings:

Link to sec-api.io : https://sec-api.io/docs/sec-filings-item-extraction-api

The code used to collect data can be found: ```Step_2_SEC_EDGAR.ipynb```

Output from Step_1 can be found in Step_2 folder ```"cleaned_company_list.csv" ```

 
#### 3. Data preprocessing: Converting text into a PDF

Link to open source nlp preprocesser spaCy: https://spacy.io/api/sentencizer

The code used to collect data can be found: ```Step_3_DataPreprocessing.ipynb```

Output of step 3: ```utitlites.pdf```

#### 4. Ingesting the PDF, preprocessing the text into chunks:  

The code used to collect data can be found: ```Step_4_Preprocessing_text.ipynb```

Output is a list of dicitonaries with token count:

```
{'page_number': 42,
  'ticker': 'LNT',
  'sector': 'Utilities',
  'filing_date': '2023-02-24T16:03:25-05:00',
  'sentence_chunk': '2) WPL - is a public utility engaged principally in the generation and distribution of electricity and the distribution and transportation of natural gas to retail customers in select markets in Wisconsin. WPL operates in municipalities pursuant to permits of indefinite duration and state statutes authorizing utility operation in areas annexed by a municipality. At December 31, 2022, WPL supplied electric and natural gas service to approximately 495,000 and 200,000 retail customers, respectively. WPL also sells electricity to wholesale customers in Wisconsin. 3) CORPORATE SERVICES - provides administrative services to Alliant Energy, IPL, WPL and AEF. 4) AEF - Alliant Energys non-utility holdings are organized under AEF, which manages a portfolio of wholly-owned subsidiaries and additional holdings, including the following distinct platforms: ATI - currently holds all of Alliant Energys interest in ATC Holdings. ATC Holdings is comprised of a 16% ownership interest in ATC and a 20% ownership interest in ATC Holdco LLC. ATC is an independent, for-profit, transmission-only company. ATC Holdco LLC holds an interest in Duke-American Transmission Company, LLC, a joint venture between Duke Energy Corporation and ATC, that owns electric transmission infrastructure in North America. ##',
  'chunk_char_count': 1298,
  'chunk_word_count': 190,
  'chunk_token_count': 324.5},
 {'page_number': 42,
  'ticker': 'LNT',
  'sector': 'Utilities',
  'filing_date': '2023-02-24T16:03:25-05:00',
  'sentence_chunk': 'TABLE_START 3 ##TABLE_ENDTable of Co ntents Corporate Venture Investments - includes various minority ownership interests in regional and national venture funds, including a global coalition of energy companies working together to help advance the transition towards a cleaner, more sustainable, and inclusive energy future, by identifying and researching innovative technologies and business models within the emerging energy economy. Non-utility Wind Farm - includes a 50% cash equity ownership interest in a 225 MW non-utility wind farm located in Oklahoma. Sheboygan Falls Energy Facility - is a 347 MW, simple-cycle, natural gas-fired EGU near Sheboygan Falls, Wisconsin, which is leased to WPL for an initial period of 20 years ending in 2025. Travero - is a diversified supply chain solutions company, including a short-line rail freight service in Iowa; a Mississippi River barge, rail and truck freight terminal in Illinois; freight brokerage services; and a rail-served warehouse in Iowa. B. INFORMATION RELATING TO ALLIANT ENERGY ON A CONSOLIDATED BASIS 1) HUMAN CAPITAL MANAGEMENT - Alliant Energys core purpose is to serve customers and build stronger communities. We constantly strive to attract, retain and develop a diverse and qualified workforce of high-performing employees, and create and foster an environment of inclusion and belonging for all employees.',
  'chunk_char_count': 1376,
  'chunk_word_count': 204,
  'chunk_token_count': 344.0}]
```

Link to hugging face: https://huggingface.co/sentence-transformers/all-mpnet-base-v2

#### 5. Creating the embeddings and sematic search pipeline between a user query and the text
Embedding the chunks: use a pretrained model mpnet-base model

See code for steps 5,6,7 in folder step_5_6_7 - ```Step_5_6_7Embeddings_LLM.ipynb```

```
Query: 'Environmental, Social, Governance Reporting'

Results:
Score: 0.5620
Text:
If we are unable to enter into favorable contracts or to obtain the necessary
regulatory and land use approvals on favorable terms, we may not be able to
construct and operate our assets as anticipated, or at all, which could
negatively affect our business, results of operations and financial condition.
We could be negatively impacted by environmental, social, and governance (ESG)
and sustainability-related matters. Governments, investors, customers, employees
and other stakeholders are increasingly focusing on corporate ESG practices and
disclosures, and expectations in this area are rapidly evolving. We have
announced, and may in the future announce, sustainability-focused goals,
initiatives, investments and partnerships. These initiatives, aspirations,
targets or objectives reflect our current plans and aspirations and are not
guarantees that we will be able to achieve them. Our efforts to accomplish and
accurately report on these initiatives and goals present numerous operational,
regulatory, reputational, financial, legal, and other risks, any of which could
have a
Page number: 704
Stock Ticker: NFE


Score: 0.5384
Text:
The following table presents employee information, including information about
CBAs, as of December 31, 2022: ##TABLE_START Total Employees Covered by CBAs
Number of CBAs CBAs New and Renewed in 2022 (a) Total Employees Under CBAs New
and Renewed in 2022 3,342 21 1 74 ##TABLE_END__________ (a) Does not include
CBAs that were extended in 2022 while negotiations are ongoing for renewal.
Environmental Matters and Regulation We are subject to comprehensive and complex
environmental legislation and regulation at the federal, state, and local
levels, including requirements relating to climate change, air and water
quality, solid and hazardous waste, and impacts on species and habitats. Our
Board of Directors is responsible for overseeing the management of environmental
matters. We have a management team to address environmental compliance and
strategy, including the CEO, our Sustainability and Climate Strategy team, and
other members of senior management. Performance of those individuals directly
involved in environmental compliance and strategy is reviewed and affects
compensation as part of the annual individual performance review process.
Page number: 268
Stock Ticker: CEG
```
   
#### 6. Loading an LLM locally

Link to LLM: https://huggingface.co/google/gemma-7b-it

```
def prompt_formatter(query: str,
                     context_items: list[dict]) -> str:
    """
    Augments query with text-based context from context_items.
    """
    # Join context items into one dotted paragraph
    context = "- " + "\n- ".join([item["sentence_chunk"] for item in context_items])

    # Create a base prompt with examples to help the model
    # Note: this is very customizable, I've chosen to use 3 examples of the answer style we'd like.
    base_prompt = """Based on the following context items, please answer the query.
Give yourself room to think by extracting relevant passages from the context before answering the query.
Don't return the thinking, only return the answer.
Make sure your answers are as explanatory as possible.
Use the following examples as reference for the ideal answer style.
\nExample 1:
Query: What are specific State/Federal Regulations in the utility sector?
Answer: Sure, here is the answer to the query:  The utility sector is subject to
regulation by a number of federal, state, and local agencies. The federal
government regulates wholesale sales of electricity rates and interstate
transmission of electricity, including System Energys sales of capacity and
energy from Grand Gulf to Entergy Arkansas, Entergy Louisiana, Entergy
Mississippi, and Entergy New Orleans pursuant to the Unit Power Sales Agreement.
State public utility commissions have jurisdiction over services and facilities,
rates and charges, accounting, valuation of property, depreciation rates and
various other matters.

    # Update base prompt with context items and query
    base_prompt = base_prompt.format(context=context, query=query)

    # Create prompt template for instruction-tuned model
    dialogue_template = [
        {"role": "user",
        "content": base_prompt}
    ]

    # Apply the chat template
    prompt = tokenizer.apply_chat_template(conversation=dialogue_template,
                                          tokenize=False,
                                          add_generation_prompt=True)
    return prompt
```

#### 7. Generating text with an LLM


```
Query: Which utility companies have plans to expand into solar energy production?
[INFO] Time taken to get scores on 4475 embeddings: 0.00006 seconds.
Answer:

The text mentions several utility companies that have plans to expand into solar
energy production, including AES Clean Energy, Consumers, and Entergy Louisiana.
AES Clean Energy is actively developing and implementing renewable energy
solutions, including solar energy. Consumers' coal-fueled generating units
burned six million tons of coal and produced a combined total of 10,217 GWh of
electricity in 2022. Entergy Louisiana has executed a 20-year agreement for 50
MW from a to-be-constructed solar photovoltaic electric generating facility
located in West Baton Rouge Parish, Louisiana.
Context items:
[{'page_number': 766,
  'ticker': 'NI',
  'sector': 'Utilities',
  'filing_date': '2023-02-22T16:23:21-05:00',
  'sentence_chunk': 'Ticker: NI, Sector: Utilities, Filed At: 2023-02-22T16:23:21-05:00 Schahfer Generating Station and upgrades to the transmission system to enhance our electric generation transition. Recent developments, including macro supply chain issues and U. S. federal policy actions, have created significant uncertainty around the availability of key input material necessary to develop and place our renewable energy projects in service. In the U. S., solar industry supply chain issues include the pending U. S. Department of Commerce investigation on Antidumping and Countervailing Duties Anti Circumvention Petition filed by a domestic solar manufacturer (the DOC Investigation), the Uyghur Forced Labor Protection Act, Section 201 Tariffs and persistent general global supply chain and labor availability issues. The most prominent effect of these issues is the significant curtailment of imported solar panels and other key components required to complete utility scale solar projects in the U. S. Any available solar panels may not meet the cost and efficiency standards of our currently approved projects and the incremental cost may not be recoverable through customer rates. As a result of the challenges in obtaining solar panels, many solar projects in the U. S. have been delayed or canceled. As we are in the midst of a transition to an electric generation portfolio with more renewable resources, including solar, our projects are subject to the effects of these issues.',
  'chunk_char_count': 1475,
  'chunk_word_count': 221,
  'chunk_token_count': 368.75,
  'embedding': array([ 1.80599913e-02,  2.24035736e-02, -3.28406505e-02, -2.20126566e-02,
          1.12129152e-02, -2.02823225e-02,  2.89319456e-02, -1.52266165e-03,
         -4.59301809e-04, -4.11665905e-03,  3.60108875e-02,  7.39563033e-02,
          2.00368594e-02,  5.93284331e-02,  2.53946427e-02,  3.10788024e-02,
          8.50221589e-02,  1.83278900e-02,  1.55767880e-03, -7.40996236e-03,
         -2.59554926e-02, -8.17613117e-03, -1.47668766e-02,  1.80975012e-02,
         -2.19128914e-02,  1.35437753e-02, -2.25015804e-02, -3.08008697e-02,
```

# Conclustion

This project employs a two-step process to analyze 10-K financial disclosures. First, it embeds both the user's query and the relevant textual context from the disclosures, ensuring precise alignment of the inquiry with the authoritative source. Second, this contextually enriched query is processed through the GEMMA-7b-it language model. This approach allows for targeted responses directly addressing the specifics of the source of information. Of the 24 questions tested, half came back with ```The text does not mention < Query >, therefore I cannot answer this query."```. I found that adding examples to the base prompt ```shown in step 6``` improved the performance. However, the simple framework makes it difficult to get relevant response from the LLM when looking at a time range or a diverse set of  sectors. 


Integrating example-based prompts, as demonstrated in step 6, improved the model's performance. However, the simple framework sometimes fails to yield relevant responses, especially for queries covering various time frames or sectors. Focusing primarily on the utility sector, the approach could be broadened by using tools like LangChain to enhance textual analysis and enable extraction of visual data such as charts or images, enriching the analysis.

https://python.langchain.com/docs/integrations/retrievers/sec_filings/



# Refrence

Refrence: https://github.com/mrdbourke/simple-local-rag/tree/main

1. "Measuring Disclosure Using 8-K Filings"


https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3354252


* VDisc_Ct is the number of voluntary 8K items and associated exhibits
* VDisc_WC is the number ofwords within the voluntary 8K items and associated exhibits

2. "ESG In Corporate Filings: An AI Perspective"

Tie the corporate actions on the environment to the investor expectations.

https://arxiv.org/pdf/2212.00018.pdf
 
Used:
 * keywords corresponding to the SASB categories of ESG terms
   
Among the key findings are: 
 * lack of transparency among the ESG raters on how the scores are assigned;
 * lack of standards on how a particular concept is measured 
 * Questionable tradeoffs: high scores in one domain may offset very low scores in another area 
 * Absence of an overall score combining performance scores alongenvironmental, social and governance axes
 * lack of acknowledgement of stakeholder expectations leading to lower acceptance rates.
   

3. Capturing Firm Economic Events

https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4510212#:~:text=To%20better%20capture%20events%20that,participants'%20uncertainty%20about%20that%20performance.


* event-driven 8-K, Items (all 8-K items except Items 2.02, 7.01, 8.01, and 9.01)
* disclosure-driven, Items 2.02, 7.01, and 8.01 constitute 8-K items with a voluntary disclosure component

  
4.  Data Extraction and Preprocessing

Extract Relevant Sections: Identify and extract ESG-relevant sections from 8-K filings

Text Cleaning: Standardize formatting and remove non-essential elements.

Refrence: https://scholar.harvard.edu/jbenchimol/files/text-mining-methodologies.pdf

5.  Labeling Data for Sentiment Analysis

Develop a Labeling Guide: Define positive, negative, and neutral ESG sentiments.

Manual Labeling: Label a subset of filings for training the sentiment analysis model.

Refrence: https://aws.amazon.com/blogs/machine-learning/use-sec-text-for-ratings-classification-using-multimodal-ml-in-amazon-sagemaker-jumpstart/



## Source: 
1.  Hartzmark, Samuel M., and Kelly Shue. "Counterproductive Sustainable Investing: The Impact Elasticity of Brown and Green Firms." 1 Nov. 2022, SSRN, https://ssrn.com/abstract=4359282 or http://dx.doi.org/10.2139/ssrn.4359282.

2.  Dubner, Stephen J. “Are E.S.G. Investors Actually Helping the Environment?” Freakonomics Radio, no. 546, Freakonomics, LLC, 14 June 2023, https://freakonomics.com/podcast/are-e-s-g-investors-actually-helping-the-environment/.

3.  



## Related Projects
1. https://www.mdpi.com/2071-1050/12/13/5317
2. https://www.mdpi.com/2071-1050/14/14/8745
3. https://www.pm-research.com/content/pmrjesg/1/3/39
