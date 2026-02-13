# Celery Tutorial Project

A simple Celery project demonstrating asynchronous task processing with Redis as both message broker and result backend.

## Prerequisites

- Docker and Docker Compose
- Python 3.7+
- Redis (provided via Docker)

## Project Structure

```bash
celery-tutorial/
├── tasks.py # Celery tasks definition
├── docker-compose.yml # Redis services configuration
├── requirements.txt # Python dependencies
└── README.md # This file
```

## Setup Instructions

### 1. Start Redis Services

```bash
docker-compose up -d
```

This starts:

- Redis server on port 6379
- Redis Commander (web UI) on port 8081

### 2. Create Virtual Environment

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Start Celery Worker

```bash
celery -A tasks worker --loglevel=info
```

### 5. Test the Task

In another terminal, run Python and execute:

```bash
from tasks import process
result = process.delay(3, 4)
print(result.get())  # Should output 25 (3² + 4² = 9 + 16 = 25)
```

## Task Details

The process task:

- Takes two numbers (x, y)
- Sleeps for 5 seconds (simulating work)
- Prints "processing" every second
- Returns x² + y²

## Monitoring

Access Redis Commander at http://localhost:8081 to monitor Redis queues and data.

### Useful Commands

- Check worker status: celery -A tasks status
- Inspect active tasks: celery -A tasks inspect active
- Purge all tasks: celery -A tasks purge
- Stop Redis: docker-compose down

## License

MIT
