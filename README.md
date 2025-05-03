# SQL_PROJECT
HR Portfolio Using SQL and Power BI

## Table of Contents

## Table of Contents

* [Introduction](#introduction)
* [Background](#background)
* [Materials and Methodology](#materials-and-methodology)
* [The Analysis](#the-analysis)
* [Key Technical Milestones](#key-technical-milestones)
* [Key Insights and Strategic Conclusion](#key-insights-and-strategic-conclusion)
* [Closing Thoughts](#closing-thoughts)
* [Recommendation](#recommendation)



# Introduction
### Data Analyst Job Market Insights
This project provides a comprehensive analysis of the data job market with a specific focus on data analyst roles. It explores key trends such as the highest-paying positions, the most in-demand technical skills, and the intersection of high demand and competitive salaries. By examining these insights, the project aims to offer valuable guidance for aspiring and current data professionals seeking to align their skill sets with market needs and maximize career opportunities.

🔍 SQL queries? Check them out here: `[project_sql folder](project_sql)`

# Background
This project was driven by the goal of navigating the data analyst job market more effectively. It originated from a desire to identify top-paying and in-demand skills, ultimately helping others streamline their job search and align their learning efforts with market demands.

The dataset used in this analysis comes from Luke Barousse’s SQL course on YouTube. It contains valuable insights into job titles, salaries, locations, and the key skills required for data analyst roles.

Through a series of SQL queries, this project aims to answer the following questions:

- What are the top-paying data analyst jobs?

- What skills are required for these top-paying roles?

- What skills are most in demand for data analysts?

- Which skills are most strongly associated with higher salaries?

- What are the most optimal skills to learn to stay competitive in the field?
  
# Materials and Methodology:
To conduct a comprehensive analysis of the data analyst job market, I utilized a range of tools designed for data extraction, analysis, and visualization:

- **SQL** – Served as the core of my analysis, enabling me to write queries to extract meaningful insights from the dataset.

- **PostgreSQL** – A robust and reliable database management system, ideal for storing and querying job posting data.

- **Visual Studio Code** – My primary development environment for writing and executing SQL queries efficiently.

- **Git & GitHub** – Used for version control and project management, allowing for seamless collaboration, tracking of changes, and sharing of my SQL scripts and findings.

- **Power BI** – Used to visualize the results of my analysis, transforming raw data into interactive dashboards and clear, insightful visuals.

# The Analysis
Each query in this project was designed to explore specific aspects of the data analyst job market. Below is a breakdown of how I approached each key question, starting with identifying the top-paying roles.

#### 1. Top Paying Data Analyst Jobs
To uncover the highest-paying opportunities for data analysts, I focused on positions that were fully ```remote (job_location = 'Anywhere')``` and included reported average annual salaries. This allowed me to zero in on competitive roles accessible regardless of location.

##### SQL Query

``` sql
SELECT	
	job_id,
	job_title,
	job_location,
	job_schedule_type,
	salary_year_avg,
	job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND 
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10;
```

Here's the breakdown of the top data analyst jobs in 2023:

##### 🔍 Key Insight: Top-Paying Roles
Wide Salary Range: The top 10 highest-paying data analyst roles offer salaries ranging from $184,000 to $650,000, underscoring the significant earning potential within the field.

Diverse Employers: High-paying roles are offered by a range of companies, including SmartAsset, Meta, and AT&T, indicating strong demand across various industries.

Job Title Variety: These roles span a wide spectrum of job titles—from Data Analyst to Director of Analytics—highlighting the diverse responsibilities and career trajectories within data analytics.

📊 Top-Paying Roles Visualization
The bar chart below visualizes the top 10 highest-paying remote data analyst positions, generated from the SQL query results using PowerBI

![image](https://github.com/user-attachments/assets/60ee2c59-bbb3-4780-990f-f2a38f48e6ae)
**Figure 4.1** _showing top paying Data Analyst Job_

#### 2. Skills for Top Paying Jobs
To identify the skills most commonly required for the highest-paying data analyst roles, I joined job postings with the associated skills dataset. This allowed me to uncover which technical proficiencies are most frequently listed by employers offering top-tier salaries.

By filtering for remote data analyst roles with above-average salaries, the analysis highlights the specific tools and technologies that contribute to higher compensation—offering valuable direction for skill development and career planning.

##### SQL QUERY
```sql
WITH top_paying_jobs AS (
    SELECT	
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND 
        job_location = 'Anywhere' AND 
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC;
```


##### 🔧 Skills Required for Top-Paying Data Analyst Jobs

To determine which skills are most valued in high-paying data analyst roles, I analyzed the job postings for the top 10 highest-paying positions and identified the frequency of each required skill.

📌 **Key Findings:**
SQL appears in 8 out of 10 top-paying roles, confirming its status as a foundational skill in data analytics.

Python is listed in 7 out of 10 roles, highlighting its importance for advanced analysis, scripting, and automation.

Tableau is mentioned in 6 roles, indicating strong demand for data visualization and dashboarding skills.

Other notable tools and languages include R, Snowflake, Pandas, and Excel, reflecting a well-rounded mix of data engineering, analysis, and reporting capabilities.

📊 Top-Paying Skills Visualization
The bar chart below illustrates the most frequently required skills for the top 10 highest-paying data analyst roles. This visualization was generated using SQL and plotted by **Power BI** based on query results.


**Figure 4.1** _showing the Top Paying Jobs_

#### 3. In-Demand Skills for Data Analysts
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```sql
SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst' 
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5;
```
Here's the breakdown of the most demanded skills for data analysts in 2023

SQL and Excel continue to be fundamental, underscoring the necessity of strong foundational skills in data processing and spreadsheet management. Additionally, proficiency in programming languages and data visualization tools such as Python, Tableau, and Power BI has become increasingly essential, reflecting the growing significance of technical expertise in data storytelling and decision-making support.

|Skills	| Demand Count|
|-----|-----|
|SQL |	7291 |
|Excel	|4611|
|Python	|4330|
|Tableau|3745|
|Power BI|2609|

 **Table 4.1:**   _Table of the demand for the top 5 skills in data analyst job postings_

#### 4. Skills Based on Salary
Analyzing the average salaries linked to various skills provided insights into which skills command the highest pay, highlighting the most financially rewarding areas within the field.

```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 25;
```
Below is the  breakdown of the results for top paying skills for Data Analysts:

High Demand for Big Data & ML Skills: Top salaries are commanded by analysts skilled in big data technologies (PySpark, Couchbase), machine learning tools (DataRobot, Jupyter), and Python libraries (Pandas, NumPy), reflecting the industry's high valuation of data processing and predictive modeling capabilities.
Software Development & Deployment Proficiency: Knowledge in development and deployment tools (GitLab, Kubernetes, Airflow) indicates a lucrative crossover between data analysis and engineering, with a premium on skills that facilitate automation and efficient data pipeline management.
Cloud Computing Expertise: Familiarity with cloud and data engineering tools (Elasticsearch, Databricks, GCP) underscores the growing importance of cloud-based analytics environments, suggesting that cloud proficiency significantly boosts earning potential in data analytics.

| Skill        | Average Salary ($) |
|--------------|--------------------|
| PySpark      | 208,172            |
| Bitbucket    | 189,155            |
| Couchbase    | 160,515            |
| Watson       | 160,515            |
| DataRobot    | 155,486            |
| GitLab       | 154,500            |
| Swift        | 153,750            |
| Jupyter      | 152,777            |
| Pandas       | 151,821            |
| Elasticsearch| 145,000            |

** Table 4.2 ** _showing the average salary for the top 10 paying skills for data analysts_

#### 5. Most Optimal Skills to Learn:
By synthesizing insights from both demand frequency and salary benchmarks, this analysis aimed to identify the most advantageous skills—those that are not only highly sought after in the job market but also associated with above-average remuneration. The findings provide a strategic basis for prioritizing skill acquisition in alignment with current industry trends and financial incentives.
``` sql
SELECT 
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True 
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25;
```
| Skill ID | Skill       | Demand Count | Average Salary ($) |
|----------|-------------|---------------|---------------------|
| 8        | Go          | 27            | 115,320             |
| 234      | Confluence  | 11            | 114,210             |
| 97       | Hadoop      | 22            | 113,193             |
| 80       | Snowflake   | 37            | 112,948             |
| 74       | Azure       | 34            | 111,225             |
| 77       | BigQuery    | 13            | 109,654             |
| 76       | AWS         | 32            | 108,317             |
| 4        | Java        | 17            | 106,906             |
| 194      | SSIS        | 12            | 106,683             |
| 233      | Jira        | 20            | 104,918             |

 ** Table 4.3 ** _Table of the most optimal skills for data analyst sorted by salary_

##### 🔍 Most Optimal Skills for Data Analysts in 2023

- High-Demand Programming Languages
Python and R emerged as the most sought-after programming languages, with demand counts of **236** and **148** respectively. While their average salaries — **$101,397** for Python and **$100,499** for R — suggest widespread availability, their prominence underscores their foundational role in data analysis and statistical computing.

- Cloud Tools and Technologies
Proficiency in cloud platforms and big data technologies such as **Snowflake**, **Azure**, **AWS**, and **BigQuery** is associated with both substantial demand and competitive compensation. This trend highlights the growing reliance on scalable, cloud-based infrastructures for advanced analytics and data management.

-  Business Intelligence and Visualization Tools
Tools like **Tableau** and **Looker** remain highly relevant, with demand counts of **230** and **49** respectively, and average salaries of **$99,288** and **$103,795**. These tools play a crucial role in transforming complex datasets into actionable insights, emphasizing the importance of data storytelling and strategic visualization.

-  Database Technologies
Skills in traditional relational databases (e.g., **Oracle**, **SQL Server**) and NoSQL systems are still in high demand. With average salaries ranging from **$97,786** to **$104,534**, this domain remains a fundamental aspect of the data infrastructure landscape.

---

# Key Technical Milestones

Throughout the duration of this project, I substantially advanced my SQL proficiency, developing a robust and adaptable analytical toolkit. This hands-on engagement not only reinforced my technical competencies but also provided valuable insights into emerging trends within the data analytics landscape—particularly in identifying high-impact, high-value skills essential for sustained professional growth.

- 🧩 **Complex Query Crafting**: Mastered the art of advanced SQL, confidently merging tables and utilizing `WITH` clauses for efficient temporary table creation and cleaner, modular queries.

- 📊 **Data Aggregation**: Developed proficiency with `GROUP BY` and aggregate functions like `COUNT()`, `AVG()`, and `SUM()` to transform raw data into meaningful summaries.

- 💡 **Analytical Problem-Solving**: Sharpened real-world analytical thinking by translating complex questions into well-structured, actionable SQL queries that uncover valuable insights.
---

# Key Insights and strategic Conclusion

The analysis yielded several key insights that highlight the dynamics of the current data analyst job market:

- 💼 **Top-Paying Roles**: Remote data analyst positions can command exceptionally high salaries, with the highest recorded offer reaching an impressive **$650,000**, demonstrating the lucrative potential within the field.

- 🧠 **Critical Skills for High-Paying Jobs**: Advanced proficiency in **SQL** consistently emerged as a prerequisite for top-tier roles, reinforcing its status as a foundational and high-leverage skill.

- 📈 **Most In-Demand Skill**: **SQL** also ranked as the most in-demand skill across job postings, confirming its essential role for anyone aspiring to enter or advance within the data analytics profession.

- 💎 **Skills with Premium Salaries**: Specialized and niche competencies, such as **SVN** and **Solidity**, are correlated with significantly higher average salaries, suggesting a market premium for rare and advanced technical expertise.

- 🚀 **Strategic Skills for Market Value**: With its unmatched combination of high demand and competitive average salary, **SQL** stands out as one of the most strategic skills for data analysts seeking to maximize their employability and market value.

These insights not only reflect current industry trends but also offer a clear roadmap for aspiring data analysts to align their skill development with high-impact, high-reward opportunities.

# Closing Thoughts

This project significantly strengthened my SQL capabilities while offering meaningful insights into the evolving landscape of the data analyst job market. The results of the analysis provide a strategic foundation for prioritizing both skill development and targeted job search efforts.

For aspiring data analysts, aligning with high-demand and high-salary skill sets is key to gaining a competitive edge in the field. This exploration underscores the critical importance of continuous learning and adaptability in response to emerging technologies and market trends within the domain of data analytics.

----

# Recommendation

Based on the insights derived from this analysis, it is recommended that aspiring and early-career data analysts prioritize the development of core technical competencies—particularly in SQL, which consistently emerged as both the most in-demand and one of the most highly compensated skills. Additionally, gaining proficiency in cloud-based tools (such as Snowflake, AWS, and Azure), data visualization platforms (like Tableau and Looker), and niche technologies (such as BigQuery or Solidity) can further enhance employability and earning potential. Continuous upskilling and staying attuned to industry trends will be essential for maintaining a competitive edge in the dynamic field of data analytics.
