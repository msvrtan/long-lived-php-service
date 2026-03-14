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
