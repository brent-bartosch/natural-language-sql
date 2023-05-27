# ChatSQL

Natural language to SQL query converter powered by OpenAI's GPT models.

## Overview

ChatSQL lets you query MySQL databases using plain English. Describe what data you want, and the system translates your request into SQL, executes it, and returns both the raw results and a human-readable summary.

**Example:**
```
Input:  "Find all fiction books taller than 200 pages by authors from Penguin"
Output: SELECT * FROM bt WHERE Genre = 'fiction' AND Height > 200 AND Publisher = 'Penguin'
```

## Features

- **Natural Language Processing** - Write queries in plain English
- **Smart SQL Generation** - GPT translates intent to valid SQL
- **Result Interpretation** - Get human-readable summaries of query results
- **gRPC Server** - Deploy as a service for integration with other applications
- **Docker Support** - Containerized deployment option

## Setup

### Requirements
- Python 3.8+
- MySQL database
- OpenAI API key

### Installation

```bash
pip install -r requirements.txt
```

### Configuration

1. Add your OpenAI API key and database credentials to `conf.json`:
```json
{
  "OPEN_AI_KEY": "your-api-key",
  "HOST": "localhost",
  "USER": "your-user",
  "PASSWD": "your-password",
  "DATABASE": "your-database"
}
```

2. Describe your database schema in `info.json`:
```json
{
  "table_name": "Description of the table",
  "column1": "What this column contains",
  "column2": "What this column contains"
}
```

## Usage

### Command Line

```bash
cd src
python3 chatsql.py -p "Your natural language query here"
```

### gRPC Server

Start the server:
```bash
cd src
python3 main.py -p 9001
```

Connect with the example client:
```bash
python3 client.py
```

### Docker

Build and run:
```bash
make docker
make docker_run p=9001
```

Note: For Docker on Mac connecting to localhost MySQL, set `HOST` to `host.docker.internal` in `conf.json`.

## Sample Data

A books dataset (`data/books.csv`) is included for testing. Load it with:
```bash
cd src
python3 sample_data_creator.py
```

## How It Works

1. Your natural language query is sent to GPT along with the schema context from `info.json`
2. GPT generates a SQL query matching your intent
3. The query executes against your MySQL database
4. Results are returned both as raw data and as a GPT-generated summary

## Limitations

Best suited for small to medium databases where the full schema context fits within GPT's token limits. For larger databases, a vector-based approach for schema retrieval would be more effective.

## License

MIT
