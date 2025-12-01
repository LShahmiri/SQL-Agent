SQL AI Agent

An AI-powered SQL assistant for querying PostgreSQL databases using natural language.

This project allows users to ask questions about customers and transactions, and the system automatically converts the question into a PostgreSQL 16 SELECT query and returns a clear, natural-language answer.

The agent is built using LangChain, OpenAI, SQLAlchemy, Flask, and Render for deployment.

Features

Convert natural-language questions into accurate PostgreSQL SELECT queries

Strict read-only querying (no INSERT/UPDATE/DELETE)

Automatic reasoning using a custom LangChain SQL Agent

Flask web interface for user interaction

Real-time chat interface

Render-hosted deployment

Includes schema-aware reasoning (tables, joins, data windows, aggregation rules)

 Database Schema
1. grocery_db.customer_details

Stores one row per unique customer.

Column	Type	Description
customer_id	INT	Unique ID
distance_from_store	NUMERIC(10,2)	Miles from store
gender	VARCHAR(2)	'M', 'F', or NULL
credit_score	NUMERIC(3,2)	Credit score (0.00–1.00)
2. grocery_db.transactions

Stores transaction-level item and cost details.

Column	Type	Description
customer_id	INT	
transaction_id	INT	
transaction_date	DATE	
product_area_id	INT (1–5)	
num_items	INT	
sales_cost	NUMERIC(10,2)	
Join Relationship
customer_details.customer_id = transactions.customer_id

SQL Agent Behavior

The system uses a detailed system prompt that defines:

✔ Query Rules

SELECT-only

Avoid double counting

Schema-qualified table names

Use DISTINCT counts for customers & transactions

Round numerical outputs for readability

Handle ambiguous questions with clarifying prompts

Respect data availability window (2020-04-01 → 2020-09-30)

✔ Tools

LangChain SQLDatabaseToolkit

OpenAI GPT-4.1 reasoning engine

✔ Example Questions

“Which gender spends more?”

“How many items were bought?”

“What is the average credit score by gender?”

 Web Interface (Flask)

The web app allows users to:

Type a question

Submit via POST

Receive the SQL-generated answer

View a modern dark/light UI

Main route (app.py):

@app.route("/", methods=["GET", "POST"])
def home():
    answer = None
    if request.method == "POST":
        question = request.form.get("question")
        if question:
            result = agent.invoke({"messages": [{"role": "user", "content": question}]})
            answer = result["messages"][-1].content
    return render_template("index.html", answer=answer)

  Environment Variables

Stored securely in Render → Environment → Edit:

OPENAI_API_KEY=...
LANGSMITH_API_KEY=...
LANGSMITH_TRACING=true

POSTGRES_HOST=...
POSTGRES_PORT=5432
POSTGRES_DBNAME=data_science_infinity
POSTGRES_USER=student
POSTGRES_PASSWORD=xxxx


.env file is ignored in GitHub for security (via .gitignore).

🚀 Deployment (Render)
Build Command
pip install -r requirements.txt

Start Command
gunicorn app:app


The app automatically loads environment variables from Render and connects to PostgreSQL with SSL.

  Requirements

Main dependencies used:

Flask

SQLAlchemy

LangChain

LangChain OpenAI

Psycopg2

python-dotenv

Gunicorn

Jinja2

(Full list in requirements.txt)

  Folder Structure
SQL-Agent/
│
├── agent/
│   ├── config.py
│   ├── sql_agent_01.py
│   ├── sql-agent-system-prompt.txt
│   └── __init__.py
│
├── templates/
│   └── index.html
│
├── static/
│   ├── style.css
│
├── app.py
├── requirements.txt
├── .gitignore
└── README.md   ← (this file)

 Example Query

User:

“On average, which gender lives furthest from the store?”

Generated SQL:

select
  gender,
  avg(distance_from_store) as avg_distance
from grocery_db.customer_details
group by gender
order by avg_distance desc;


AI Response:

“On average, female customers live the furthest from the store.”
