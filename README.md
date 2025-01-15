### ETL PIPELINE

#### Step 1: Architecture

![system_architecture](https://github.com/Chukwuka1488/palantir-data-pipeline/blob/main/system_architecture.jpg?raw=true)

Key Components:

Palantir Data Source: External API providing JSON data
Cron Job Service: Scheduled data fetcher (runs at 11 PM)
MongoDB: Data storage
Flask Backend: RESTful API server with OpenAPI 3.0
Frontend Client: Consumes the API

Data Flow:

Cron job fetches data from Palantir at 11 PM daily
Data is stored/updated in MongoDB
Flask API serves data to frontend via REST endpoints
Frontend makes API calls to fetch data

#### Step 2: Tradeoffs

MongoDB vs Other Databases:

Pros:

Flexible schema for JSON data
Good performance for read-heavy operations
Easy horizontal scaling

Cons:

No strict schema validation (though we'll use Pydantic)
More memory usage compared to SQL databases

##### Flask vs FastAPI:

Pros:

Lightweight and simple
Mature ecosystem
Easy to understand and maintain

Cons:

No built-in async support
Manual OpenAPI integration needed

Cron Job Implementation:

Options:

System crontab
APScheduler with Flask
Celery with Redis

We'll use Celery with Redis

- Celery Beat: Scheduler that triggers tasks at 11 PM
- Redis: Message broker for task queue management
- Celery Workers: Process the data fetching tasks

Benefits of Celery + Redis:

Task Queue Management:

Reliable task execution
Task retry mechanisms
Task prioritization
Task monitoring and logging

Scalability:

Multiple workers can run in parallel
Easy to scale horizontally by adding more workers
Redis provides efficient message routing

Production Features:

Task status monitoring
Error handling and retries
Task result backend
Task scheduling with Celery Beat
