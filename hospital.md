[<img src="images/Blue Modern Minimalist Simple Article LinkedIn Sponsored Contentsql .png"/>](https://www.linkedin.com/pulse/exploring-world-banking-data-bank-insights-sql-megan-easton-qe2re/)
# Exploring the World of Banking Data: The World Bank Insights with SQL


When I came to this project in the Data Career Jumpstart's Data Analytics Accelerator, I felt a little more excited for this project than previous ones for two reasons: (1) I already had some experience with SQL (_and loved it_), and (2) I've been working in banking for almost six years, and was excited to see what banking data **actually** looked like. For this project, data came from the World Bank's International Development Association (IDA), which provides grants and low-interest loans to help countries invest in their futures, improve lives, and create safer, more prosperous communities around the world. I used [csvfiddle](https://csvfiddle.io) to run my queries.

<br>


#### Questions to Answer
1. How many loans does each country have?
2. What is the most expensive project?
3. Which countries have the highest service charge rate?
4. What is the most common credit status?
5. Which countries have the highest amount of cancelled debt?

#### Dataset Details
For my analysis, I used a dataset sourced from the World Bank, which contained 1.4 million rows and 30 columns of data. This dataset was as of May 31, 2025, and included typical lending information such as the country, borrower details, disbursement information, and various status indicators. The size and detail of this dataset made it a perfect fit for my exploratory analysis, and was great for using SQL (as opposed to Excel) based on its size. The dataset can be found [here.](https://financesone.worldbank.org/ida-statement-of-credits-grants-and-guarantees-historical-data/DS00976)

---

## Analysis
Since this dataset was too large to be imported to Excel, I didn't have an opportunity to really clean the data as I would have liked. So I started with a limited "SELECT *" statement to get a feel for the data. This revealed that there were numerous row entries bearing the same loan ID, but with different dates (usually once a month, and presumably regular payments or updates). As a result, in many of my queries I used the DISTINCT operator to try to only have one entry per loan ID. Because of the use of DISTINCT, you will also see many AVG operators as well. Once I had this down, I was able to really get into the data.
<br><br>
The first query I ran was a simple Count, to figure out the number of rows. <br><br>
<img src="images/Big Count Cod.png"/> <br><br>
Which returned this result:
<br><br>
<img src="images/Big Count Result.png">
<br><br>
This also served to reinforce that the use of the DISTINCT operator would be important throughout.
<br><br>
Then I dove right in to determining the answer to my first question - how many loans does each country have? <br><br>
<img src="images/loans by country & commitment code.png"> <br><br>
Which returned the following: <br><br>
<img src="images/loans by country & commitment result.png"> <br><br>
<img src="images/loans by country bottom.png"> <br><br>
This query returned a total of 143 rows, and showed that India had 442 loans with World Bank's IDA program and an average loan commitment of $117,488,661!! Bangladesh had the next highest number of unique loan IDs, but Ethiopia had the next highest average loan commitment. Multiple countries/economies had only one loan, but St. Kitts and Nevis had the smallest amount at $1.5 million.
<br><br>
Next, I looked into the various projects, limiting the results to the top 5: <br><br>
<img src="images/project code.png"><br><br>
Which returned this: <br><br>
<img src="images/project result.png"><br><br>
The PEACE in Ukraine project had the most borrowed funds out of all of them, with _**$1,618,604,651**_, followed by Padma Bridge. Numbers 3 and 4 appeared to be the same project - National Rural Livelihoods in India - just with a slight variation in the project name, and fifth was Nigeria's National Social Safety Net Program. <br><br>
Then I looked into which countries had the highest service charge, also limited to the top 5: <br><br>
<img src="images/SERVICE CHARGE CODE.png"><br><br>
Which returned the following:<br><br>
<img src="images/service charge result.png"><br><br>
St. Kitts and Nevis, which you'll remember had the **smallest** average loan amount, had the **highest** average service charge by far! The next three results were Macedonia (another duplicate) and North Macedonia, and fifth was Albania.
<br><br>
Next, I worked on determining the distribution of credit statuses:<br><br>
<img src="images/sort by credit status code.png"><br><br>
Which returned:<br><br>
<img src="images/sort by credit status result.png"><br><br>
This showed that most loans' credit statuses were either disbursing (in the process of funds being distributed to the borrower, possibly over a period of time), repaying (funds being repaid to the lender), or disbursed (funds fully sent to the borrower, but not yet repaying). It does seem like there may be some duplicate credit statuses here - is there a difference between disbursed and fully disbursed? Fully repaid and repaid? I am not familiar with the inner workings of World Bank, so I can't be sure, but I would recommend consolidating credit statuses if possible.<br><br>
Finally, I looked into countries with the most cancelled debt, but from two angles - (1) the highest USD amount of cancelled debt and (2) the highest number of cancelled loan IDs. First is the highest amount of cancelled debt:<br><br>
<img src="images/country $ cancelled code.png"><br><br>Which gave us:<br><br>
<img src="images/country $ cancelled result.png"><br><br>
This revealed that India had $13,705,173,117 in cancelled debt. Please note that this number is a SUM function, rather than an AVG like I have been using for many of my queries.<br><br>
Next, looking at the highest number of cancelled loans:<br><br>
<img src="images/country num cancelled code.png"><br><br>
Which returned:<br><br>
<img src="images/country by num cancelled result.png"><br><br>
For this, I used a CASE operator to effectively count the number of entries for each country with the credit status as Fully Cancelled or Cancelled. This does contain the duplicate Loan ID entries, so I view these as percentages more than hard numbers. Of 1,432,916 total rows, 2,341 or 0.16% of those were cancelled loans from Pakistan. Interestingly, India is not in the top 16 results for the highest number of cancelled loans, despite having the highest sum of cancelled debt. But since India also has the highest average dollar amount per loan ID, it likely needs fewer cancelled loans to rack up a higher amount of cancelled dollars. <br>

---

### Conclusions
In summary, my analysis of the data found the following:

1. India had the highest number of unique loan IDs and St. Kitts and Nevis had the least.
2. The PEACE in Ukraine project had the highest loan amount.
3. St. Kitts and Nevis had the highest service charge.
4. Most loans were disbursing, repaying or disbursed.
5. India had the highest dollar amount of cancelled debt, and Pakistan had the highest number of rows with the status as cancelled.

I think this dataset offers opportunity for deeper investigation: How can the World Group reduce risk to avoid cancelled debt, while still providing financial support? Do interest rates on other loans within these relationships justify the amount of cancelled debt? These questions may not apply to the IDA program specifically, but I believe these are questions a regular bank working with regular borrowers would have, and would be seeking answers to.
<br><br>
As always, thank you for reading! Feel free to leave your thoughts or questions, or connect with me on [LinkedIn.](https://www.linkedin.com/in/megan-
