# Architecture Decision Records

## ADR-001: Use Symfony 8.x as the Application Framework

**Status:** Accepted

**Context:** The task requires a PHP framework for building a REST API that handles asynchronous file format conversion. Multiple options exist, including Laravel, Slim, API Platform, and plain PHP.

**Decision:** Use Symfony 8.x as the application framework.

**Reasons:**
- Personal proficiency with Symfony

## ADR-002: Require PHP 8.5

**Status:** Accepted

**Context:** Symfony 8.x requires at least PHP 8.2. PHP 8.5 is the latest stable release.

**Decision:** Require PHP >= 8.5 everywhere.

**Reasons:**
- Latest stable PHP release with modern language features
- Meets and exceeds Symfony 8.x minimum requirement

## ADR-003: SQLite as the Application Database

**Status:** Accepted

**Context:** The application needs a database for storing information. While PostgreSQL is the default for Symfony Doctrine, it requires running a database server.

**Decision:** Use SQLite as the database for all environments (dev, test, production).

**Reasons:**
- Zero infrastructure: no database server required, just a single file (`var/app.db`)

## ADR-004: Coding Standards

**Status:** Accepted

**Context:** Consistent code style and static analysis are needed to maintain quality and catch issues early, especially in a project that will be reviewed by others.

**Decision:** Enforce coding standards with `php-cs-fixer` (Symfony ruleset) and static analysis with `phpstan` (strictest level).

**Reasons:**
- `php-cs-fixer` auto-fixes code style violations
- `phpstan` at max level catches warnings and errors
- Both tools integrate into CI easily

## ADR-005: UUID v7 for Entity Identification

**Status:** Accepted

**Context:** Entities need unique identifiers. Database autoincrement integers and UUIDs are the available options.

**Decision:** Use UUID v7 as primary keys for all entities, implemented via `symfony/uid`. V7 is time-ordered, which provides natural chronological sorting.

**Reasons:**
- IDs can be generated anywhere, decoupled from the database
- Simplifies testing by removing database dependency for ID generation

## ADR-006: JWT Authentication via lexik/jwt-authentication-bundle

**Status:** Accepted

**Context:** The API needs to authenticate customers making conversion requests. A stateless authentication mechanism is preferred for a REST API.

**Decision:** Use JWT (JSON Web Tokens) for API authentication via `lexik/jwt-authentication-bundle`.

**Reasons:**
- Standard approach for stateless API authentication
