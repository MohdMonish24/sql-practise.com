# sql-practise.com

## www.sql-practise.com

This repository is dedicated to students and professionals who would like to brush up and advance their SQL skills for their careers. I found (https://www.sql-practice.com) educational and the web site user friendly however there wasn’t the complete solution book shared on the web site neither on GitHub. So now you can get the answers with alternative solutions for each question 

I have used PostgreSQL as main Database Management Systems (DBMS) and therefore you may encounter some queries that work in PostgreSQL may not work in
other DBMS such as SQL Server or MySQL as some implementations slight vary in syntax. For such cases, for example, in date and time operations 
I included standart SQL queries as well.

I populated mockdata using www.mockaroo.com to effectively practise the queries and get some output while working locally. Note that it is not the same large dataset used on the web site, so I suggest you use the web site itself as long as you have internet connection, which is also good for tracking your progress. You may use the sql database files given in the repository for convenience and a rapid start in creating schemas and inserting datasets.

The hospital test contains 50 questions and consists of 3 category:

- Easy
- Medium
- Hard

You can use the following Entity Relationship Diagram (ERD) to figure out what data to store, the entities, their attributes and also how entities 
relate to other entities.

![erd](https://user-images.githubusercontent.com/19313466/211203253-23f70de3-4786-45aa-8d7f-54fb81525415.png) 


## Answers

> Want to help this repository? Feel free to contribute by submitting a custom solution to be added to the questions.

---

### Section1: Easy

---

Questions 1- 17

1. Show first name, last name, and gender of patients who's gender is 'M'

```sql
SELECT first_name,
       last_name,
       gender
FROM patients
WHERE gender = 'M';
```
2. Show first name and last name of patients who does not have allergies. (null)

 ```sql
SELECT
   first_name, last_name
FROM patients
WHERE allergies IS NULL
ORDER BY first_name ASC;
```

3. Show first name of patients that start with the letter 'C'

```sql
SELECT
  first_name
FROM  patients
WHERE first_name LIKE 'C%';
```

4. Show first name and last name of patients that weight within the range of 100 to 120 (inclusive)

```sql
SELECT
  first_name,
  last_name
FROM patients
WHERE weight BETWEEN 100 AND 120;;
```

5. Update the patients table for the allergies column. If the patient's allergies is null then replace it with 'NKA'

```sql
UPDATE patients
SET allergies = 'NKA'
WHERE allergies IS NULL;

```

6. Show first name and last name concatenated into one column to show their full name.

```sql
SELECT
  CONCAT(first_name,' ',last_name) AS full_name
FROM patients; 

```

7. Show first name, last name, and the full province name of each patient.

```sql
SELECT first_name,last_name,province_name 
FROM patients as p 
JOIN province_names AS pn  
ON p.province_id = pn.province_id;

```

8. Show how many patients have a birth_date with 2010 as the birth year.

```sql
SELECT COUNT(province_id) AS total_patients
FROM patients 
WHERE YEAR(birth_date) = 2010;

-- alternative #1
SELECT COUNT(patients.birth_date)
FROM patients
WHERE birth_date BETWEEN '2010-01-01' AND '2010-12-30';

-- alternative #2
SELECT COUNT(patients.birth_date)
FROM patients
WHERE birth_date LIKE '2010%';

```

9. Show the first_name, last_name, and height of the patient with the greatest height.

```sql
SELECT first_name,last_name
MAX (height) as hight
FROM patients;

-- alternative #1
SELECT 
  First_name, last_name, height
FROM patients
WHERE height =(
    SELECT MAX (height)
    FROM patients )

-- alternative #2
SELECT first_name, last_name, MAX(height) AS height
FROM patients
GROUP BY first_name, last_name
ORDER BY height DESC
LIMIT 1;
```

10. Show all columns for patients who have one of the following patient_ids:
    1,45,534,879,1000

```sql
-- alternative #1
SELECT *
FROM patients
WHERE patient_id IN (1, 45, 534, 879, 1000);


-- alternative #2
SELECT *
FROM patients
WHERE patient_id = 1
   OR patient_id = 45
   OR patient_id = 534
   OR patient_id = 879
   OR patient_id = 1000;
```

11. Show the total number of admissions

```sql
SELECT COUNT(admissions.patient_id) AS total_number_of_admissions
FROM admissions;
```

12. Show all the columns from admissions where the patient was admitted and discharged on the same day.

```sql
SELECT *
FROM admissions
WHERE admission_date = discharge_date;

```

13. Show the total number of admissions for patient_id 579.

```sql
SELECT patient_id,
COUNT(*) as total_admission 
FROM admissions
WHERE patient_id = 579
GROUP BY patient_id;
```

14. Based on the cities that our patients live in, show unique cities that are in province_id 'NS'?

```sql
SELECT DISTINCT(city) 
FROM patients 
WHERE province_id = 'NS';

-- alternative #1
SELECT DISTINCT(city) AS unique_cities
FROM patients
         JOIN province_names ON patients.province_id = province_names.province_id
WHERE province_names.province_id = 'NS';
```

15. Write a query to find the first_name, last name and birth date of patients who have height more than 160 and weight more than 70

```sql
SELECT first_name, last_name, birth_date
FROM patients
WHERE height > 160
  AND weight > 70;

```

16. Write a query to find list of patients first_name, last_name, and allergies from Hamilton where allergies are not null

```sql
SELECT first_name,last_name,allergies 
FROM patients
WHERE city = 'Hamilton' AND allergies IS NOT NULL;
```

17. Based on cities where our patient lives in, write a query to display the list of unique city starting with a vowel (a, e, i, o, u). Show the
    result order in ascending by city.

```sql
SELECT city
FROM patients
WHERE city SIMILAR TO '[aeiouAEIOU]%'
ORDER BY city ASC;
```

SIMILAR TO and LIKE are not the same in PostgreSQL.

SIMILAR TO is used to match a string against a pattern that can include regular expressions. For example, you can use SIMILAR TO to find all strings
that start with a vowel by using the pattern '[AEIOU]%'.

LIKE is used to match a string against a pattern that does not include regular expressions. It is similar to the = operator, except that it allows you
to use the % and _ wildcard characters to match any number of or a single character, respectively. For example, you can use LIKE to find all strings
that start with a vowel by using the pattern 'A%'.

```sql
SELECT DISTINCT(city) 
FROM patients
WHERE city LIKE 'A%'
   OR city LIKE 'E%'
   OR city LIKE 'I%'
   OR city LIKE 'O%'
   OR city LIKE 'U%'
   OR city LIKE 'a%'
   OR city LIKE 'e%'
   OR city LIKE 'i%'
   OR city LIKE 'o%'
   OR city LIKE 'u%'
ORDER BY city ASC;

```

---
### Section2: Medium

---

Questions 1- 23
1. Show unique birth years from patients and order them by ascending.

```
-- SQL
SELECT DISTINCT(YEAR(birth_date)) AS unique_years
FROM patients
ORDER BY YEAR(birth_date) ASC;

```

2. Show unique first names from the patients table which only occurs once in the list.
   For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list.
   If only 1 person is named 'Leo' then include them in the output.

```sql
SELECT first_name
FROM patients
GROUP BY first_name
HAVING COUNT(first_name) = 1;

```

    Tip: HAVING clause was added to SQL because the WHERE keyword cannot be used with aggregate functions.

3. Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.

```sql
SELECT patient_id, first_name
FROM patients
WHERE first_name SIMILAR TO 's____%s';

```
-- alternative #1
```sql
SELECT patient_id, first_name
FROM patients
WHERE first_name LIKE 's____%s';
```

4. Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.
   Primary diagnosis is stored in the admissions table.

```sql
SELECT p.patient_id, p.first_name, p.last_name
FROM patients p
         JOIN admissions a
ON p.patient_id = a.patient_id
WHERE a.diagnosis = 'Dementia';

```

5. Display every patient's first_name.
   Order the list by the length of each name and then by alphbetically.

```sql
SELECT first_name
FROM patients
ORDER BY LENGTH(first_name), first_name;
```

6. Show the total amount of male patients and the total amount of female patients in the patients table.
   Display the two results in the same row.

```sql
-- Postgresql
SELECT (SELECT COUNT(*) FROM patients WHERE gender = 'M') AS male_count,
       (SELECT COUNT(*) FROM patients WHERE gender = 'F') AS female_count;

-- alternative #1
-- SQL
SELECT SUM(gender = 'M') AS male_count,
       SUM(gender = 'F') AS female_count
FROM patients;
```

7. Show the total amount of male patients and the total amount of female patients in the patients table.
   Display the two results in the same row.

```sql
SELECT first_name, last_name, allergies
FROM patients
WHERE allergies = 'Penicillin'
   OR allergies = 'Morphine'
ORDER BY allergies ASC, first_name ASC, last_name ASC;

-- alternatively 

SELECT first_name,
       last_name,
       allergies
FROM patients
WHERE allergies IN ('Penicillin', 'Morphine')
ORDER BY allergies,
         first_name,
         last_name;
```

8. Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.

```sql
SELECT patient_id, diagnosis
FROM admissions
GROUP BY patient_id, diagnosis
HAVING COUNT(diagnosis = diagnosis) > 1;
```

9. Show the city and the total number of patients in the city.
   Order from most to least patients and then by city name ascending.

```sql
SELECT city, COUNT(*) AS number_of_patients
FROM patients
GROUP BY city
ORDER BY number_of_patients DESC, city ASC;
```

10. Show first name, last name and role of every person that is either patient or doctor.
    The roles are either "Patient" or "Doctor"

```sql
SELECT first_name, last_name, 
'Patient' AS 'role'
 FROM patients
UNION ALL
select first_name, last_name, 'Doctor' 
FROM doctors;
```



11. Show all allergies ordered by popularity. Remove NULL values from query.

```sql
SELECT allergies, 
COUNT(*) AS total_diagnosis
FROM patients
GROUP BY allergies
HAVING allergies IS NOT NULL;
ORDER BY total_diagnosis DESC;
```


12. Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade.
    Sort the list starting from the earliest birth_date.

```sql
--PostgreSQL version
SELECT first_name, last_name, birth_date
FROM patients
WHERE EXTRACT(YEAR FROM birth_date) BETWEEN 1970 AND 1979;

--SQL
SELECT first_name,
       last_name,
       birth_date
FROM patients
WHERE YEAR(birth_date) BETWEEN 1970 AND 1979
ORDER BY birth_date ASC;

-- SQL Regex
SELECT first_name,
       last_name,
       birth_date
FROM patients
WHERE year(birth_date) LIKE '197%'
ORDER BY birth_date ASC
```

13. We want to display each patient's full name in a single column.
    Their last_name in all upper letters must appear first, then first_name in all lower case letters.
    Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
    EX: SMITH,jane

```sql
SELECT CONCAT(UPPER(last_name), ',', LOWER(first_name)) AS new_name_format
FROM patients
ORDER BY first_name DESC;

```

14. Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000.

```sql
SELECT pr.province_id,
SUM(pa.height) AS sum_height
FROM province_names pr
         JOIN patients pa ON pr.province_id = pa.province_id
GROUP BY pr.province_id
HAVING SUM(pa.height) >= 7000;
```
-- alternatively 
```sql
SELECT province_id, 
SUM(height) AS total_height
FROM patients
GROUP BY province_id
HAVING total_height > 7000;
```

15. Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'

```sql
SELECT (MAX(weight) - MIN(weight)) AS weight_delta
FROM patients
WHERE last_name = 'Maroni';
```


16. Show all of the days of the month (1-31) and how many admission_dates occurred on that day.
    Sort by the day with most admissions to least admissions.

```sql
-- PostgreSQL
SELECT EXTRACT(DAY FROM admission_date) AS day_number, COUNT(patient_id) AS number_of_admissions
FROM admissions
GROUP BY day_number
ORDER BY number_of_admissions DESC;

-- SQL
SELECT DAY(admission_date) AS date, 
COUNT(admission_date) AS admissions
FROM admissions
GROUP BY date
ORDER BYadmissions DESC;
```


17. Show all columns for patient_id 542's most recent admission_date.

```sql
-- PostgreSQL
SELECT *
FROM admissions
WHERE patient_id = 542
GROUP BY patient_id, admission_date, discharge_date, diagnosis, attending_doctor_id
HAVING admission_date = MAX(admission_date);

-- alternatively 
-- sql
SELECT *
FROM admissions
WHERE patient_id = 542
ORDER BY admission_date DESC
LIMIT 1;

-- alternatively 
SELECT *
FROM admissions
WHERE patient_id = 542
GROUP BY patient_id
HAVING admission_date = MAX(admission_date);
```

18. Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria:
    - patient_id is an odd number and attending_doctor_id is either 1, 5, or 19.
    - attending_doctor_id contains a 2 and the length of patient_id is 3 characters.

```sql
-- PostgreSQL (we would need to explicity cast the type attending_doctor_id is integer whereas LIKE method performs operations on Strings
SELECT patient_id, attending_doctor_id, diagnosis
FROM admissions
WHERE patient_id % 2 = 1 AND attending_doctor_id IN (1, 5, 19)
   OR CONCAT(attending_doctor_id) LIKE '%2%' AND LENGTH(CONCAT(patient_id)) = 3;

-- SQL
SELECT patient_id, attending_doctor_id, diagnosis
FROM admissions
WHERE 
(patient_id%2 != 0 AND attending_doctor_id IN (1, 5, 19))
OR
(LEN(patient_id) = 3 AND attending_doctor_id LIKE  '%2%');
```



19. Show first_name, last_name, and the total number of admissions attended for each doctor.
    Every admission has been attended by a doctor.

```sql
SELECT 
d.first_name, d.last_name,
COUNT(a.attending_doctor_id) AS addmissions_total
FROM doctors d
JOIN admissions a
ON d.doctor_id = a.attending_doctor_id
GROUP BY d.doctor_id;
```



20. For each doctor, display their id, full name, and the first and last admission date they attended.

```sql
SELECT 
d.doctor_id,
   CANCAT(d.first_name, ' ', d.last_name) AS full_name,
   MIN(a.admission_date) AS first_addmission_date,
   MAX(a.admission_date) AS last_addmission_date
FROM doctors d
JOIN admissions a
ON  d.doctor_id = a.attending_doctor_id
GROUP BY d.doctor_id;
```



21. Display the total amount of patients for each province. Order by descending.

```sql
SELECT p.province_name, 
COUNT(*) AS patient_count
FROM province_names p
JOIN patients pa
ON p.province_id = pa.province_id
GROUP BY p.province_name
ORDER BY  patient_count DESC;
```


22. For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.

```sql
SELECT
CONCAT(p.first_name, " ", p.last_name) AS patient_full_name, a.diagnosis, 
CONCAT(d.first_name, " ", d.last_name) AS doc_full_name 
FROM patients as p  
JOIN admissions AS a         
ON p.patient_id = a.patient_id      
JOIN doctors AS d         
ON d.doctor_id = a.attending_doctor_id;

```

23. Display the number of duplicate patients based on their first_name and last_name.

```sql

SELECT first_name, last_name, 
COUNT(*) AS num_of_duplicates 
FROM patients 
GROUP BY first_name, last_name 
HAVIN COUNT(*) > 1;

/*
In the HAVING clause, you can only use the names of columns that are included in the GROUP BY clause 
or that are aggregated by an aggregate function (such as COUNT, MAX, MIN, etc.).

In this case, the COUNT(*) function is used to count the number of rows in each group, 
and the result of this function is given an alias of "number_of_duplicates".
However, the HAVING clause does not refer to this alias. Instead, it refers to the function itself (i.e. COUNT(*)).
*/

```
---
### Section3: Hard

---
