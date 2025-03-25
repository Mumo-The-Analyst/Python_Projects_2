# The Analysis

## 1. What are the most demanded skills for the top 3 most popular data roles in the UK?

To find the most demanded skills for the top 3 most popular data roles in the UK, i filtered out those positions by which ones were the most popular, and got the top 5 skills for these top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills i should pay attention to depending on the role i'm targeting.

View my notebook with detailed steps here: [2_Skill_Demand.ipynb](3_Project\2_Skills_Demand.ipynb)

## Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)
sns.set_theme(style='ticks')

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 78)

    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')

    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in United Kingdom Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix the overlap
plt.show()
```

### Results

![Visualization of Top Skills for Data Nerds in the UK](3_Project\images\skills_demand_all_data_roles_in_UK.png)

### Insights
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (69%) and Data Engineers (55%).
- SQL is the most requested skill for Data Analyst and Data Scientists, with it in over half of the job postings for both roles. For Data Engineers, SQL is the most sough-after skill, appearing in 60% of job postings.
- Data Engineers require more specialised technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau)

## 2. How are in-demand skills trending for Data Scientists in the UK?

### Visualize Data

```python
# Plotting the skills trends using Seaborn
df_plot = df_DA_UK_percent.iloc[:, :5]

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Scientists in the UK')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(11.2, df_plot.iloc[-1, i], df_plot.columns[i])
plt.show()
```
### Results

![Trending Top Skills for Data Scientists in the UK](3_Project\images\trending_top_skills.png)

### Insights
- Python remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
-SQL experienced a slight increase in May.

## 3. How well do jobs and skills pay for Data Jobs in the UK?

### Salary Analysis for Data Nerds

#### Visualize Data
```python
# Plotting boxplots using Seaborn
sns.boxplot(data=df_UK_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
plt.xlabel('Yearly Salary(USD)')
plt.ylabel('')
plt.title('Salary Distribution in the United Kingdom')
plt.xlim(0, 300000)
ticks_x = plt.FuncFormatter(lambda y, pos:f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
#### Results
![Salary Distribution of Data Jobs in the UK](3_Project\images\salary_analysis_UK.png)

#### Insights
- There is a significant variation in salaries for different job titles. Senior Data Scientists, Senior Data Engineers, and Senior Data Analysts in the UK tend to have the highest salary potential, indicating the high value placed on advanced data skills and experience in the industry. 

### Highest Paid & Most Demanded Skills for Data Scientists In UK

### Visualize Data

```python 
# Plotting
fig, ax = plt.subplots(2, 1)

sns.set_theme(style="ticks")


sns.barplot(data=df_DS_top_pay, x='median', y=df_DS_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r')
ax[0].legend().remove()
ax[0].set_title('Top 10 Highest Paid Skills for Data Scientists')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))


sns.barplot(data=df_DS_skills, x='median', y=df_DS_skills.index, ax=ax[1], hue='median', palette='light:b')
ax[1].legend().remove()
ax[1].set_title('Top 10 Most In-Demand Skills for Data Scientists')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

plt.tight_layout()
plt.show()
```
![The Highest Paid & Most In-Demand Skills for Data Scientists in the UK](3_Project\images\highest_paid_and_in_demand_skills.png)

#### Insights

- The top graph shows specialised skills like `Scala`, `Word`, and `Redshift` are associated with higher salaries, indicating that advanced technical proficiency can increase earning potential.

- The bottom graph highlights that foundational skills like `Gcp`, `Looker`, and `r` are the most in-demand, even though they may not offer the highest salaries.


## 4. What is the most optimal skill to learn for Data Scientists?

#### Visualize Data

```python
# Plotting results

from adjustText import adjust_text


df_DS_skills_high_demand.plot(kind='scatter', x='skill_percent', y='median_salary')

texts = []
for i, txt in enumerate(df_DS_skills_high_demand.index):
    texts.append(plt.text(df_DS_skills_high_demand['skill_percent'].iloc[i], df_DS_skills_high_demand['median_salary'].iloc[i], txt))

adjust_text(texts, arrowprops=dict(arrowstyle="->", color='gray', lw=1))

sns.despine()
sns.set_theme(style='ticks')

plt.xlabel('Percent of Data Scientist Jobs')
plt.ylabel('Median Yearly Salary($USD)')
plt.title('Most Optimal Skills for Data Scientists in the UK')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.tight_layout
plt.show()
```
![Most Optimal Skills for Data Scientists in the UK](3_Project\images\optimal_skills_data_scientists_UK.png)

#### Insights
- The scatter plot shows that `SQL` and `Python`skills tend to cluster at majority of job postings, indicating their significance in Data Science jobs.

- However, other skills like `Power bi` and `gcp`, even though they are associated with higher salary tend to cluster at lower percentage of job postings.