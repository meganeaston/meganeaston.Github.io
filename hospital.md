[<img src="images/sql 2 image.png"/>](https://www.linkedin.com/pulse/exploring-hospital-data-sql-relationships-between-los-megan-easton-ommue/?trackingId=WdjrS%2Fd2myKqkzeh4fgL9Q%3D%3D)
# Exploring Hospital Data with SQL: Relationships Between LOS, Procedures, and Demographics


Welcome to my next data project! We're back at it again with SQL, but analyzing hospital data this time. As you _may_ know by now, I am a SQL lover-girl, so it was exciting to get to continue using a tool I enjoy. But looking at data from the healthcare industry adds another layer to the analysis, since I think the importance of healthcare access and affordability **cannot** be stated enough. Hospitals themselves face many challenges, including ensuring quality of care, managing costs, and managing hospital resources. Understanding factors like readmittance rates or the average length of stay (LOS) can help them make better decisions to provide better, more cost-effective care. 

<br>
In this article, I’ll share some key findings from my analysis of hospital data. You’ll learn about the relationships between the number of procedures, length of stay, and readmittance rates, and how these vary across demographic groups.
<br>

#### About the Data
For my analysis, I used a dataset from Kaggle that consisted of two tables: One table held demographic information, while the other contained data on patients’ hospital stays including the LOS, number of procedures, readmittance, and more. The dataset is comprised of 51 columns and (as revealed by an initial COUNT(*) query) 101,766 rows, making it rich with information for analysis. The dataset can be found [here.](https://www.kaggle.com/code/iabhishekofficial/prediction-on-hospital-readmission/data?select=diabetic_data.csv)

---

## Analysis
One key factor hospitals may look at for insights into the quality of patient care is the amount of time a patient spent in the hospital. To visualize the average amount of time each patient spent in the hospital, I created a histogram. 
<br><br>
<img src="images/histogram code.png"/> <br><br>
Which yielded this, where each * equals 100 patients:
<br><br>
<img src="images/histogram result.png">
<br><br>
From this visual, it is clearly evident that most patients are staying 1-4 days in the hospital, with 3 days as the most common. Generally, patients would prefer to get out of the hospital faster so this could be a good result. Also, as one patient leaves the hospital, this opens up space for another individual to be cared for, so a quicker turnaround rate can yield more patients being seen. However, hospitals would need to be very intentional to make sure they are not discharging patients prematurely, as this could lead to poorer health outcomes for the patient and could potentially lead to the patient's readmission to the hospital.
<br><br>
Next, I looked at which medical specialties have the most procedures, since procedures can be costly for both the patient and the hospital: <br><br>
<img src="images/top proc code.png"> <br><br>
Which returned the following: <br><br>
<img src="images/top proc result.png"> <br><br>
I filtered the data to only include specialties with a total number of procedures above 50 to make sure the sample size of each specialty was appropriate. This revealed that thoracic surgery had the highest average number of procedures per patient, but there were the highest number of cardiology procedures overall. I chose not to investigate further with medical specialties, but if one wanted to look further I would also want to consider: (1) How do surgical vs. non-surgical specialties compare? (2) Which specialties have longer LOS? (3) Are certain medical specialities more likely to have a patient be readmitted?
<br><br>
I then moved on to see if there was a correlation between the number of lab procedures a patient has and their average LOS. First, I looked at some aggregate functions of the number of lab procedures to determine the range of results: <br><br>
<img src="images/establishing range code.png"><br><br>
<img src="images/establishing range result.png"><br><br>
I used the results to help determine my parameters for my CASE WHEN statement in the next query: <br><br>
<img src="images/lab proc freq code.png"><br><br>
Which showed this: <br><br>
<img src="images/lab proc freq result.png"><br><br>
This revealed that, yes, there is a correlation between the amount of lab procedures a patient receives and the amount of time they are admitted to the hospital. Based on the data, you can't determine causality, but you can at least see that a relationship exists.<br><br>
This result prompted me to look a little deeper - how do LOS, procedures, and medications vary across demographic groups? Do they have any relation to being readmitted to the hospital? To look into this, I created two tables to look at how these changed across races and age groups:<br><br>
<img src="images/big table code.png"><br><br>
Which returned:
<br><br><img src="images/big table result.png"><br><br>
A second table was made using very similar code, but displaying and grouped by age instead:<br><br>
<img src="images/big table age result.png"><br><br>
In these results some correlations presented themselves, but I decided each table needed to be split up to look at fewer factors and to get a more clear view of their relationships. So I made a total of four tables - procedures grouped by race and age, and readmission status also grouped by race and age. Here is the code for looking at procedures:<br><br>
<img src="images/race procedure code.png"><br><br>
Which gave:<br><br>
<img src="images/race procedure result.png"><br><br>
And the following, when grouped by age:<br><br>
<img src="images/age procedures result.png"><br><br>
In these two tables we can see a couple interesting things:
- African American patients had the longest average LOS and highest average number of lab procedures, but second lowest number of procedures.
- Asian patients had the shortest average LOS, as well as the fewest average lab procedures and fewest average medications.
- Generally, the older you are, the longer your hospital stay. The only exception was that patients aged 80-90 stayed longer than those aged 90-100.
- When grouped by age, the number of procedures and number of medications have a bell curve, with people in middle age groups receiving the highest average amount of procedures and medications.
- The number of lab procedures seems to be pretty similar among all age groups, but younger age groups do receive slightly fewer lab procedures on average.
- Overall, patient groups with longer LOS tended to have more lab procedures and medications, but the number of procedures was less consistent.<br><br>
Next, looking at the tables focusing on readmission:<br><br>
<img src="images/age readmitted code.png"><br><br>
Which yielded:<br><br>
<img src="images/Age readmitted result.png"><br><br>
And the following, when grouped by race:<br><br>
<img src="images/race readmitted rsult.png">
From these two tables I was able to make some more observations:<br><br>
  <LI>Younger age groups generally have a higher percentage of patients not readmitted, but they also have a smaller sample size compared to middle and older age groups.</LI>
  <LI>Patients aged 20-30 had the highest percentage readmitted within 30 days, and patients aged 80-90 had the highest percentage readmitted after 30 days.</LI>
  <LI>Caucasian patients had the highest percentage readmitted (both within 30 days and after 30 days), as well as the largest sample size</LI>
<LI>Asian patients had the highest percentage not readmitted, as well as the second lowest percentage readmitted within 30 days and lowest percentage readmitted after 30 days. They were also the smallest sample size.</LI>
<LI>When grouped by race, patients who stayed longer had a higher likelihood of being readmitted. However, this could be a sample size issue as larger sample sizes had longer LOS and a higher proportion of patients being readmitted.</LI>
<LI>When grouped by age, the percentage of patients being readmitted was not significantly related to LOS, though younger patients did tend to have a shorter LOS and were less likely to be readmitted to the hospital.</LI><br>

---

### Main Findings

1. Most patients stayed in the hospital between 1-4 days, with 3 days being the most common.
2. A longer LOS was related to a higher number of lab procedures.
3. Demographic groups with longer LOS tended to have more lab procedures and medications, but the number of procedures was less consistent.
4. When grouped by race, patients who stayed in the hospital longer had a higher likelihood of being readmitted. When grouped by age, the percentage of patients being readmitted was not significantly related to LOS.

I think there is lots of opportunity for further investigation here, outside of what I mentioned above relating to medical specialties. When looking at this dataset, there is much we don’t know about these patients that could be affecting LOS and readmission. Are there additional factors at play that affect these key markers? Was the hospital understaffed? Are there complicating factors that require observation, and therefore a longer LOS? A patient may want to leave earlier than recommended by doctor and end up being readmitted to the hospital at a later time. Patients could have additional diagnoses or allergies that require special considerations regarding the kind of treatment given to them. And most of all: _every single person is just different_.<br><br>
I think more data could be gathered to try to determine how socio-economic factors play a role in these as well as implicit bias within the medical system. Findings from this could potentially help improve patients' quality of care significantly, decrease turn around times and thereby decrease costs to hospitals when they don't have to readmit patients who received sub-par care.
<br><br>
Once again, thank you so much for reading! Please reach out to connect with me on [LinkedIn](https://www.linkedin.com/in/megan-easton2/) and share your thoughts or questions about this project!
