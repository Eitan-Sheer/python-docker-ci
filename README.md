# Python FastAPI - Docker CI/CD Pipeline

[![CI/CD Pipeline](https://github.com/Eitan-Sheer/python-docker-ci/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/Eitan-Sheer/python-docker-ci/actions/workflows/ci.yml)

This repository demonstrates a complete, automated DevOps lifecycle for a modern Python web application. It transitions from source code to a fully tested, containerized application with verified runtime health.

## 🚀 The CI/CD Architecture (Three-Tier Validation)
Our automated pipeline ensures quality at three distinct layers before any code is considered "production-ready":
1. **Code Validation:** Unit tests with `pytest` via FastAPI's TestClient.
2. **Build Validation:** Packaging the application into a lightweight `python:3.12-slim` Docker image.
3. **Runtime Validation (Smoke Test):** The pipeline explicitly spins up the newly built container and queries the `/health` endpoint using `curl --fail` to prove the service successfully binds to the port and responds to HTTP traffic.

---

## 📂 Project Structure

```text
├── .github/
│   └── workflows/
│       └── ci.yml          # GitHub Actions pipeline configuration
├── venv/                   # Local Python virtual environment (ignored by Git/Docker)
├── .dockerignore           # Prevents unnecessary files from entering the Docker image
├── .gitignore              # Prevents environment and temporary files from being tracked
├── Dockerfile              # Instructions for building the application container
├── main.py                 # FastAPI application core logic and endpoints
├── requirements.txt        # Production runtime dependencies (FastAPI, Uvicorn)
├── requirements-dev.txt    # Development & Testing dependencies (includes Pytest, HTTPX)
├── test_main.py            # Automated test suite for health check validation
└── README.md               # Operational guide and documentation
---

## 🛠️ Operational Guide

### How to run locally
To set up the application environment on your local machine for development, execute the following commands:

```bash
# 1. Create a Python virtual environment
python3 -m venv venv

# 2. Activate the virtual environment
source venv/bin/activate

# 3. Install all development and testing dependencies
pip install -r requirements-dev.txt

# 4. Start the local Uvicorn development server with live-reload enabled
uvicorn main:app --reload

*The local development server will be accessible at `http://127.0.0.1:8000`.*
*Interactive Swagger API documentation can be viewed at `http://127.0.0.1:8000/docs`.*

### Run tests
To execute the automated test suite locally and verify application logic before containerization, run:

# Run unit tests using pytest
pytest

Run with Docker
To build the production-ready container image and run it locally (simulating a production environment without needing Python installed on the host), use these commands:


# 1. Build the production Docker image (installs ONLY production runtime dependencies)
docker build -t python-docker-ci .

# 2. Run the container in detached mode (-d), mapping host port 8000 to container port 8000
docker run -d -p 8000:8000 --name my-app python-docker-ci

# 3. Verify the containerized application is running and responding to traffic
curl http://localhost:8000/health

CI pipeline
The automated Continuous Integration workflow is defined in .github/workflows/ci.yml. It triggers on every push or pull_request to the main branch and executes two isolated, sequential jobs:

Test Job:

Spawns a clean ubuntu-latest runner.

Sets up Python 3.12.

Installs development dependencies via requirements-dev.txt.

Runs pytest. If any test fails, the entire pipeline stops immediately to prevent broken builds.

Build & Smoke Test Job:

Runs only after the Test Job passes successfully (needs: test).

Standardizes the environment by building the production Docker image using requirements.txt.

Launches a temporary instance of the container.

Executes a live Smoke Test using curl --fail http://localhost:8000/health. If the container crashes on startup or fails to respond, the pipeline fails, ensuring that un-runnable images are never deployed.