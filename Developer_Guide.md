# Fireblocks Technical Writer API - Developer Guide

## Table of Contents
1. [Introduction](#introduction)
2. [API Overview](#api-overview)
3. [Authentication](#authentication)
4. [Endpoints](#endpoints)
   - [GET /tasks](#get-tasks)
   - [POST /tasks](#post-tasks)
5. [Error Handling](#error-handling)
6. [Best Practices](#best-practices)
7. [Code Example: POST /tasks](#code-example-post-tasks)
8. [Submission Instructions](#submission-instructions)

---

## Introduction
Welcome to the Fireblocks Technical Writer API Developer Guide. This guide explains how to interact with the API designed to manage technical writing tasks using RESTful endpoints.

---

## API Overview
This fictional API allows you to:
- Retrieve existing tasks.
- Create new tasks.

It’s ideal for automating the workflow of content teams, technical writers, and documentation pipelines.

**Base URL:**  
```
https://api.fireblocks.com
```

**Supported Format:** JSON  
**Rate Limits:** 100 requests per minute

---

## Authentication
Authentication is required for all endpoints via an API key.

**Include the following header with each request:**
```http
Authorization: Bearer YOUR_API_KEY
```

Keep your API key secure by storing it in environment variables and never exposing it in source code.

---

## Endpoints

### GET /tasks

**Description:**  
Retrieves a list of existing technical writer tasks.

- **Method**: `GET`
- **URL**: `/tasks`
- **Headers**:
  - `Authorization: Bearer YOUR_API_KEY`
  - `Content-Type: application/json`

**Success Response:**
```json
[
  {
    "id": "1",
    "title": "Draft Developer Guide",
    "status": "in-progress"
  }
]
```

**Error Response (401 Unauthorized):**
```json
{
  "error": "Invalid API key"
}
```

---

### POST /tasks

**Description:**  
Creates a new task in the system.

- **Method**: `POST`
- **URL**: `/tasks`
- **Headers**:
  - `Authorization: Bearer YOUR_API_KEY`
  - `Content-Type: application/json`

**Request Body:**

| Field        | Type   | Required | Description |
|--------------|--------|----------|-------------|
| `title`      | string | ✅ Yes   | Title of the task (max 100 chars). |
| `description`| string | ❌ No    | Additional details about the task. |
| `status`     | string | ✅ Yes   | One of: `pending`, `in-progress`, `completed`. |

**Success Response (201 Created):**
```json
{
  "id": "2",
  "title": "Review OpenAPI Spec",
  "status": "pending"
}
```

**Error Response (400 Bad Request):**
```json
{
  "error": "Missing required field: title"
}
```

---

## Error Handling

| HTTP Code | Meaning               | Notes                              |
|-----------|------------------------|-------------------------------------|
| 400       | Bad Request            | Check for missing or invalid fields. |
| 401       | Unauthorized           | Ensure API key is included and valid. |
| 429       | Too Many Requests      | Respect rate limits. |
| 500       | Internal Server Error  | Temporary issue on the server. Retry later. |

---

## Best Practices

- **Rate Limits:** Do not exceed 100 requests/min. Use retry logic with exponential backoff.
- **Secure API Keys:** Use environment variables. Never hard-code secrets.
- **Validation:** Validate your request payloads before sending.
- **Idempotency:** Use `Idempotency-Key` headers to avoid duplicate POST requests.

---

## Code Example: POST /tasks

### Python
```python
import requests
import os

url = "https://api.fireblocks.com/tasks"
headers = {
    "Authorization": f"Bearer {os.getenv('FIREBLOCKS_API_KEY')}",
    "Content-Type": "application/json"
}
data = {
    "title": "Write blog post",
    "description": "Post about API usability",
    "status": "pending"
}
response = requests.post(url, json=data, headers=headers)
print(response.json())
```

### JavaScript (Node.js)
```javascript
require('dotenv').config();
const axios = require('axios');

const headers = {
  Authorization: `Bearer ${process.env.FIREBLOCKS_API_KEY}`,
  'Content-Type': 'application/json'
};

const data = {
  title: 'Write blog post',
  description: 'Post about API usability',
  status: 'pending'
};

axios.post('https://api.fireblocks.com/tasks', data, { headers })
  .then(res => console.log(res.data))
  .catch(err => console.error(err.response.data));
```

---

