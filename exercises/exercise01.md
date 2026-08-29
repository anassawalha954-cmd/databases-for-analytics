# Exercise 01: World Database SQL Practice

- Name: Anas Alsawalhi
- Course: Database for Analytics
- Module: 1
- Database Used: World Database

---

See:

[MySQL: Setting Up the World Database](https://dev.mysql.com/doc/world-setup/en/)

---

## Instructions

- Answer each question below.
- All SQL commands **must be executed** against the World database.
- For each SQL command:
  - Include the SQL in a fenced code block
  - Include a **screenshot** showing the command and results
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

**Compare and contrast the data types used for:**

- `country.Population`
- `country.LifeExpectancy`

Why were these data types selected?

### Answer

⁠country.Population⁠ is stored as an integer data type (such as INT) because population represents a discrete count of whole individuals where fractions do not exist. In contrast, ⁠country.LifeExpectancy⁠ is stored as a floating-point or decimal data type (such as FLOAT or DECIMAL) because life expectancy is a continuous statistical measure that includes decimal fractions of years. These specific data types were selected to accurately reflect the mathematical nature of the data—ensuring exact whole-number counting for populations while allowing precision for statistical averages.


### Screenshot <img width="671" height="636" alt="image" src="https://github.com/user-attachments/assets/7cec8bfe-d696-4d16-93b5-ed49d18d7afb" />



_Show the table structure or DESCRIBE output._

```sql
DESCRIBE country;
```

![Q1 Screenshot](screenshots/q1_datatypes.png)

---

## Question 2

**What is the data type of `country.IndepYear`?**
Why do you think this data type was selected?

### Answer

The data type for ⁠country.IndepYear⁠ is an integer type (such as ⁠INT⁠ or ⁠SMALLINT⁠). This type was selected because years represent whole, discrete numerical values (such as 1946 or 1971) that do not require decimal fractions. Additionally, this data type allows for storing ⁠NULL⁠ values for countries that may not have gained independence or where the independence year is unknown.


### Screenshot <img width="545" height="623" alt="image" src="https://github.com/user-attachments/assets/25f762d2-3cc5-4e48-93fc-f91d5be968f4" />


```sql
DESCRIBE country;
```

![Q2 Screenshot](screenshots/q2_indepyear.png)

---

## Question 3

**Make a case for a different data type for `country.IndepYear`.**
Explain why your proposed data type might be better in some situations.

### Answer

A case could be made for using a ⁠YEAR⁠ data type or even a string type (⁠VARCHAR⁠). A string data type might be better in some situations if you need to store approximate or uncertain historical periods (such as "circa 1900" or ancient BCE years that require descriptive text). However, for standard numerical filtering and sorting, the integer type remains efficient.


---

## Question 4

Write a SQL command to **list the names of all cities in alphabetical order**.

### SQL

```sql
SELECT Name
FROM city
ORDER BY Name;
```

### Screenshot <img width="630" height="537" alt="image" src="https://github.com/user-attachments/assets/b1697dcd-24a3-474f-8f25-896e6639bcde" />



![Q4 Screenshot](screenshots/q4_cities_sorted.png)

---

## Question 5

Write a SQL command to
**list all forms of government from the `country` table**,
showing **each only once**, sorted alphabetically.

### SQL

```sql
SELECT DISTINCT GovernmentForm
FROM country     
ORDER BY GovernmentForm;
```

### Screenshot<img width="621" height="620" alt="image" src="https://github.com/user-attachments/assets/aca4b0df-4383-4bd2-accc-f85028ff3511" />



![Q5 Screenshot](screenshots/q5_government_forms.png)

---

## Question 6

Write a SQL command to **list all countries in the `Oceania` continent**.

### SQL

```sql
SELECT Name
FROM country
WHERE Continent = 'Oceania';
```

### Screenshot<img width="635" height="615" alt="image" src="https://github.com/user-attachments/assets/ae72d159-1ba0-4204-9ce9-547b4aead10f" />



![Q6 Screenshot](screenshots/q6_oceania.png)

---

## Question 7

Write a SQL command to **list the names and country code of all cities**.

### SQL

```sql
SELECT Name, CountryCode
FROM city;
```

### Screenshot<img width="611" height="610" alt="image" src="https://github.com/user-attachments/assets/751a8dd3-f657-40d9-978c-e3a5b08817a0" />




![Q7 Screenshot](screenshots/q7_city_countrycode.png)

---

## Question 8

Write a SQL command to **update the city named `"Nashville-Davidson"` to `"Nashville"`**.

### SQL

```sql
UPDATE city
SET Name = 'Nashville'
WHERE Name = 'Nashville-Davidson';
```

### Screenshot<img width="639" height="628" alt="image" src="https://github.com/user-attachments/assets/e93f9a2e-1646-4127-816e-a53b270a897b" />



![Q8 Screenshot](screenshots/q8_update_city.png)

---

## Question 9

Write a SQL command to **insert a new country named `"Narnia"`**
with a country code of `"NAR"`.
Use reasonable values for the remaining columns.

### SQL

```sql
INSERT INTO country (Code, Name, Continent, Region, Population)
VALUES ('NAR', 'Narnia', 'Europe', 'Fantasy', 1000000);
```

### Screenshot<img width="625" height="563" alt="image" src="https://github.com/user-attachments/assets/f57adeb6-5e08-4179-bd0e-434b6cd95e09" />



![Q9 Screenshot](screenshots/q9_insert_narnia.png)

---

## Question 10

Write a SQL command to **delete the country with the country code `"NAR"`**.

### SQL

```sql
DELETE FROM country
WHERE Code = 'NAR';
```

### Screenshot<img width="1280" height="1036" alt="image" src="https://github.com/user-attachments/assets/1d9151c0-32e5-46a8-9a9b-f555a3ad8a3c" />


![Q10 Screenshot](screenshots/q10_delete_narnia.png)
