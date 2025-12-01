# 🧠 SQL AI Agent  
An AI-powered SQL assistant that converts natural language questions into accurate PostgreSQL 16 SELECT queries.

This project allows users to ask questions about customer demographics and purchase behavior.  
The system automatically generates SQL queries and returns clear summaries using OpenAI + LangChain.

---

## 🚀 Features
- Natural language → PostgreSQL SELECT queries  
- Strict read-only queries (no INSERT/UPDATE/DELETE)  
- Supports joins, aggregations, grouping, and date filtering  
- Smart handling of ambiguous questions  
- Flask web interface  
- Hosted on Render  
- Uses LangChain SQL toolkit + GPT-4.1 reasoning

---

## 🗄️ Database Schema

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

### 🔗 Join Relationship
customer_details.customer_id = transactions.customer_id

---

## 🧠 Agent Configuration

The system uses a detailed system prompt defining:

### ✔ SQL Rules
- SELECT only  
- Use LIMIT 100 when returning raw tables  
- Always schema-qualify, e.g.:  
  `grocery_db.customer_details`  
- Avoid double-counting  
- Use DISTINCT for customers & transactions  
- Round monetary outputs  
- Handle ambiguous questions with clarifying questions  
- Date window: 2020-04-01 to 2020-09-30  

---

## 🖥️ Web App (Flask)

The Flask interface:

```python
@app.route("/", methods=["GET", "POST"])
def home():
    answer = None
    if request.method == "POST":
        question = request.form.get("question")
        if question:
            result = agent.invoke({"messages": [{"role": "user", "content": question}]})
            answer = result["messages"][-1].content
    return render_template("index.html", answer=answer)
🔧 Environment Variables

Set these in Render → Environment:
OPENAI_API_KEY=...
LANGSMITH_API_KEY=...
LANGSMITH_PROJECT=ABC_Grocery_SQL_Agent
LANGSMITH_ENDPOINT=https://api.smith.langchain.com
LANGSMITH_TRACING=true

POSTGRES_HOST=...
POSTGRES_PORT=5432
POSTGRES_DBNAME=data_science_infinity
POSTGRES_USER=student
POSTGRES_PASSWORD=xxxx
.env is kept private using .gitignore.

📦 Requirements

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
🧪 Example Question

User:

"Which gender lives furthest from the store on average?"

Generated SQL:

select
  gender,
  avg(distance_from_store) as avg_distance
from grocery_db.customer_details
group by gender
order by avg_distance desc;
AI Response:

“Female customers live the furthest on average.”
