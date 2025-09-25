# 📊 Data Analyst Job Market Exploration

Dive into the data job market! This project focuses on **data analyst roles**, exploring:

- 💰 Top-paying jobs  
- 🔥 In-demand skills  
- 📈 Where high demand meets high salary in data analytics  

🔍 **SQL queries?** Check them out in the `project_sql` folder.

---

## 🧠 Background

Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to pinpoint **top-paid and in-demand skills**, streamlining others' work to find optimal jobs.

**Data Source:** From my SQL course, packed with insights on job titles, salaries, locations, and essential skills.

### ❓ Key Questions

- What are the top-paying data analyst jobs?  
- What skills are required for these top-paying jobs?  
- What skills are most in demand for data analysts?  
- Which skills are associated with higher salaries?  
- What are the most optimal skills to learn?

---

## 🛠️ Tools I Used

- **SQL**: The backbone of my analysis  
- **PostgreSQL**: For managing job posting data  
- **Visual Studio Code**: For database management and SQL execution  
- **Git & GitHub**: For version control and sharing my work  

---

## 📈 The Analysis

### 1. 💸 Top Paying Data Analyst Jobs

```sql
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
nsights:

Wide Salary Range: $184,000 to $650,000

Diverse Employers: SmartAsset, Meta, AT&T

Job Title Variety: From Data Analyst to Director of Analytics

##  2. 🧠 Skills for Top Paying Jobs
``` sql
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
Top Skills:

SQL: 8 mentions

Python: 7 mentions

Tableau: 6 mentions

Others: R, Snowflake, Pandas, Excel

## 3. 📊 In-Demand Skills for Data Analysts
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
### 📊 In-Demand Skills for Data Analysts (2023)

| Skill     | Demand Count |
|-----------|--------------|
| SQL       | 7,291        |
| Excel     | 4,611        |
| Python    | 4,330        |
| Tableau   | 3,745        |
| Power BI  | 2,609        |

## 5. 🧭 Most Optimal Skills to Learn
```sql
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
### 🧠 Most Optimal Skills to Learn (2023)

| Skill ID | Skill       | Demand Count | Avg Salary ($) |
|----------|-------------|--------------|----------------|
| 8        | Go          | 27           | 115,320        |
| 234      | Confluence  | 11           | 114,210        |
| 97       | Hadoop      | 22           | 113,193        |
| 80       | Snowflake   | 37           | 112,948        |
| 74       | Azure       | 34           | 111,225        |
| 77       | BigQuery    | 13           | 109,654        |
| 76       | AWS         | 32           | 108,317        |
| 4        | Java        | 17           | 106,906        |
| 194      | SSIS        | 12           | 106,683        |
| 233      | Jira        | 20           | 104,918        |

## 🎯 Summary of Optimal Skills
Programming Languages: Python, R — high demand, moderate salary

Cloud Tools: Snowflake, Azure, AWS — strong demand and salary

BI Tools: Tableau, Looker — essential for data storytelling

Databases: Oracle, SQL Server, NoSQL — foundational for data management

## 🚀 What I Learned

Throughout this adventure, I've turbocharged my SQL toolkit with some serious firepower:

- 🧩 **Complex Query Crafting**: Mastered the art of advanced SQL, merging tables like a pro and wielding `WITH` clauses for ninja-level temp table maneuvers.
- 📊 **Data Aggregation**: Got cozy with `GROUP BY` and turned aggregate functions like `COUNT()` and `AVG()` into my data-summarizing sidekicks.
- 💡 **Analytical Wizardry**: Leveled up my real-world puzzle-solving skills, turning questions into actionable, insightful SQL queries.

---

## 📌 Conclusions

### 🔍 Insights

From the analysis, several general insights emerged:

- **Top-Paying Data Analyst Jobs**: Remote roles offer a wide salary range, with the highest reaching **$650,000**!
- **Skills for Top-Paying Jobs**: Advanced proficiency in **SQL** is a key requirement for high-paying positions.
- **Most In-Demand Skills**: **SQL** tops the list, making it essential for job seekers.
- **Skills with Higher Salaries**: Specialized skills like **SVN** and **Solidity** are linked to the highest average salaries, showing the value of niche expertise.
- **Optimal Skills for Market Value**: **SQL** stands out for both demand and salary, making it one of the most strategic skills to learn.

---

## 🧠 Closing Thoughts

This project enhanced my SQL skills and provided valuable insights into the data analyst job market. The findings serve as a guide to:

- Prioritizing skill development  
- Targeting high-value job opportunities  
- Navigating the competitive landscape with confidence  

Aspiring data analysts can better position themselves by focusing on **high-demand, high-salary skills**. This exploration underscores the importance of **continuous learning** and **adapting to emerging trends** in the field of data analytics.







