# Fireblocks Technical Writer API - Developer Guide

## Introduction
This guide provides an overview of the Fireblocks Technical Writer API, which allows users to manage technical writing tasks. The API consists of two endpoints: 
- `GET /tasks`: Retrieve a list of existing tasks.
- `POST /tasks`: Create a new task.

This document explains how to authenticate, interact with these endpoints, handle errors, and follow best practices when using the API.

## API Overview
- **Base URL**: `https://api.fireblocks.com`
- **Authentication**: API key authentication
- **Supported Formats**: JSON
- **Rate Limits**: 100 requests per minute

## Authentication
The API requires an API key for authentication. Include your API key in the `Authorization` header for each request:
```http
Authorization: Bearer YOUR_API_KEY
```

## Endpoints
### GET /tasks
#### Description
Retrieves a list of existing tasks.

#### Request
- **Method**: `GET`
- **URL**: `/tasks`
- **Headers**:
  - `Authorization: Bearer YOUR_API_KEY`
  - `Content-Type: application/json`

#### Response
- **Success (200 OK)**
```json
[
  {
    "id": "123",
    "title": "Write API Documentation",
    "status": "in-progress"
  }
]
```
- **Error (401 Unauthorized)**
```json
{
  "error": "Invalid API key"
}
```

### POST /tasks
#### Description
Creates a new task.

#### Request
- **Method**: `POST`
- **URL**: `/tasks`
- **Headers**:
  - `Authorization: Bearer YOUR_API_KEY`
  - `Content-Type: application/json`
- **Body**:
```json
{
  "title": "Review Technical Guide",
  "description": "Ensure clarity and completeness of API documentation",
  "status": "pending"
}
```

#### Response
- **Success (201 Created)**
```json
{
  "id": "124",
  "title": "Review Technical Guide",
  "status": "pending"
}
```
- **Error (400 Bad Request)**
```json
{
  "error": "Missing required field: title"
}
```

## Python Example
```python
import requests

API_URL = "https://api.fireblocks.com/tasks"
API_KEY = "your_api_key_here"

headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

def create_task():
    data = {
        "title": "Review API Structure",
        "description": "Check endpoints and responses",
        "status": "pending"
    }
    response = requests.post(API_URL, json=data, headers=headers)
    print(response.json())

create_task()
```

## Best Practices
- **Rate Limits**: Avoid exceeding 100 requests per minute.
- **Error Handling**: Implement logic to retry requests on transient errors (e.g., 500 errors).
- **Secure API Key**: Store API keys securely and avoid hardcoding them in source code.
- **Validate Input**: Ensure request payloads are correctly formatted before sending.


---
