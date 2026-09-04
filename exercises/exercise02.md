# Exercise 02: World Database – Joins, Grouping, and Data Quality

- Name: Anas
- Course: Database for Analytics
- Module: 2
- Database Used: World Database (PostgreSQL)

---

## Instructions

- Answer each question below using SQL executed against the **World database**.
- All SQL commands **must be run by you**.
- For each SQL-based question:
  - Include the SQL command in a fenced code block
  - Include a **screenshot** showing the command and its results
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

When importing records from `worldPGSQL.sql`, **how many cities were imported**?

### Answer

 4079 cities were imported 

### Screenshot

_Show evidence of how you determined this (for example, a COUNT query)._

```sql
SELECT COUNT(*) FROM city;
```

![Q1 Screenshot] <img width="983" height="573" alt="image" src="https://github.com/user-attachments/assets/1a946589-f2b6-4886-bc54-7342b4b68d95" />


---

## Question 2

Using the World database, write the SQL command to
**display each country name**
along with the **name of each language spoken in that country**.

### SQL

```sql
 SELECT country.name, countrylanguage.language
FROM country
JOIN countrylanguage
    ON country.code = countrylanguage.countrycode;
```

### Screenshot

![Q2 Screenshot] <img width="776" height="646" alt="image" src="https://github.com/user-attachments/assets/1c774da2-5f8c-4df9-9c42-4fd2eb2349c4" />


---

## Question 3

Using the World database, write the SQL command
to **display each country name** along with the name
of each **official language spoken in that country**.

### SQL

```sql
 SELECT country.name, countrylanguage.language
FROM country
JOIN countrylanguage
    ON country.code = countrylanguage.countrycode
WHERE countrylanguage.isofficial = 'T';
```

### Screenshot

![Q3 Screenshot] <img width="776" height="643" alt="image" src="https://github.com/user-attachments/assets/69869136-ab53-43b6-af63-8e006328c524" />


---

## Question 4

Consider the following two SQL statements:

```sql
SELECT *
FROM country, countrylanguage
WHERE country.code = countrylanguage.countrycode;
```

```sql
SELECT *
FROM country
LEFT OUTER JOIN countrylanguage
ON country.code = countrylanguage.countrycode;
```

**In your own words**, describe what data the
**second query returns that the first query does not**.

### Answer

The first query performs an implicit inner join (using a comma join with a ⁠WHERE⁠ clause), which only returns rows where there is a matching country code in both the ⁠country⁠ and ⁠countrylanguage⁠ tables.
The second query uses a ⁠LEFT OUTER JOIN⁠, which returns all records from the left table (⁠country⁠), regardless of whether they have a match in the right table (⁠countrylanguage⁠).
Therefore, the data that the second query returns that the first query does not includes countries that do not have any associated languages recorded in the ⁠countrylanguage⁠ table. For those countries, the second query will still display the country's information while filling in the columns from the ⁠countrylanguage⁠ table with ⁠NULL⁠ values.

---

## Question 5

Using the World database, write the SQL command
to **list all different forms of government** found in the data.
Do **not** repeat any form of government more than once.

### SQL

```sql
SELECT DISTINCT governmentform
FROM country;
```

### Screenshot

![Q5 Screenshot] <img width="781" height="670" alt="image" src="https://github.com/user-attachments/assets/c1d3157c-757f-4ee2-aeb4-4a3a6d156aa4" />


---

## Question 6

Using the World database, write the SQL command
to **list all names of cities and countries in one column**.
Label the column **"City or Country Name"**.

### SQL

```sql
SELECT name AS "City or Country Name"
FROM city

UNION

SELECT name
FROM country;
```

### Screenshot

![Q6 Screenshot] <img width="789" height="672" alt="image" src="https://github.com/user-attachments/assets/acf3f4e7-c0e5-43f0-8f9f-4e4376d6d4d0" />


---

## Question 7

Using the World database, write the SQL command
to **list all countries by name**,
along with the **number of languages spoken in each country**.
Be sure to **sort by country name**.

### SQL

```sql
SELECT country.name,
       COUNT(countrylanguage.language) AS language_count
FROM country
LEFT JOIN countrylanguage
    ON country.code = countrylanguage.countrycode
GROUP BY country.name
ORDER BY country.name;
```

### Screenshot

![Q7 Screenshot] <img width="781" height="678" alt="image" src="https://github.com/user-attachments/assets/b6ac1ba3-5568-420a-a840-fa9c8af29eef" />


---

## Question 8

Using the World database, write the SQL command
to **list all languages**, along with the
**number of countries where each language is spoken**.
Be sure to **sort by language name**.

### SQL

```sql
SELECT language,
       COUNT(countrycode) AS country_count
FROM countrylanguage
GROUP BY language
ORDER BY language;
```

### Screenshot <img width="811" height="638" alt="image" src="https://github.com/user-attachments/assets/67d3b57c-1336-4a51-ad82-eaafbd0f751b" />


![Q8 Screenshot] 

---

## Question 9

Using the World database, write the SQL command
to **list countries that have more than two official languages**,
along with the **number of official languages spoken**.

_Hint: There are 8 such countries in this dataset._

### SQL

```sql
SELECT country.name,
       COUNT(countrylanguage.language) AS official_language_count
FROM country
JOIN countrylanguage
    ON country.code = countrylanguage.countrycode
WHERE countrylanguage.isofficial = 'T'
GROUP BY country.name
HAVING COUNT(countrylanguage.language) > 2;
```

### Screenshot

![Q9 Screenshot] <img width="783" height="641" alt="image" src="https://github.com/user-attachments/assets/8fc818b8-24e3-40c8-aefe-6981d6bda954" />


---

## Question 10

Using the World database, write the SQL command to
**find cities where the district value is missing**.

Hint: Use `LIKE` and the dash (`-`)
since some rows use that instead of actual data.

### SQL

```sql
SELECT name, district
FROM city
WHERE district LIKE CHR(8211) || '%';
```

### Screenshot

![Q10 Screenshot] <img width="780" height="643" alt="image" src="https://github.com/user-attachments/assets/a77538d8-81d7-4494-8ca1-133dbf7d562e" />


---

## Question 11

Using the World database, write the SQL command to
**calculate the percentage of cities with missing district values**.

_Hint: The result should be approximately 0.4%._

### SQL

```sql
SELECT
    COUNT(*) * 100.0 / (SELECT COUNT(*) FROM city)
        AS missing_district_percentage
FROM city
WHERE district LIKE CHR(8211) || '%';
```

### Screenshot <img width="865" height="550" alt="image" src="https://github.com/user-attachments/assets/8bdff2c9-4541-4023-b3c2-66b969f53443" />


![Q11 Screenshot](screenshots/q11_missing_district_percentage.png)
