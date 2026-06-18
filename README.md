# Python FastAPI - Docker CI/CD Pipeline

![CI/CD Status][![CI/CD Pipeline](https://github.com/Eitan-Sheer/python-docker-ci/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/Eitan-Sheer/python-docker-ci/actions/workflows/ci.yml)

This repository demonstrates a complete, automated DevOps lifecycle for a modern Python web application. It transitions from source code to a fully tested, containerized application with verified runtime health.

## 🚀 The CI/CD Architecture (Three-Tier Validation)
Our automated pipeline ensures quality at three distinct layers before any code is considered "production-ready":
1. **Code Validation:** Unit tests with `pytest` via FastAPI's TestClient.
2. **Build Validation:** Packaging the application into a lightweight `python:3.12-slim` Docker image.
3. **Runtime Validation (Smoke Test):** The pipeline explicitly spins up the newly built container and queries the `/health` endpoint using `curl --fail` to prove the service successfully binds to the port and responds to HTTP traffic.

---

## 🛠️ Operational Guide: How to Run

### Option A: Run Locally (Development)
Ideal for writing code and running local tests.

```bash
# 1. Create and activate a virtual environment
python3 -m venv venv
source venv/bin/activate

# 2. Install required dependencies
pip install -r requirements.txt

# 3. Run the local test suite
pytest

# 4. Start the application server
uvicorn main:app --reload
