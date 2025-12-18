# Projman Service

A requirements and status tracking service built in Go that registers with the Atomic Platform's host-server for service discovery.

## Overview

Projman is a microservice that allows you to track project requirements with the following features:

- Create, read, update, and delete requirements
- Associate technologies with requirements
- Create sub-items for each requirement
- Track status of requirements and sub-items (pending, in-progress, complete)

## API Endpoints

### Requirements Management

- `GET /requirements` - List all requirements
- `GET /requirements/{id}` - Get a specific requirement
- `POST /requirements` - Create a new requirement
- `PUT /requirements/{id}` - Update a requirement
- `DELETE /requirements/{id}` - Delete a requirement

### Requirements by Status

- `GET /requirements/status/{status}` - Get requirements filtered by status (pending, in-progress, complete)

### Sub-items Management

- `POST /requirements/{id}/subitems` - Add a sub-item to a requirement
- `PUT /requirements/{id}/subitems/{subId}` - Update a sub-item
- `DELETE /requirements/{id}/subitems/{subId}` - Delete a sub-item

### Health Check

- `GET /health` - Service health check endpoint

## Data Models

### Requirement
```json
{
  "id": "req-1",
  "name": "Requirement Name",
  "description": "Requirement Description",
  "status": "pending",
  "technologies": ["Go", "JWT"],
  "subItems": [
    {
      "id": "sub-1",
      "name": "Sub-item Name",
      "status": "complete"
    }
  ],
  "createdAt": "",
  "updatedAt": ""
}
```

## Service Registration

The service automatically registers with the host-server at startup and performs periodic heartbeats every 30 seconds.

## Environment Variables

- `PORT` - Port to run the service on (default: 9094)
- `SERVICE_HOST` - Host for the service (default: localhost)
- `SERVICE_REGISTRY_URL` - URL for the host-server registry (default: http://localhost:8085/api/registry)

## Running the Service

```bash
# Build the service
go build -o projman .

# Run the service
./projman
```

Or run directly:
```bash
go run *.go
```

## Example Usage

### Create a Requirement
```bash
curl -X POST http://localhost:9094/requirements \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Implement User Authentication",
    "description": "Add user authentication with JWT tokens",
    "technologies": ["Go", "JWT", "bcrypt"],
    "status": "pending"
  }'
```

### Add a Sub-item
```bash
curl -X POST http://localhost:9094/requirements/req-1/subitems \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Create JWT middleware",
    "status": "pending"
  }'
```

## Docker

The service can also be run with Docker:

```bash
# Build the image
docker build -t projman .

# Run the container
docker run -p 9094:9094 projman
```