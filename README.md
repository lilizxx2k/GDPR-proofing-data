# GDPR proofing a dataset

## Disclaimer
The data used in this project is fake and was generated for examination purposes by the University of Copenhagen. 
## Project description 
In this project, I will ensure that the data is useable under GDPR guidelines. 
### Data origin
The excel file contains three data sheets. The first (...) is data collected using an online survey. The second (...) is data scraped from X and the third (...) is a found dataset that was obtained from a news ... website. 
### Data purpose
The proposed purpose of the data is to conduct a social data science project which examines .... 
### GDPR guidelines
In order to legally use this data, it is important to consider various 
1. Anonymity: brackets
2. 

## Variables before data cleaning 

## Variables after data cleaning
* Anonymized_name - randomly generated identifier
### Variables (derived) from the online survey
* Occupation - 
* Education - 
* Political_orientation - political orientation that the participant most identifies with
* Religious_belief - religion that the participant identifies with
* Extent_of_engagement - answer to the question '...' 
* Birth_year_bracket - birth year bracket in steps of ten years ranging from to 2011 (the last bracket encompasses 11 years, namely 2000-2011)
* Income_bracket_usd - monthly income bracket in steps of 2k ranging from 0 to 30k usd
* Price_of_subscriptions_in_usd - answer to the question '...'
### Variable from the found data
* Subscription - news source that the participant is subscribed to
### Variable from the data scraped from X
* Comment_paraphrased - the paraphrased version of the original comment made with regards to a news source on X 

## Steps to clean the data (also described in the ipynb file)
To comply with the GDPR articles mentioned above, I clean the data in the following way (see ipynb file for code):
1. Load and merge the data.
2. Concerning the 'Birth year' column, I first remove participants born after 2012 and the comments of participants born after 2006. In this way I avoid including participants who are under the age of 13 in 2025 (the year in which the survey was conducted) and analyzing comments of participants who were under 13 in 2019 (the year in which the comments were posted). Next I create brackets of ten years to anonymize participants. The last bin consists of 11 years (2000-2011) to avoid the bin 2011-2019 only containing participants born in 2011. In this way I create the column 'birth_year_bracket'
3. To modify monetary variables, I first convert DKK (Danish Krona) to USD. This not only helps to ensure anonymity but also international interpretability. I do this for the columns 'income' and '...', creating the new columns '...' and '...' respectively. Moreover, I create brackets of 2k USD for income ranging form 0 to 30 USD monthly salary.  
4. Next, I generate random codes to replace participants real names.
5. To ensure that the comments cannot be traced back to the participants, it is important to paraphrase them. This has been done by prompting a GPT4 model to rephrase the content whilst maintaining the message of the text. I then manually copied these to use as replacements for the original comments.
6. Lastly I remove the unecessary data that may lead to reidentification. Additionally, I clean up the column names by removing quotation marks, simplifying titles, capitalizing, replacing symbols with words, replacing spaces with underscores, and making the 'Anonymized_name' identifier the first column before saving the dataset. 

## Ethical considerations

