# Exercise 05: SQLDA Database - Dates, Data Quality, Arrays, and JSON

- Name: Anas
- Course: Database for Analytics
- Module: 5
- Database Used: `sqlda` (Sample Datasets)
- Tools Used: PostgreSQL (pgAdmin or psql)

---

## Instructions

- Use the **sqlda** database from the "Loading the Sample Datasets" instructions.
- For each SQL task:
  - Include your SQL in a fenced code block
  - Execute it and include a **screenshot** showing the query and results
- Store screenshots in the `screenshots/` folder and embed them below each answer.
- For explanation questions:
  - Write your answer in complete sentences
  - Include a screenshot if requested

---

## Question 1

Using the `sqlda` database, write the SQL needed
to show a **list of years** that emails were sent.

Your results should list years like this (order matters):

```text
year
2011
2013
2014
2015
2016
2017
2018
2019
```

### SQL

```sql
SELECT DISTINCT
    EXTRACT(YEAR FROM sent_date) AS year
FROM emails
ORDER BY year;
```

### Screenshot

![Q1 Screenshot] <img width="585" height="760" alt="image" src="https://github.com/user-attachments/assets/4756a10c-20c7-4c67-ae16-be57bd832e49" />


---

## Question 2

Using the `sqlda` database, write the SQL needed to
show the **number of messages sent by year**,
ordered by year (as shown in the prompt).

Output should resemble:

```text
count   year
...
```

### SQL

```sql
SELECT
    EXTRACT(YEAR FROM sent_date) AS year,
    COUNT(*) AS messages_sent
FROM emails
GROUP BY EXTRACT(YEAR FROM sent_date)
ORDER BY year;
```

### Screenshot

![Q2 Screenshot] <img width="531" height="776" alt="image" src="https://github.com/user-attachments/assets/442535c0-b3b5-4921-b7a3-987d5f6bf27f" />


---

## Question 3

Using the `sqlda` database, write the SQL needed to show:

- the **sent date**
- the **opened date**
- the **interval** between the two

Only include emails that contain **both** a sent date and an opened date.

### SQL

```sql
SELECT
    sent_date,
    opened_date,
    opened_date - sent_date AS interval
FROM emails
WHERE sent_date IS NOT NULL
  AND opened_date IS NOT NULL;
```

### Screenshot

![Q3 Screenshot] <img width="681" height="797" alt="image" src="https://github.com/user-attachments/assets/45435f5d-db4c-40e1-93cd-b258924ce9bd" />


---

## Question 4

Using the `sqlda` database,
write the SQL needed to
show emails that contain an **opened date BEFORE the sent date**.

### SQL

```sql
SELECT
    sent_date,
    opened_date,
    opened_date - sent_date AS interval
FROM emails
WHERE opened_date IS NOT NULL
  AND sent_date IS NOT NULL
  AND opened_date < sent_date;
```

### Screenshot

![Q4 Screenshot] <img width="521" height="834" alt="image" src="https://github.com/user-attachments/assets/972efc34-e561-4846-ae18-009eb44d7e20" />


---

## Question 5

Using the `sqlda` database:
there are **over 100 emails**
that contain an opened date **BEFORE** the sent date.

After looking at the data, **why is this the case?**

### Answer

The reason why some emails contain an opened date before the sent date is because ⁠sent_date⁠ is often logged as a standard/default batch timestamp (typically ⁠15:00:00⁠ or a coordinated universal time), whereas ⁠opened_date⁠ is recorded based on the actual local time when the customer opens the email, which can sometimes precede that default time stamp.


### Screenshot (if requested by instructor)

![Q5 Screenshot](screenshots/q5_explain_date_issue.png)

---

## Question 6

Using the `sqlda` database, explain in your own words what the following code does:

```sql
CREATE TEMP TABLE customer_points AS (
    SELECT
        customer_id,
        point(longitude, latitude) AS lng_lat_point
    FROM customers
    WHERE longitude IS NOT NULL
    AND latitude IS NOT NULL
);

CREATE TEMP TABLE dealership_points AS (
    SELECT
        dealership_id,
        point(longitude, latitude) AS lng_lat_point
    FROM dealerships
);

CREATE TEMP TABLE customer_dealership_distance AS (
    SELECT
       customer_id,
       dealership_id,
       c.lng_lat_point <@> d.lng_lat_point AS distance
    FROM customer_points c
    CROSS JOIN dealership_points d
);
```

### Answer

The code creates three temporary tables to handle spatial/geographical data for customers and dealerships:
1. ⁠customer_points⁠: Converts the longitude and latitude of customers into point data types, filtering out any null values.
2. ⁠dealership_points⁠: Converts the longitude and latitude of dealerships into point data types.
3. ⁠customer_dealership_distance⁠: Performs a cross join between customer points and dealership points and uses the PostGIS/PostgreSQL distance operator (⁠<@>⁠) to calculate the distance between them.


---

## Question 7

Using the `sqlda` database,
write SQL to display an
**array of salespeople for each dealership**,
sorted by dealership.

For example - dealership 1 is below:

```text
"{""Fidell,Granville"",""Onele,Jereme"",""Sheriff,Lelia"",""McSpirron,Massimiliano"",""Rennick,Nadia"",""Mace,Eveleen"",""Oxteby,Dukie"",""Spong,Marcos"",""Wogden,Quent"",""Duny,Sandye"",""Loraine,Englebert"",""Meere,Ira"",""Gibbens,Cristine"",""Prine,Lyda"",""McCoughan,Sheff"",""Schule,Giselbert"",""McAndie,Eleen"",""Dosedale,Dorie"",""Nafziger,Shay""}"
```

### SQL

```sql
SELECT
    dealership_id,
    ARRAY_AGG(last_name || ' ' || first_name) AS salespeople
FROM salespeople
GROUP BY dealership_id
ORDER BY dealership_id;
```

### Screenshot

![Q7 Screenshot] <img width="1280" height="672" alt="image" src="https://github.com/user-attachments/assets/b6c407b8-50e7-4713-b748-c36456b57b39" />


---

## Question 8

Using the `sqlda` database, write SQL to display:

- an **array of salespeople for each dealership**
- the **state** of the dealership
- the **number of salespeople** for the dealership

Sort by **state**.

Reference image:

![05-ExerciseArray](./instructions/05-ExerciseArray.jpg)

### SQL

```sql
SELECT
    d.dealership_id,
    d.state,
    ARRAY_AGG(s.first_name || ' ' || s.last_name) AS salespeople,
    COUNT(s.salesperson_id) AS number_of_salespeople
FROM dealerships AS d
JOIN salespeople AS s
    ON d.dealership_id = s.dealership_id
GROUP BY
    d.dealership_id,
    d.state
ORDER BY
    d.state;
```

### Screenshot

![Q8 Screenshot] <img width="1097" height="775" alt="image" src="https://github.com/user-attachments/assets/aafd70b0-df45-486d-af70-75f9658cba0e" />


---

## Question 9

Using the `sqlda` database, write the SQL needed to convert
the **customers** table to **JSON**.

### SQL

```sql
SELECT row_to_json(customers)
FROM customers;
```

### Screenshot

![Q9 Screenshot] <img width="1280" height="707" alt="image" src="https://github.com/user-attachments/assets/39ca4e0d-f626-4aa2-a42a-f0ff93a36b7c" />


---

## Question 10

Using the `sqlda` database, write SQL to display:

- an **array of salespeople for each dealership**
- the **state**
- the **number of salespeople**
- sorted by **state**

Then **convert this result to JSON**.

Reference image:

![05-ExerciseArray-1](./instructions/05-ExerciseArray-1.jpg)

### SQL

```sql
SELECT row_to_json(dealership_data)
FROM (
    SELECT
        d.dealership_id,
        d.state,
        ARRAY_AGG(s.first_name || ' ' || s.last_name) AS salespeople,
        COUNT(s.salesperson_id) AS number_of_salespeople
    FROM dealerships AS d
    JOIN salespeople AS s
        ON d.dealership_id = s.dealership_id
    GROUP BY
        d.dealership_id,
        d.state
    ORDER BY
        d.state
) AS dealership_data;
```

### Screenshot

![Q10 Screenshot] <img width="1280" height="695" alt="image" src="https://github.com/user-attachments/assets/378da4d7-5b92-4a6b-8cef-2be828cb3210" />

