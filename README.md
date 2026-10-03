# Chat with a Database: Natural Language to SQL Chatbot (Langflow)

An AI chatbot that answers business questions about a music store's database in plain language. It translates each question into a SQL query, runs it, and explains the result.

![Langflow canvas](images/flow.png)

## Overview

Business questions about sales data normally require writing SQL. This project builds a chatbot, with no hand-written code, that lets anyone ask questions like *"Which country generates the most revenue?"* and get an answer backed by a real query.

The data is the **Chinook** sample database (a digital music store, 2009-2013). Revenue in this store is flat at roughly $450-$480 per year, so the chatbot is used to explore where the business could grow: top countries, genres, artists, customers, and inactive customers.

## How It Works

```
Chat Input → Agent (Gemini) ⇄ SQL Database tool (SQLite) → Chat Output
```

- **Agent:** a Gemini model that decides when to query the database and writes the SQL
- **SQL Database (Tool Mode):** executes the generated query on the Chinook SQLite file
- **Agent instructions:** include the database schema and relationships, so the agent knows the table names and joins instead of guessing

## Tech Stack

Langflow, Google Gemini, SQLite, SQL

## Setup

1. Download the Chinook SQLite database from the [Chinook releases](https://github.com/lerocha/chinook-database/releases) (it is not included in this repo; check its license on that page).
2. Open Langflow and import `flow/chinook-sql-chatbot.json`.
3. Add your own Google API key in the Agent component.
4. In the SQL Database component, set the database URL to your local file, for example `sqlite:///C:/path/to/chinook.db`.
5. Open the Playground and start asking questions.

## Example Questions

- How many customers are there?
- Which country generates the most revenue?
- Which genre sells the best?
- Who are the top 3 customers by total spending?
- What percentage of revenue comes from the USA?

## Evaluation

I tested the chatbot against answers computed with manual SQL queries.

| Question | Expected | Chatbot | Correct? |
|---|---|---|---|
| How many customers are there? | 59 | There are 59 customers. | ✔ |
| How many tracks, albums, and artists are there? | 3,503 tracks, 347 albums, 275 artists | There are 3,503 tracks, 347 albums, 275 artists. | ✔ |
| What is the average invoice total? | $5.65. | The average invoice total is $5.65. | ✔ |


**Score: 3 / 3 correct.** 


## Limitations

- The agent can write incorrect SQL or misread ambiguous terms such as "inactive customers", so answers should be verified against manual queries.
- It only knows the tables described in its instructions.
- Use a copy of the database, since the agent can execute any query it generates.
