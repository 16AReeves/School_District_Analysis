# School_District_Analysis
---
## Performing analysis on School Districts to uncover trends
---
### Analysis on School district funding and standardized testing scores
---
#### Background of Project
---
##### Analysis of difference school metrics was undertaken to analyze for trends in reading and math scores along with district's key metrics. This was to help Maria, a chief data scientist for a city school district, analyze student funding and test scores. In the original module, all data was analyzed to help make these decisions. The data was corrected, to take out ninth graders test scores from Thomas High School, due to academic dishonesty. This analysis was then used to help the district make decisions based on the findings below.
---
#### Results
---
##### The first step of this analysis was to take out the dishonest grades from the ninth graders test scores from Thomas High School, this was done by cleaning the code. The code used is below:
```
student_data_df.loc[(student_data_df["grade"]=="9th") & (student_data_df["school_name"]=="Thomas High School") , "reading_score"] = np.nan

student_data_df.loc[(student_data_df["grade"]=="9th") & (student_data_df["school_name"]=="Thomas High School") , "math_score"] = np.nan

school_data_complete_df = pd.merge(student_data_df, school_data_df, how="left", on=["school_name", "school_name"])

school_data_complete_df.head()
```
##### This section of code locates all of the 9th graders test scores at Thomas High School and replaces them with Null values, or NaN. This allowed for a cleaner data set and this is where most of the original module code and the challenge code merged from here on out. Let's compare the two codes and how the results differed after cleaning the data below: 
* ##### The below table is a summary 
---
#### Summary
---
##### summarize the four changes in the updated school district analysis after scores were replaced with NaNs. 
