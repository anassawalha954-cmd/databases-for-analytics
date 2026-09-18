# Exercise 04: Advanced SQL, Jupyter, and Visualization

- Name: Anas
- Course: Database for Analytics
- Module:4 
- Database Used: World Database
- Tools Used: PostgreSQL, SQLAlchemy, Pandas, Jupyter Notebooks

---

## Instructions

- Complete each task using the **World database** installed earlier.
- For SQL questions:
  - Write the SQL command in a fenced code block
  - Execute the command and include a **screenshot of the results**
- For Jupyter Notebook questions:
  - Include the required Python statements
  - Include **screenshots of the notebook output**
- Store all screenshots in the `screenshots/` folder and embed them below each question.

---

## Question 1

Considering the World database, write a SQL statement that will
**display the names of countries**
that speak **more than two official languages**,
along with the **number of official languages spoken**.

- Sort the results by **number of languages**, from **most to least**.
- _Hint: There are fewer than 10 countries in the results._

### SQL

```sql
SELECT country.name, COUNT(countrylanguage.language) AS countLanguage
FROM country
JOIN countrylanguage ON country.code = countrylanguage.countrycode
WHERE countrylanguage.isofficial = 'T'
GROUP BY country.name
HAVING COUNT(countrylanguage.language) > 2
ORDER BY countLanguage DESC
```

### Screenshot

![Q1 Screenshot] <img width="1280" height="775" alt="image" src="https://github.com/user-attachments/assets/0d6aa0f9-c9fb-4803-b7d3-ebe2338730db" />


---

## Question 2

Using **Jupyter Notebooks**, you must use the
`create_engine` command to connect to your database.

After the `create_engine` command is executed,
**what are the three statements** required to
execute the query from Question 1 and
**display the results in the notebook**?

### Python Code

```python
sql = "SELECT country.name, COUNT(countrylanguage.language) AS countLanguage FROM country JOIN countrylanguage ON country.code = countrylanguage.countrycode WHERE countrylanguage.isofficial = 'T' GROUP BY country.name HAVING COUNT(countrylanguage.language) > 2 ORDER BY countLanguage DESC"
df = pd.read_sql(sql, engine)
display(df)

```

### Screenshot

![Q2 Screenshot] <img width="1231" height="1070" alt="image" src="https://github.com/user-attachments/assets/aa004334-947f-4d8a-8755-a4a07f174f31" />


---

## Question 3

Using **Jupyter Notebooks**, write the Python code needed
to produce the following graph:

![countries.jpg](./instructions/04-countries.jpg)

(The graph shows country-level results derived from the World database.)

### Python Code

```python
# Create the bar chart using pandas plotting and matplotlib
import matplotlib.pyplot as plt

plt.bar(df["name"], df["num_languages"])

<img width="846" height="1264" alt="image" src="https://github.com/user-attachments/assets/344675bc-2f5a-4c59-8129-8fc183b12ae8" />


plt.xlabel("name")
plt.ylabel("num_languages")

plt.xticks(rotation=90)

plt.show()

```

### Screenshot

![Q3 Screenshot](screenshots/q3_countries_graph.png)
