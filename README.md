# GDPR proofing a dataset

In this project, I aim to ensure that the data is useable for a research project under the GDPR principles. To do so I clean and process the data to make it ready for analysis and so that it can be shared within the academic community for replication purposes. 

## Disclaimer
The data used in this project is fake and was generated for examination purposes by the University of Copenhagen. 

### The data 
The provided data is in the form of an excel file and contains three data sheets (see DG_Dataset.xlsx). The first sheet is 'Dataset 1 (Survey)'. This data collected using an online survey with informed consent of the participants. 'Dataset 2 (Twitter Data)' consists of comments about news sources obtained from the twitter API. Note that this was collected in 2019 when the platform was still named twitter, which is why I refer to it as such throughout this project. Lastly, 'Dataset 3 (Found dataset)' is a found dataset that was obtained from a news ... website. It is not specified exactly how this data was 'found'. For a full description of the variables in each of these datasets, see below: 'Variables before data cleaning'. 

### Data purpose
The proposed purpose of the data is to conduct a social data science project which examines .... 

### GDPR principles

In order to legally use this data, it is important to consider the GDPR principles. In the following, I explain how I aim to adhere to each one: 

1. **Lawfulness, Fairness, and Transparency**:
   - GDPR allows for the age of consent to be set by EU member-states (GDPR, 2016, 8(1)). To comply with the Danish Data Protection Act (2018), I include participants who are over the age of 13 at the time of the survey and keep only the comments of the participants who were over the age of 13 when the comments were posted.
   - To uphold the principle of transparency, participants must be informed of the data and analysis processes.
   - Potentially illegally obtained information (such as credit card numbers) cannot be used and must not be collected in the first place, which is why I remove this variable.

2. **Storage Limitation**: The data should only be stored as long as necessary and deleted after it has fulfilled its purpose. 

3. **Purpose Limitation**: I create income and birth year brackets to further prevent the possibility of reidentification to reduce granularity, thereby balancing anonymity and data utility.

4. **Data Minimisation**: Any data that is not necessary for the research purpose should be removed. In this case, it includes variables such as phone numbers and geolocation.

5. **Integrity and Confidentiality**: It is important to anonymize participants by erasing their names and instead identifying them with a randomly generated identifier. Additionally, to prevent the comments from being traced back to the data subject, it is important to paraphrase them before making the data available to any other party for replication or other purposes. To avoid a conflict with the principle of accuracy, this should be done in a way that the meaning of the original comment is not compromised. 

6. **Accuracy**: To ensure accuracy and recency, participants should be informed of what data beyond the survey will be used for analysis, not only for consensual purposes but also to give them the opportunity to rectify out-of-date or wrong information. 

7. **Accountability**: Alongside transparency on the data processing steps, it is important for the data processors to take full accountability for these actions. 


## Variables before data cleaning 

### Variables from Dataset 1 (Survey)
- `'Name'` - the participants' first and last name  
- `'Birth year'` - the participants' year of birth  
- `'Occupation'` - the current occupation practiced by the participant  
- `'Education'` - the highest level of education completed by the participant  
- `'Monthly income (DKK)'` - monthly income in Danish Krona  
- `'Political orientation'` - political orientation that the participant most identifies with  
- `'Religious belief'` - religion that the participant identifies with  
- `'"How much do you pay for online news subscriptions per month (in DKK)"?'` - the answer to this question as recorded in the survey  
- `'"On a scale from 1-5, how much do engage with news sources on social media?"'` - the answer to this question as recorded in the survey  

### Variables from Dataset 2 (Twitter Data)
- `'Comment #'` - number to identify the comment  
- `'Profile name'` - the Twitter username of the participant  
- `'date'` - the date which the comment was posted  
- `'comment'` - the content of the original comment as posted by the participant  

### Variables from Dataset 3 (Found dataset)
- `'Name'` - the first and last name of the participant  
- `'Date of purchase'` - the date which the subscription was purchased  
- `'Geolocation'` - the location of the participant at the time which the subscription was purchased  
- `'Credit card number'` - the number of the credit card used to purchase the news subscription  
- `'Telephone number'` - phone number registered at the purchase of the transaction  
- `'Subscription'` - the news source to which the participant subscribed  
- `'Number of Other Subscriptions'` - the amount of subscriptions purchased by the participant apart from the one named in `'Subscription'`  

---

## Variables after data cleaning

- `'Anonymized_name'` - randomly generated identifier  

### Variables (derived) from the online survey
- `'Occupation'` - the current occupation practiced by the participant  
- `'Education'` - the highest level of education completed by the participant  
- `'Political_orientation'` - political orientation that the participant most identifies with  
- `'Religious_belief'` - religion that the participant identifies with  
- `'Extent_of_engagement'` - answer to the question `"On a scale from 1-5, how much do you engage with news sources on social media?"`  
- `'Birth_year_bracket'` - birth year bracket in steps of ten years ranging to 2011 (the last bracket encompasses 11 years, namely 2000–2011)  
- `'Income_bracket_usd'` - monthly income bracket in steps of 2k ranging from 0 to 30k in USD  
- `'Price_of_subscriptions_in_usd'` - answer to the question `"How much do you pay for online news subscriptions per month (in DKK)?"`  

### Variable from Dataset 2 (Twitter Data)
- `'Comment_paraphrased'` - the paraphrased version of the original comment made with regards to a news source on X  

### Variable from the found data
- `'Subscription'` - news source that the participant is subscribed to  

## Steps to clean the data (also described in the ipynb file)
To comply with the GDPR articles mentioned above, I clean the data in the following way (see ipynb file for code):
1. Firstly, I load the data and merge the three sheets into one dataframe.
2. Concerning the 'Birth year' column, I remove participants born after 2012 and the comments of participants born after 2006. In this way I avoid including participants who are under the age of 13 in 2025 (the year in which the survey was conducted) and analyzing comments of participants who were under 13 in 2019 (the year in which the comments were posted). Next I create brackets of ten years to anonymize participants. The last bin consists of 11 years (2000-2011) to avoid the bin 2011-2019 only containing participants born in 2011. In this way I create the column 'birth_year_bracket'
3. To modify monetary variables, I first convert DKK (Danish Krona) to USD. This not only helps to ensure anonymity but also international interpretability. I do this for the columns 'income' and '...', creating the new columns '...' and '...' respectively. Moreover, I create brackets of 2k USD for income ranging form 0 to 30 USD monthly salary.  
4. Next, I generate random codes to replace participants real names.
5. To ensure that the comments cannot be traced back to the participants, it is important to paraphrase them. This has been done by prompting a GPT4 model to rephrase the content whilst maintaining the message of the text. I then manually copied these to use as replacements for the original comments.
6. Lastly I remove the unecessary data that may lead to reidentification or sensitive data that was presumably obtained without informed consent of the data subject. I remove the following columns: 'Date of purchase', 'Geolocation', 'Credit card number', 'income_usd', '"How much do you pay for online news subscriptions per month (in DKK)"?', 'Telephone number', 'Number of Other Subscriptions', 'date', 'Monthly income (DKK)', 'Name', 'Birth year', 'comment', and 'Comment #'. Additionally, I clean up the column names by removing quotation marks, simplifying titles, capitalizing, replacing symbols with words, replacing spaces with underscores, and making the 'Anonymized_name' identifier the first column before saving the dataset. 

## Ethical considerations
There are multiple ethical considerations to keep in mind in this process:
1. Firstly, it is important to obtain informed consent from all participants retrospectively concerning all data. Twitter comments, for example, can be obtained without the knowledge of the data subjects, which arguably diminishes their autonomy and control of the data process. 
2. Secondly, paraphrasing comments is highly debated, as it is often impossible to convey the exact same message in different words. For this reason, it is important to obtain participants' approval of these paraphrased alternatives.
3. Concerning 'Dataset 3 (Found dataset)', there is no specification on how the data was 'found'. Should it have been obtained in an illegal manner, it is important to remove this dataset, therefore also warranting the deletion of the 'Subscription' variable in the GDPR_proofed_data.
4. Lastly, including participants age 13, whilst legal in Denmark, is debatable. Some may argue that informed consent is only valid from the age of 16. This is a highly subjective topic and important to keep in mind in any data science project. 

## References
* General Data Protection Regulation. (2016). Regulation (EU) 2016/679 of the European Parliament and of the Council of 27 April 2016 on the protection of natural persons with regard to the processing of personal data and on the free movement of such data, and repealing Directive 95/46/EC (General Data Protection Regulation). Official Journal of the European Union, L 119, 1–88. https://eur-lex.europa.eu/eli/reg/2016/679/oj
* Danish Data Protection Act, § 6(2) (2018). https://www.datatilsynet.dk/media/7753/danish-data-protection-act.pdf
