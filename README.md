#  SQL AI Agent  
**Live Demo:**  
https://sql-agent-1-te6s.onrender.com/
More Description : https://lshahmiri.github.io/2025/12/01/AI-SQL-Agent.html

<img width="918" height="426" alt="SQL-AGENT" src="https://github.com/user-attachments/assets/59506390-c936-4e24-ac27-a98e20c63104" />


An AI-powered SQL assistant that converts natural language questions into accurate PostgreSQL SELECT queries.
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

 ## Example Question

User:

"Which gender lives furthest from the store on average?"

AI Response:

“Female customers live the furthest on average.”
