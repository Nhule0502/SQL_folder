# Introduction
📊 Dive into the data job market! Focusing on data analyst roles, this project explores 💰 top-paying jobs, 🔥 in-demand skills, and 📈 where high demand meets high salary in data analytics.

🔍 SQL queries? Check them out here: [SQL_project folder](/SQL_project/)

# Background
Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlining others work to find optimal jobs.

The questions I wanted to answer through my SQL queries were:
What are the top-paying data analyst jobs?
What skills are required for these top-paying jobs?
What skills are most in demand for data analysts?
Which skills are associated with higher salaries?
What are the most optimal skills to learn?

# Tool I used
For my deep dive into the data analyst job market, I harnessed the power of several key tools:

SQL: The backbone of my analysis, allowing me to query the database and unearth critical insights.
PostgreSQL: The chosen database management system, ideal for handling the job posting data.
Visual Studio Code: My go-to for database management and executing SQL queries.
Git & GitHub: Essential for version control and sharing my SQL scripts and analysis, ensuring collaboration and project tracking.

# The analysis
Each query for this project aimed at investigating specific aspects of the data analyst job market. Here’s how I approached each question:

1. Top Paying Data Analyst Jobs
To identify the highest-paying roles, I filtered data analyst positions by average yearly salary and location, focusing on remote jobs. This query highlights the high paying opportunities in the field.

```sql
select  
    job_id,
    job_title,
    job_location, 
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name as company_name
from 
    job_postings_fact
LEFT JOIN company_dim on company_dim.company_id = job_postings_fact.company_id    
where 
    job_title_short = 'Data Analyst' and 
    job_work_from_home = TRUE AND
    salary_year_avg is not NULL
ORDER BY 
    salary_year_avg desc
limit 10
```
Here's the breakdown of the top data analyst jobs in 2023:

Wide Salary Range: Top 10 paying data analyst roles span from $184,000 to $650,000, indicating significant salary potential in the field.
Diverse Employers: Companies like SmartAsset, Meta, and AT&T are among those offering high salaries, showing a broad interest across different industries.
Job Title Variety: There's a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within data analytics.

2. Skills for Top Paying Jobs
To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.

```sql
with top_paying_jobs as (
    select  
        job_id,
        job_title,
        job_location, 
        job_schedule_type,
        salary_year_avg,
        job_posted_date,
        job_title_short,
        name as company_name
    from 
        job_postings_fact
    LEFT JOIN company_dim on company_dim.company_id = job_postings_fact.company_id   
    where 
        job_title_short = 'Data Analyst' and 
        job_work_from_home = TRUE AND
        salary_year_avg is not NULL
    ORDER BY 
        salary_year_avg desc
    limit 10
)
select 
    skills,
    top_paying_jobs.*
FROM top_paying_jobs
Inner join skills_job_dim on skills_job_dim.job_id = top_paying_jobs.job_id
Inner join skills_dim on skills_dim.skill_id = skills_job_dim.skill_id
order by salary_year_avg desc
limit 10
```
Here's the breakdown of the most demanded skills for the top 10 highest paying data analyst jobs in 2023:

SQL is leading with a bold count of 8.
Python follows closely with a bold count of 7.
Tableau is also highly sought after, with a bold count of 6. Other skills like R, Snowflake, Pandas, and Excel show varying degrees of demand.

3. In-Demand Skills for Data Analysts
This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand

Here's the breakdown of the most demanded skills for data analysts in 2023
```sql
SELECT
    skills,
    count (skills_job_dim.job_id) as demand_count
FROM job_postings_fact
Inner join skills_job_dim on skills_job_dim.job_id = job_postings_fact.job_id
INNER JOIN skills_dim on skills_dim.skill_id = skills_job_dim.skill_id
WHERE job_title_short = 'Data Analyst'
GROUP BY skills
ORDER BY demand_count DESC
Limit 5
```

SQL and Excel remain fundamental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.
Programming and Visualization Tools like Python, Tableau, and Power BI are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.

5. Most Optimal Skills to Learn
Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.

```sql
WITH skill_demand as (
    SELECT
        skills,
        skills_dim.skill_id,
        count (skills_job_dim.job_id) as demand_count
    FROM job_postings_fact
    Inner join skills_job_dim on skills_job_dim.job_id = job_postings_fact.job_id
    INNER JOIN skills_dim on skills_dim.skill_id = skills_job_dim.skill_id
    WHERE 
        job_title_short = 'Data Analyst' AND
        salary_year_avg is not NULL 
    GROUP BY skills_dim.skill_id
),
    avg_salary as (
    select
        skills,
        skills_dim.skill_id,
        round (avg(salary_year_avg),0) as avg_salary_per_skill
    from job_postings_fact
    Inner join skills_job_dim on skills_job_dim.job_id = job_postings_fact.job_id
    INNER JOIN skills_dim on skills_dim.skill_id = skills_job_dim.skill_id
    WHERE 
        job_title_short = 'Data Analyst' and
        salary_year_avg is not NULL
    GROUP by 
        skills_dim.skill_id
)
select
     skill_demand.skill_id,
     skill_demand.skills,
     demand_count,
     avg_salary_per_skill
from skill_demand
inner join avg_salary on avg_salary.skill_id = skill_demand.skill_id
ORDER BY 
    demand_count DESC,
    avg_salary_per_skill DESC
limit 10
```

# What I learned
Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:

🧩 Complex Query Crafting: Mastered the art of advanced SQL, merging tables like a pro and wielding WITH clauses for ninja-level temp table maneuvers.
📊 Data Aggregation: Got cozy with GROUP BY and turned aggregate functions like COUNT() and AVG() into my data-summarizing sidekicks.
💡 Analytical Wizardry: Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.


# Conclusions
nsights
From the analysis, several general insights emerged:

Top-Paying Data Analyst Jobs: The highest-paying jobs for data analysts that allow remote work offer a wide range of salaries, the highest at $650,000!

Skills for Top-Paying Jobs: High-paying data analyst jobs require advanced proficiency in SQL, suggesting it’s a critical skill for earning a top salary.

Most In-Demand Skills: SQL is also the most demanded skill in the data analyst job market, thus making it essential for job seekers.

Skills with Higher Salaries: Specialized skills, such as SVN and Solidity, are associated with the highest average salaries, indicating a premium on niche expertise.

Optimal Skills for Job Market Value: SQL leads in demand and offers for a high average salary, positioning it as one of the most optimal skills for data analysts to learn to maximize their market value.

Closing Thoughts
This project enhanced my SQL skills and provided valuable insights into the data analyst job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills. This exploration highlights the importance of continuous learning and adaptation to emerging trends in the field of data analytics.
