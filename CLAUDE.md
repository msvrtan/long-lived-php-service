# Prompting

Each given prompt should be recorded in `_prompts.md`, using format:

```
## Agent: {agent & model} {datetime in ISO 8601}

Prompt: {prompt}
```

Before recording, run `date -u +"%Y-%m-%dT%H:%M:%SZ"` to get the current UTC datetime.

# Coding Standards

Follow PHP PSR and Symfony standards, utilize php-cs-fixer to enforce them.

Use phpstan to enforce the strictest rules on code & tests.
