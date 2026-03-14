# Technical Specification

## Overview

REST API for asynchronous file format conversion. Customers submit files (CSV, JSON, XLSX, ODS) and retrieve the converted output (JSON, XML) once the long-running job completes.

## API Design

### `POST /api/conversions`

Accepts `multipart/form-data` with the file in a `file` field.

The input format (CSV, XLSX, ODS, JSON) is detected from the file extension and MIME type.

**Response:** `201 Created` with the conversion entity (including its `id` and initial `status`).

### `GET /api/conversions/{id}`

Returns the conversion result. The response format is determined by the `Accept` header (`application/json` or `application/xml`).

**Response:** The conversion entity with its current status. When `completed`, includes the converted output.

## File Storage

### Input Files

- Allowed extensions: `.csv`, `.json`, `.xlsx`, `.ods`
- Storage path: `var/uploads/{customerId}/{conversionId}.{ext}`

### Output Files

- Allowed extensions: `.json`, `.xml`
- Storage path: `var/results/{customerId}/{conversionId}.{ext}`

### Data Cleanup

Not implemented — needs to be discussed first.

## Job Lifecycle

1. Customer sends a file via `POST /api/conversions`.
2. Uploaded file is saved to local disk, a new conversion entity is created, and a job message is published to the queue.
3. Queue consumer picks up the message:
   - Sets job status to `in_process`.
   - Converts the file to both JSON and XML.
     - **Success:** Both output files are stored on local disk, job status becomes `completed`, message is removed from the queue.
     - **Failure:** Job status becomes `errored`, error message is written to the job entity, message is removed from the queue.
4. Customer polls `GET /api/conversions/{id}` until the result is ready.

### Status Flow

```
pending → in_process → completed
                     → errored
```

## Conversion Service

Fake implementation (no actual file parsing):

- Generates dummy JSON and XML output files at the expected result paths.
- Sleeps for 150 seconds to simulate a long-running conversion.
- Always succeeds (no error scenarios in the fake implementation).

## Authentication

API authentication is handled via JWT tokens using `lexik/jwt-authentication-bundle`.

## Entity Identification

All entities use UUID v7 as their primary key instead of autoincrement integers, using `symfony/uid`.

## Database

SQLite with the following database files:

- **Development:** `var/app.db`
- **Test:** `var/test.db`

## Testing Strategy

- **Unit tests:** Cover domain services.
- **Functional tests:** Full API flow (upload, queue, convert, retrieve) using web test client.
