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
7. [Code Examples](#code-examples)
8. [Submission](#submission)

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

| Field | Type | Required | Description |
|--------|------|----------|-------------|
| `title` | `string` | ✅ Yes | The title of the task (max 100 chars). |
| `description` | `string` | ❌ No | Details about the task. |
| `status` | `string` | ✅ Yes | Can be `pending`, `in-progress`, or `completed`. |

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

## Error Handling
| Status Code | Error Code | Meaning | How to Fix |
|-------------|-----------|---------|------------|
| `400` | `invalid_input` | Missing or malformed request fields | Ensure all required fields are sent in the correct format. |
| `401` | `unauthorized` | Invalid API key | Check if your API key is correct. |
| `429` | `rate_limited` | Too many requests | Implement retry logic with exponential backoff. |

## Best Practices
- **Rate Limits**: Avoid exceeding 100 requests per minute. Implement retry logic with exponential backoff.
- **Pagination**: Use pagination for large datasets.
- **Idempotency**: Use an `Idempotency-Key` header to prevent duplicate requests.
- **Secure API Key**: Store API keys securely and avoid hardcoding them in source code.
- **Validate Input**: Ensure request payloads are correctly formatted before sending.

## Submission
Ensure the guide is well-structured and submitted as a GitHub repository containing:
1. `README.md` (this guide)
2. `openapi.yaml` (API specification)
3. Optional: An HTML version for improved readability.
