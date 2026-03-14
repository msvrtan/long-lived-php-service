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
