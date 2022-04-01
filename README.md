# School_District_Analysis
---
## Performing analysis on School Districts to uncover trends
---
### Analysis on School district funding and standardized testing scores
---
### Background of Project
---
##### Analysis of difference school metrics was undertaken to analyze for trends in reading and math scores along with district's key metrics. This was to help Maria, a chief data scientist for a city school district, analyze student funding and test scores. In the original module, all data was analyzed to help make these decisions. The data was corrected, to take out ninth graders test scores from Thomas High School, due to academic dishonesty. This analysis was then used to help the district make decisions based on the findings below.
---
### Results
---
##### The first step of this analysis was to take out the dishonest grades from the ninth graders test scores from Thomas High School, this was done by cleaning the code. The code used is below:
```
student_data_df.loc[(student_data_df["grade"]=="9th") & (student_data_df["school_name"]=="Thomas High School") , "reading_score"] = np.nan

student_data_df.loc[(student_data_df["grade"]=="9th") & (student_data_df["school_name"]=="Thomas High School") , "math_score"] = np.nan

school_data_complete_df = pd.merge(student_data_df, school_data_df, how="left", on=["school_name", "school_name"])

school_data_complete_df.head()
```
##### This section of code locates all of the 9th graders test scores at Thomas High School and replaces them with Null values, or NaN. This allowed for a cleaner data set and this is where most of the original module code and the challenge code merged from here on out. Let's compare the two codes and how the results differed after cleaning the data below: 
* ##### The below table is a summary of the district before and after the correction for academic dishonesty:
District Summary Before Correction: | District Summary After Correction:
--- | --- |
![Module_Code_District_Summary](https://user-images.githubusercontent.com/98365963/161321318-1267a69c-39de-4e44-be58-5b542b483e48.png) | ![Challenge_Code_District_Summary](https://user-images.githubusercontent.com/98365963/161321380-c043492c-e5c6-4d97-995e-25862ca7ccb3.png)
##### The change in the district summary isn't strictly significant. Between the two graphs, the math, reading, and overall passing percentages changed by 0.1%. From this, we can conclude that taking out the ninth graders scores had little effect on the overall district summary. This chart was coded the following way:
```
# Create a DataFrame
district_summary_df = pd.DataFrame(
          [{"Total Schools": school_count, 
          "Total Students": student_count, 
          "Total Budget": total_budget,
          "Average Math Score": average_math_score, 
          "Average Reading Score": average_reading_score,
          "% Passing Math": passing_math_percentage,
         "% Passing Reading": passing_reading_percentage,
        "% Overall Passing": overall_passing_percentage}])



# Format the "Total Students" to have the comma for a thousands separator.
district_summary_df["Total Students"] = district_summary_df["Total Students"].map("{:,}".format)
# Format the "Total Budget" to have the comma for a thousands separator, a decimal separator and a "$".
district_summary_df["Total Budget"] = district_summary_df["Total Budget"].map("${:,.2f}".format)
# Format the columns.
district_summary_df["Average Math Score"] = district_summary_df["Average Math Score"].map("{:.1f}".format)
district_summary_df["Average Reading Score"] = district_summary_df["Average Reading Score"].map("{:.1f}".format)
district_summary_df["% Passing Math"] = district_summary_df["% Passing Math"].map("{:.1f}".format)
district_summary_df["% Passing Reading"] = district_summary_df["% Passing Reading"].map("{:.1f}".format)
district_summary_df["% Overall Passing"] = district_summary_df["% Overall Passing"].map("{:.1f}".format)

# Display the data frame
district_summary_df
```
##### The above code passes in all of the previous dataframes created using mathematics and complies it into one district summary. The code below that formats the table to make the output nicer to read in the challenge code. Then the dataframe is shown as seen above. The math functions used in the previous dateframes are: `.count()`, `.mean()`, and `.sum()`.
---
* ##### The below table summarizes the schools before and after the correction for academic dishonesty:
School Summary Before Correction: | School Summary After Correction:
--- | --- |
![Module_Code_School_Summary](https://user-images.githubusercontent.com/98365963/161323020-79f63356-736b-491c-b252-15b7cb391663.png) | ![Challenge_Code_School_Summary](https://user-images.githubusercontent.com/98365963/161323045-94b92fe6-7968-4df2-a594-aebee1b0ad51.png)
##### The change in the overall school summary also isn't entirely significant. All of the percentages, other than the overall passing percentage and passing reading percentage, decreased by 0.1% or less. The overall passing percentage and passing reading percentage decreased by 0.3% or less. Looking at the comparsion, the overall school summary was changed only slightly. The code to create this chart is as follows:
```
# Determine the School Type
per_school_types = school_data_df.set_index(["school_name"])["type"]

# Calculate the total student count.
per_school_counts = school_data_complete_df["school_name"].value_counts()

# Calculate the total school budget and per capita spending
per_school_budget = school_data_complete_df.groupby(["school_name"]).mean()["budget"]
# Calculate the per capita spending.
per_school_capita = per_school_budget / per_school_counts

# Calculate the average test scores.
per_school_math = school_data_complete_df.groupby(["school_name"]).mean()["math_score"]
per_school_reading = school_data_complete_df.groupby(["school_name"]).mean()["reading_score"]

# Calculate the passing scores by creating a filtered DataFrame.
per_school_passing_math = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)]
per_school_passing_reading = school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)]

# Calculate the number of students passing math and passing reading by school.
per_school_passing_math = per_school_passing_math.groupby(["school_name"]).count()["student_name"]
per_school_passing_reading = per_school_passing_reading.groupby(["school_name"]).count()["student_name"]

# Calculate the percentage of passing math and reading scores per school.
per_school_passing_math = per_school_passing_math / per_school_counts * 100
per_school_passing_reading = per_school_passing_reading / per_school_counts * 100

# Calculate the students who passed both reading and math.
per_passing_math_reading = school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)
                                               & (school_data_complete_df["math_score"] >= 70)]

# Calculate the number of students passing math and passing reading by school.
per_passing_math_reading = per_passing_math_reading.groupby(["school_name"]).count()["student_name"]

# Calculate the percentage of passing math and reading scores per school.
per_overall_passing_percentage = per_passing_math_reading / per_school_counts * 100
```
##### The above code is how the averages and percentages were created from the data. The most notable functions are `.value_counts()` and `.groupby()`. The `.value_counts()` function counts all of the unique names per school to create a complete student count. The `.groupby()` function allows data to be summarized by taking two columns, and applying math to get a new dataframe. To complete the code, the summary chart was formatted similar to the formatting from the district summary.
---
* ##### Replacing the ninth graders math and reading scores at Thomas High School had little effect when compared to the other schools outcomes. Before removing the scores, Thomas High School was ranked second among the district and that didn't change after removing the scores. This can be seen in the below table:
Top 5 Schools Before Correction: | Top 5 Schools After Correction:
--- | --- |
![Module_Code_Top_5_Schools](https://user-images.githubusercontent.com/98365963/161325052-9e7efa00-4e3e-44ce-bd84-d8e90c592afc.png) | ![Challenge_Code_Top_5_Schools](https://user-images.githubusercontent.com/98365963/161325080-130389ec-72c7-47db-b0a3-21f619828f50.png)
##### As seen above, Thomas High Schools ranking didn't change after the correction. The correction had no effect on Thomas High Schools overall performance. To get this chart, the following code was used: 
```
top_schools = per_school_summary_df.sort_values(["% Overall Passing"], ascending=False)

top_schools.head()
```
##### This section of code sorts the values from the % Overall Passing column into descending order and places the results into a new dataframe. 
---
* ##### Below is a summary of the change in the overall scores and metrics after removing the ninth graders scores from the data:
  * ##### Math Scores by Grade
Math Scores by Grade Before Correction: | Math Scores by Grade After Correction:
--- | --- |
![Module_Code_Math_By_Grade](https://user-images.githubusercontent.com/98365963/161326276-04c57515-17ee-482a-9860-bb4fd16d2c6a.png) | ![Challenge_Code_Math_By_Grade](https://user-images.githubusercontent.com/98365963/161326306-49b885f2-d4a6-42f4-8c86-72d0d474abc4.png)
  * ##### Reading Scores by Grade
Reading Scores by Grade Before Correction: | Reading Scores by Grade After Correction:
--- | --- |
![Module_Code_Reading_By_Grade](https://user-images.githubusercontent.com/98365963/161326893-e39c805d-7520-42f3-b30a-d6bf293bd67a.png) | ![Challenge_Code_Reading_By_Grade](https://user-images.githubusercontent.com/98365963/161326915-1838d3c5-3c06-4800-bb4b-c0e4ecef98cc.png)
  * ##### Scores by School Spending
Scores by School Spending Before Correction: | Scores by School Spending After Correction:
--- | --- |
![Module_Code_Spending_Summary](https://user-images.githubusercontent.com/98365963/161327051-597e7313-4a1d-4b13-b957-fab8d9b7ed60.png) | ![Challenge_Code_Spending_Summary](https://user-images.githubusercontent.com/98365963/161327073-6a0f42ad-5b0c-4e84-99dc-63fb1427cccb.png)
  * ##### Scores by School Size
Scores by School Size Before Correction: | Scores by School Size After Correction:
--- | --- |
![Module_Code_Scores_By_School_Size](https://user-images.githubusercontent.com/98365963/161327168-71e1c549-a79f-4eb4-a819-173652cd4393.png) | ![Challenge_Code_Scores_By_School_Size](https://user-images.githubusercontent.com/98365963/161327182-a72de8de-fcb1-43df-bc9c-50a7c82823b5.png)
  * ##### Scores by School Type
Scores by School Type Before Correction: | Scores by School Type After Correction:
--- | --- |
![Module_Code_Scores_By_School_Type](https://user-images.githubusercontent.com/98365963/161327308-5ae24acb-515b-4eea-913d-d39f1746694e.png) | ![Challenge_Code_Scores_By_School_Type](https://user-images.githubusercontent.com/98365963/161327340-860882a9-9d2c-41da-a711-c64095d3354e.png)
##### The above tables and charts show the summary of the changes in math and reading scores by grade, scores by school spending, scores by school size, and scores by school type. The math and reading scores by grade chart did not change other than the fact that the ninth graders scores are summarized by a NaN, meaning the data for that group is blank. The scores by school spending chart had no change in the overall resulting scores. The scores by school size chart had no change in the overall resulting scores. The scores by school type chart had no change in the overall resulting scores.  
--- 
### Summary
---
##### After taking out the ninth graders scores, the overall district summary scores changed by less than 0.1% or less. The overall school summary scores changed by 0.3% or less. The overall performance from Thomas High School did not change, since the ranking for the high school among the other schools had no change. The overall scores based on math and reading, school size, school spending, and school type had no change after the ninth graders were removed. 
