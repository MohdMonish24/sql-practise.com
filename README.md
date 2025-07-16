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
