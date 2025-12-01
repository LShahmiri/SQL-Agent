#  SQL AI Agent  
An AI-powered SQL assistant that converts natural language questions into accurate PostgreSQL 16 SELECT queries.

This project allows users to ask questions about customer demographics and purchase behavior.  
The system automatically generates SQL queries and returns clear summaries using OpenAI + LangChain.

---

##  Features
- Natural language → PostgreSQL SELECT queries  
- Strict read-only queries (no INSERT/UPDATE/DELETE)  
- Supports joins, aggregations, grouping, and date filtering  
- Smart handling of ambiguous questions  
- Flask web interface  
- Hosted on Render  
- Uses LangChain SQL toolkit + GPT-4.1 reasoning

---

##  Database Schema

### **1. grocery_db.customer_details**

| Column              | Type           | Description |
|--------------------|---------------|-------------|
| customer_id        | INT           | Unique customer ID |
| distance_from_store| NUMERIC(10,2) | Miles from store |
| gender             | VARCHAR(2)    | 'M', 'F', or NULL |
| credit_score       | NUMERIC(3,2)  | Credit score (0.00–1.00) |

---

### **2. grocery_db.transactions**

| Column            | Type            | Description |
|------------------|------------------|-------------|
| customer_id      | INT              |
| transaction_id   | INT              |
| transaction_date | DATE             |
| product_area_id  | INT (1–5)        |
| num_items        | INT              |
| sales_cost       | NUMERIC(10,2)    |

---

###  Join Relationship
customer_details.customer_id = transactions.customer_id

---

##  Agent Configuration

The system uses a detailed system prompt defining:

---

 ## Requirements

Main libraries:

Flask

SQLAlchemy

Psycopg2

LangChain + langchain-openai

python-dotenv

gunicorn

Jinja2
Full list inside requirements.txt.
📁 Folder Structure
SQL-Agent/
│
├── agent/
│   ├── config.py
│   ├── sql_agent_01.py
│   ├── sql-agent-system-prompt.txt
│   └── __init__.py
│
├── static/
│   └── style.css
│
├── templates/
│   └── index.html
│
├── app.py
├── .gitignore
├── requirements.txt
└── README.md
 Example Question

User:

"Which gender lives furthest from the store on average?"

AI Response:

“Female customers live the furthest on average.”
