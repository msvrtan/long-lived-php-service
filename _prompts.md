# LLM Prompt Log

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:36:24Z

Prompt: please add .claude and .idea folders into gitignore

## Agent: Claude Opus 4.6 (1M context) 2026-03-14 00:00:00

Prompt: Lets add Technical Specification to docs

Overview: REST API for asynchronous file format conversion. Customers submit files (CSV, JSON, XLSX, ODS) and retrieve the converted output (JSON, XML) once the long-running job completes.

API Design:
 - `POST /api/conversions` accepts `multipart/form-data` with the file in a `file` field. The input format (CSV, XLSX, ODS, JSON) is detected from the file extension and MIME type.
 - `GET /api/conversions/{id}` returns the conversion result. The response format is determined by the `Accept` header.

File Upload:
 - allowed input extensions: `.csv`, `.json`, `.xlsx`, `.ods`, stored to `var/uploads/{customerId}/{conversionId}.{ext}`
 - allowed output extensions: `.json`, `.xml`, stored to `var/results/{customerId}/{conversionId}.{ext}`

Data Cleanup: Not implemented — needs to be discussed first.

Job Lifecycle:
- customer sends a file via `POST /api/conversions`
- uploaded file is saved to local disk, new conversion entity is created and job is published to the queue
- queue consumer picks up the message and sets
  - job status to `in_process`
  - converts the file to both JSON and XML
   - success: both output files are stored on local disk, job status becomes `completed`, message is removed from the queue
   - failure: Job status becomes `errored`, error message is written to the job entity, message is removed from the queue
- customer polls `GET /api/conversions/{id}` until the result is ready

Conversion Service: fake implementation ( no file parsing)
  - generates dummy JSON and XML output files at the expected result paths
  - sleeps for 150 seconds to simulate a long-running conversion
  - always succeeds (no error scenarios in the fake implementation)

Testing Strategy:
 - Unit tests to cover domain services
 - functional tests for full API flow (upload, queue, convert, retrieve) using web test client

Authentication: API authentication is handled via JWT tokens using `lexik/jwt-authentication-bundle`.

Entity Identification: all entities use UUID v7 as their primary key instead of autoincrement integers using `symfony/uid`

Database: using SQLite, files are `var/app.db` (dev), `var/test.db` (test)

## Agent: Claude Opus 4.6 (1M context) 2026-03-14 00:00:00

Prompt: please commit

## Agent: Claude Opus 4.6 (1M context) 2026-03-14 00:00:00

Prompt: add task to _TODO.md about defining ADRs about Symfony, PHP, Sqlite, Coding Standards, using uuid, authenticating via JWT

## Agent: Claude Opus 4.6 (1M context) 2026-03-14 00:00:00

Prompt: also add these adrs: customer entity as Symfony user provider, using Symfony messenger for queues, using fixtures for tests and running CI via Github actions

## Agent: Claude Opus 4.6 (1M context) 2026-03-14 00:00:00

Prompt: please commit

## Agent: Claude Opus 4.6 (1M context) 2026-03-14 00:00:00

Prompt: adrs should be stored in docs/adrs.md file

## Agent: Claude Opus 4.6 (1M context) 2026-03-14 00:00:00

Prompt: lets start working on defining ADRs

Use Symfony 8.x

Context: The task requires a PHP framework for building a REST API. Multiple options exist (Laravel, Slim, API Platform, plain PHP).
Decision: Use Symfony 8.x as the application framework.

Reasons:
- personal proficiency + suggested as a recommended ecosystem

## Agent: Claude Opus 4.6 (1M context) 2026-03-14 00:00:00

Prompt: next adr is

Require PHP 8.5

Context: Symfony 8.x requires at least PHP 8.2 we will use latest PHP 8.5
Decision: Require PHP >= 8.5 everywhere

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T00:00:00

Prompt: new ADR

SQLite as the application database

Context: The application needs a database for storing information, while PostgreSQL is default for Symfony Doctrine it requires running a database server
Decision: Use SQLite as the database for all environments (dev, test, production).

Reasons: zero infrastructure: no database server required, just a single file (`var/app.db`)
