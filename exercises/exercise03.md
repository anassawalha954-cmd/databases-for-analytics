# Exercise 03: MongoDB – Document Queries and Analysis

- Name: Anas
- Course: Database for Analytics
- Module: 3
- Database Used: MongoDB
- Dataset: `restaurants-json.json`

---

## Instructions

- Import the provided `restaurants-json.json` file into MongoDB.
- All commands must be **executed by you** in the MongoDB shell or MongoDB Compass.
- For each query:
  - Include the MongoDB command in a fenced code block
  - Include a **screenshot** showing the command and its result
- Store screenshots in the `screenshots/` folder and embed them below each answer.

---

## Question 1

When importing the documents from `restaurants-json.json`,
**how many documents were imported into your collection**?

### Answer

25358

### Screenshot

_Show evidence of how you determined this (for example, a count query)._

```javascript
db.restaurants.countDocuments({})
```

![Q1 Screenshot] <img width="799" height="645" alt="image" src="https://github.com/user-attachments/assets/95849889-35a4-4f06-81ca-0da5fbdf6ad5" />


---

## Question 2

Before writing queries on the data,
**what command** do you use to set the
**MongoDB shell to operate on the `44661` database**?

### MongoDB Command

```javascript
use("44661")

```

### Screenshot

![Q2 Screenshot] <img width="535" height="380" alt="image" src="https://github.com/user-attachments/assets/998a5286-4991-443e-aa8d-fa494db5c1dc" />




---

## Question 3

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**locate all documents in the `"Queens"` borough**.

### MongoDB Query

```javascript
db.restaurants.find({ borough: "Queens" })
```

### Screenshot

![Q3 Screenshot] <img width="644" height="543" alt="image" src="https://github.com/user-attachments/assets/46f58dec-f6a6-40fa-b5e1-45bbb767acf1" />




---

## Question 4

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**find the number of restaurants in the `"Queens"` borough**.

### MongoDB Query

```javascript
db.restaurants.countDocuments({ borough: "Queens" })
```

### Screenshot

![Q4 Screenshot] <img width="601" height="387" alt="image" src="https://github.com/user-attachments/assets/ddcdbc54-f0d9-4e4e-b660-1ad6962c4267" />



---

## Question 5

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**find the number of restaurants** in the `"Queens"` borough
**whose cuisine is `"Hamburgers"`**.

### MongoDB Query

```javascript
db.restaurants.countDocuments({
  borough: "Queens",
  cuisine: "Hamburgers"
})
```

### Screenshot

![Q5 Screenshot] <img width="628" height="404" alt="image" src="https://github.com/user-attachments/assets/523cd46b-9826-412c-a74f-a00c1f619f29" />


---

## Question 6

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**find the number of restaurants in Zipcode `10460`**.

_Hint: Look up how to query **embedded documents**._

### MongoDB Query

```javascript
db.restaurants.countDocuments({
  "address.zipcode": "10460"
})
```

### Screenshot

![Q6 Screenshot] <img width="540" height="397" alt="image" src="https://github.com/user-attachments/assets/a62af9a2-02a5-405c-abfa-aba3855d8911" />


---

## Question 7

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**display only the names of restaurants in Zipcode `10460`**.

_Hint: Look up how to **project fields** in MongoDB._

Your output should resemble:

```json
{ name: "Wild Asia" }
{ name: "Terrace Cafe" }
{ name: "African Terrace" }
{ name: "Cool Zone" }
{ name: "Beaver Pond" }
...
```

### MongoDB Query

```javascript
db.restaurants.find(
  { "address.zipcode": "10460" },
  { _id: 0, name: 1 }
)
```

### Screenshot

![Q7 Screenshot] <img width="794" height="621" alt="image" src="https://github.com/user-attachments/assets/49317137-ac9b-4c0b-b294-3aef7ecf3ac0" />


---

## Question 8

Using your `restaurants` collection in the `44661` database,
write the MongoDB query needed to
**display only the names of restaurants whose name contains `"IHOP"`**,
ignoring case.

Your results should include:

- `"Ihop"`
- `"Ihop Restaurant"`

### MongoDB Query

```javascript
db.restaurants.find({ name: /IHOP/i }, { _id: 0, name: 1 }).forEach(r => print(r.name))
```

### Screenshot

![Q8 Screenshot] <img width="1009" height="601" alt="image" src="https://github.com/user-attachments/assets/9dd8d390-ef73-42db-b15c-a7bf0b4e48ab" />

