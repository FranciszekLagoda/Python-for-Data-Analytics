# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.
```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter Poland Jobs

To focus my analysis on the Poland job market, I apply filters to the dataset, narrowing down to roles based in Poland.

```python
df_PL = df[df['job_country'] == 'Poland']

```
# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:

## 1. What are the most demanded skills for the top 3 most popular data roles?
To find the most demanded skills for the data roles that I am interested in. I filtered out those positions and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I'm targeting. 

View my notebook with detailed steps here: [2_Skills_Count.ipynb](2_Skills_Count.ipynb).

### Visualize Data
```python
fig, ax = plt.subplots(len(job_list),1)
sns.set_theme(style='ticks')

for i, job_title in enumerate(job_list):
    df_plot = df_skills_percet[df_skills_percet['job_title_short']==job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)

plt.show()
```
### Results
![Visualization of the top skills](images/skills_demand.png)

### Insights
 * SQL is a foundational skill vital for **data querying**, management, and manipulation, highlighting its universal importance across all roles that require data handling.
* Excel remains a critical tool for **data analysis** and **reporting**, emphasizing its continued relevance in data-centric roles.
* Python’s significance reflects a growing trend toward programming-based analysis and **automation**, particularly in **advanced analytics** and machine learning.
* Power BI's and Tableau use underscores the importance of **data visualization** tools for transforming raw data into **actionable insights**.
R’s usage indicates **a niche** but essential skill for **statistical analysis** and **data modeling**, particularly in specialized analytical tasks.
* The rising need for cloud platforms (Azure & AWS) illustrates the shift toward **scalable** data solutions and emphasizes the importance of **cloud-based** data processing skills.

## 2. How are in-demand skills trending for Data Analysts?
### Visualise Data
```python
sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.despine()
#Change values to %
from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

#Add data labels
for skill in df_plot.columns:
    plt.text(11.2, df_plot[skill].iloc[-1], skill)

plt.show()
```

### Result
![Visual](images/skills_trend.png)

### Insights
* SQL remains the most demanded skill, but we can see a gradual decrease in demand throughout the year.
* Excel as a basic analytical tool remains in second position, but we can see a decreasing trend in the last year.
* Python is becoming increasingly popular, almost catching up to Excel by the end of the year.
* Both Tableau and Power BI are less demanded in Data Analyst job postings, but we can see a high percentage increase in demand for Power BI.
* R is less popular than Python and does not show as dynamic a growth as its competitor.
* In summary, all the most popular skills for Data Analytics seem to be viable to learn, but among coding languages, Python has an advantage.

## What is a salary distribution for data roles?
### Visualise data
```python
# Plot data
sns.boxplot(data=df_plot, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

#Format axes
ax = plt.gca()
ax.set_xlim(0,500000)
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

#Add labels
plt.title('Salary distribution of jobs of my interest', fontsize=16)
plt.xlabel('Salary year average')
plt.ylabel('')
plt.show()
```

### Result
![vis](images\salary_distribution.png)

### Insights
* Senior roles generally have higher salaries
* Data Scientist is the highest-paying role. Even junior data analyst can earn more than other roles.
* Business analyst is the lowest-paying roles.
* In summary, more experienced roles with higher technical skills have higher pay.