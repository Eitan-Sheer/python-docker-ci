# Python FastAPI - Docker CI/CD Pipeline

![CI/CD Status][![CI/CD Pipeline](https://github.com/Eitan-Sheer/python-docker-ci/actions/workflows/ci.yml/badge.svg?branch=main)](https://github.com/Eitan-Sheer/python-docker-ci/actions/workflows/ci.yml)

This repository demonstrates a complete, automated DevOps lifecycle for a modern Python web application. It showcases how to build a FastAPI application, write automated tests using `pytest`, containerize the application using `Dockerfile` optimization practices, and implement a robust CI/CD pipeline using GitHub Actions.

The main goal of this project is to ensure that every code change is automatically tested and verified before being packaged into a reusable Docker image, enforcing the core principles of Continuous Integration (CI).

## 🚀 Features

- **FastAPI Application:** A lightweight, high-performance Python web API with a dedicated `/health` check endpoint.
- **Automated Testing:** Unit tests implemented with `pytest` and `TestClient` to validate application stability.
- **Docker Containerization:** Optimized multi-layer Docker image based on `python:3.12-slim` to reduce image size and maximize build caching.
- **GitHub Actions CI/CD:** A fully automated pipeline that triggers on every `push` or `pull_request`, running tests first, and building the Docker image only if all tests pass.

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
├── requirements.txt        # Python package dependencies
└── test_main.py            # Automated test suite for health check validation
