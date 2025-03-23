# Career-Track-Analysis-with-SQL-and-Tableau

## Project Overview
SQL project showcasing a career track analysis of students on the 365 Data Science Platform. The primary objective of this project is to properly utilize SQL to create a subquery that groups completion percentages of various career tracks on the platform into completion buckets. This info will then be transfered to Tableau, where visualizations will be created to better showcase this data.

## Exploratory Data Analysis
1. What is the number of enrolled students monthly? Which is the month with the most enrollments? Speculate about the reason for the increased numbers.
2. Which career track do students enroll most in?
3. What is the career track completion rate? Can you say if it’s increasing, decreasing, or staying constant with time?
4. How long does it typically take students to complete a career track? What type of subscription is most suitable for students who aim to complete a career track: monthly, quarterly, or annual?
5. What advice and suggestions for improvement would you give the 365 team to boost engagement, increase the track completion rate, and motivate students to learn more consistently?

## Data Analysis
 ~~~ sql
   USE sql_and_tableau;

SELECT 
    *
FROM
    career_track_info;
    
SELECT
 *
FROM
 career_track_student_enrollments;

-- Building a subquery by selecting a data set containing certain columns

SELECT
 ROW_NUMBER() OVER (ORDER BY e.student_id, e.track_id) AS student_track_id,
    e.student_id,
    i.track_name,
    e.date_enrolled,
    CASE
  WHEN date_completed IS NULL THEN 0
        ELSE 1
 END AS track_completed,
    DATEDIFF(e.date_completed, e.date_enrolled) AS days_for_completion,
    CASE
  WHEN e.date_completed IS NULL THEN NULL
        WHEN DATEDIFF(e.date_completed, e.date_enrolled) = 0 THEN 'Same Day'
        WHEN DATEDIFF(e.date_completed, e.date_enrolled) BETWEEN 1 AND 7 THEN '1 to 7 days'
        WHEN DATEDIFF(e.date_completed, e.date_enrolled) BETWEEN 8 AND 30 THEN '8 to 30 days'
        WHEN DATEDIFF(e.date_completed, e.date_enrolled) BETWEEN 31 AND 60 THEN '31 to 60 days'
        WHEN DATEDIFF(e.date_completed, e.date_enrolled) BETWEEN 61 AND 90 THEN '61 to 90 days'
        WHEN DATEDIFF(e.date_completed, e.date_enrolled) BETWEEN 91 AND 365 THEN '91 to 365 days'
        WHEN DATEDIFF(e.date_completed, e.date_enrolled) > 365 THEN '366+ days'
 END AS completion_bucket
FROM
 career_track_student_enrollments e
  JOIN
 career_track_info i ON e.track_id = i.track_id;
~~~

## Interpretation
1. The combination chart reveals fluctuations in monthly student enrollments throughout the year. For most months, the average enrollment figure falls within a range of 800 to 1,200 students. However, notable exceptions include October, with an approximate low of 400 enrollments, and August, which peaks at approximately 1,650 enrollments, highlighting significant deviations from the typical trend. The sharp rise in August enrollments, reaching approximately 1,650 students, likely reflects the start of the academic year, when students and professionals seize the opportunity to gain skills and certifications in tools like SQL and Tableau for career or academic advancement.
2. The graphs above illustrate the three career tracks available for student enrollment. The data reveals that the Data Analyst career track (depicted second) consistently records the highest average monthly enrollments.
3. The career track completion rate exhibits considerable variability throughout the year. At its lowest point, the rate drops to approximately 0.5% in September, reflecting a substantial decline from its peak of 2.3% in August, the preceding month. These figures represent the most significant outliers; however, consistent fluctuations are observed across the entire year.
4. Referencing our completion bucket bar graph, the data indicates that the most prevalent timeframe for career track completion falls within the range of 91 to 365 days, representing the broadest category. This analysis suggests that a yearly subscription model is the most appropriate option to accommodate student completion rates effectively.
5. Our data indicates that August is the most common month for students to initiate a career track, a trend that aligns closely with the start of the academic year for many institutions. However, there is a notable decline in completion rates in September, the subsequent month. To address this, I propose implementing a multi-month promotional campaign specifically designed to enhance retention among students who enroll in August. This strategic initiative could significantly improve both student retention and overall completion percentages.

## Data Source
[Project File] (https://learn.365datascience.com/projects/career-track-analysis-with-sql-and-tableau/)
