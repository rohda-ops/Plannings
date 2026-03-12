Parser Agent
Takes the user's natural language question and extracts the intent — what data they want, what conditions they want, what tables or columns might be involved. Outputs a structured representation of the question.
Schema Discovery Agent
Connects to the database using MCP Toolbox / pymssql. Queries system metadata tables to retrieve all table names, column names, data types, primary keys, and foreign key relationships. Outputs the full database structure.
Data Sampler Agent
Runs SELECT queries with a small LIMIT (like 5-10 rows) on each relevant table to fetch sample data. Outputs actual row data so downstream agents can understand what real values look like in each column.
Analysis Agent
Takes the schema info and sampled data, then determines the semantic meaning of each column — what it represents, what values it contains, which columns across different tables hold similar data. Outputs column-level descriptions and observations.
Graph Builder Agent
Takes the primary key and foreign key relationships from Schema Discovery. Creates a NetworkX graph where each table is a node and each foreign key relationship is an edge with join column metadata attached. Outputs the graph object.
Graph Query Agent
Receives a query about table relationships and traverses the NetworkX graph to find paths between tables, neighboring tables, or connected components. Outputs the path and the join columns for that path.
Mapper Agent
Takes the parsed user question, schema info, analysis results, and graph query results. Determines which tables are needed, how to join them, which columns to select, and which conditions to apply. Outputs a structured mapping JSON.
SQL Generator Agent
Takes the mapping JSON from the Mapper and converts it into valid SQL syntax. Outputs the final SQL query.
Report Generator Agent
Takes the SQL output or query results and produces a human-readable summary. Outputs the final report.