# alemeno_credit_backend

A Dockerized Django REST API backend implementing Alemeno’s Credit Approval System assignment.  
This backend manages customer registration, loan eligibility verification, loan creation, and fetching loan records.  
It ingests initial customer and loan data from Excel files using background tasks, calculates EMIs using compound interest, and runs fully containerized with PostgreSQL and Django.

---

## Table of Contents

- [Prerequisites](#prerequisites)  
- [Setup Instructions](#setup-instructions)  
- [Environment Variables (`.env` file)](#environment-variables-env-file)  
- [Running the Project with Docker](#running-the-project-with-docker)  
- [Database Migrations](#database-migrations)  
- [Initial Data Ingestion (Optional)](#initial-data-ingestion-optional)  
- [Available API Endpoints](#available-api-endpoints)  
- [Testing](#testing)  
- [Stopping the Project](#stopping-the-project)  
- [Troubleshooting](#troubleshooting)  
- [Project Structure](#project-structure)

---

## Prerequisites

Before you start, ensure the following are installed on your system:

- [Git](https://git-scm.com/downloads) — To clone the repository  
- [Docker](https://docs.docker.com/get-docker/) — To build and run containers  
- [Docker Compose](https://docs.docker.com/compose/install/) — To manage multi-container Docker apps  

> **Note:** Docker Desktop for Windows and Mac includes Docker Compose by default.  
> For Linux, you may need to install Docker Compose separately.

---

## Setup Instructions

Follow these steps carefully to get the project running smoothly.

### Step 1: Clone the Repository

Open your terminal or command prompt and run:

git clone https://github.com/sseth345/alemeno_credit_backend.git
cd alemeno_credit_backend

### Step 2: Create `.env` File with Environment Variables

In the root directory of the project (same level as `manage.py`), create a file named `.env`.

Add the following lines to `.env`:
DB_NAME=credit_db
DB_USER=postgres
DB_PASSWORD=password
DB_HOST=db
DB_PORT=5432

## Running the Project with Docker

### Step 3: Build and Start Containers

Run this command from the project root to build and start your services:
docker compose up --build -d


- `--build` forces rebuild of images (useful if code changed).  
- `-d` runs containers in the background (detached mode).

Your Django app and PostgreSQL database will start inside Docker containers.

---

## Database Migrations

### Step 4: Run Migrations

Apply Django migrations to initialize the database schema:
docker compose exec web python manage.py migrate


- Here `web` is the service name of your Django container defined in `docker-compose.yml`.

---

## Initial Data Ingestion (Optional)

### Step 5: Load Provided Excel Data

If you have implemented data ingestion from the provided Excel files, run the management command to load initial data:

docker compose exec web python manage.py ingest_initial_data customer_data.xlsx loan_data.xlsx

- Replace filenames if they are different or place them inside the container mount path (check your docker-compose volumes).

---

## Available API Endpoints

Use an API testing tool like **[Postman](https://www.postman.com/downloads/)** or **curl** to test APIs.

| Endpoint                | Method | Description                                  |
|-------------------------|--------|----------------------------------------------|
| `/api/register/`        | POST   | Register a new customer                        |
| `/api/check-eligibility/` | POST   | Check loan eligibility                         |
| `/api/create-loan/`     | POST   | Create a new loan if eligible                  |
| `/api/view-loan/<loan_id>/`  | GET    | View details of a specific loan by loan ID    |
| `/api/view-loans/<customer_id>/` | GET    | List all active loans for a particular customer |

### Example Request for Customer Registration

POST /api/register/
Content-Type: application/json

{
"first_name": "Sita",
"last_name": "Verma",
"age": 29,
"phone_number": "9876543210",
"monthly_income": 55000
}

---

## Testing

### Step 6: Run Automated Tests

To ensure everything is working as expected, run:

docker compose exec web python manage.py test core

This will execute all unit tests located in the `core` app.

---

## Stopping the Project

### Step 7: Stop and Remove Containers

To stop running containers:
docker compose down


To also remove volumes (data):
docker compose down -v


---

## Troubleshooting

- **Docker Compose command not found:**  
  Ensure Docker Compose installed properly. Use `docker-compose` or `docker compose` as per your Docker version.

- **Database connection errors:**  
  Verify `.env` contents and that containers are running (`docker ps`).

- **Port conflicts:**  
  Check if port 8000 or 5432 is already in use.

- **API not reachable:**  
  Make sure containers are running and firewall is not blocking.

- To view logs for debugging:

docker compose logs -f web
docker compose logs -f db


---

## Project Structure Overview

alemeno_credit_backend/
│
├── core/ # Django app: models, views, serializers, tests
│ ├── migrations/ # Django migration files
│ ├── tests.py # Unit tests file
│ └── ...
│
├── customer_data.xlsx # Provided initial data (optional for ingestion)
├── loan_data.xlsx # Provided initial data (optional for ingestion)
├── Dockerfile # For building Django app container
├── docker-compose.yml # Defines web and db services
├── manage.py # Django management CLI
├── requirements.txt # Python dependencies
├── README.md # This file
├── .env # Environment variables (excluded from git)
└── ...


---

## Final Notes

- Always keep your `.env` file confidential, do not commit to version control.  
- Update documentation if you modify or add APIs.  
- This setup runs the entire application and dependencies like database in Docker, so no external setup required.  
- For detailed API request/response formats please see assignment or source code serializers.

---

## Contact / Support

For any issues or questions, refer to the project maintainer: Siddharth Seth  
Email: [sethsiddharth345@gmail.com] 

---


