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

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:24:21Z

Prompt: add new ADR

Coding standards

Context: Consistent code style and static analysis are needed to maintain quality and catch issues early, especially in a project that will be reviewed by others.
Decision: Enforce coding standards with `php-cs-fixer` (Symfony ruleset) and static analysis with `phpstan` (strictest level).

Reasons:
- `php-cs-fixer` auto-fixes code style violations
- `phpstan` at max level catches warnings and errors
- both tools integrate into CI easily

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:26:30Z

Prompt: next adr

Use UUID instead of autoincrement integer for entity IDs

Context: Entities need unique identifiers. Database autoincrement integers and UUIDs are only options.
Decision: Use UUIDs as primary keys for all entities, v7 cause it's time-ordered. We will implement it via `symfony/uid`

Reasons: IDs can be generated anywhere, decoupled from db simplifying testing

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:27:51Z

Prompt: add adr

JWT authentication using lexik/jwt-authentication-bundle

Context: The API needs to authenticate customers making conversion requests. A stateless authentication mechanism is preferred for a REST API.
Decision: Use JWT (JSON Web Tokens) for API authentication via `lexik/jwt-authentication-bundle`.

Reasons: Standard approach for API authentication

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:29:30Z

Prompt: new adr

Customer entity as Symfony user provider with bcrypt hashing

Context: JWT authentication requires a user provider to validate credentials on login
Decision: create a `Customer` entity, configured as the Symfony Security user provider. Use bcrypt for password hashing.

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:31:03Z

Prompt: new adr

Symfony Messenger using filesystem

Context: The application needs asynchronous job processing for file conversions. A message queue is required to dispatch conversion jobs and process them in a background worker.
Decision: Use `symfony/messenger` with the filesystem transport (`filesystem://%kernel.project_dir%/var/messenger`) configured via the `MESSENGER_TRANSPORT_DSN` environment variable. In the test environment, use `sync://` transport for synchronous execution.
Reasons: zero infra: no external message broker (RabbitMQ, Redis) required — messages are stored as files on disk

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:32:36Z

Prompt: new adr

Use fixtures for easier testing

Context: Tests and development require a predictable set of test customers in the database.
Decision: Use `doctrine/doctrine-fixtures-bundle` with 5 hardcoded customer fixtures. Each customer has a deterministic UUID defined as a class constant, plain password `test` hashed with bcrypt

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:34:17Z

Prompt: new adr

GitHub Actions CI pipeline

Context: project needs automated checks to prevent regressions and keep code quality high
Decision: Use GitHub Actions to run php-cs-fixer, phpstan, then phpunit

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:44:00Z

Prompt: please mark 002 task as completed

## Agent: Claude Opus 4.6 (1M context) 2026-03-14T17:49:16Z

Prompt: next task should be installing symfony
