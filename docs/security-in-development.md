# Security in Our Development Process

|                    |                 |
| ------------------ | --------------- |
| **Content Owners** | @Owner |
| **Last Updated**   | 2025-10-02      |

---

## Other References

- <https://owasp.org/www-project-top-10-for-large-language-model-applications/>
- <https://owasp.org/www-project-top-ten/>

## Writing Secure Code

---

**Treat all input as hostile.** Every piece of data coming into our applications gets validated and sanitized before we use it. This means validating data types, lengths, and formats at API boundaries, using parameterized queries for all database operations, sanitizing any data that gets displayed back to users, and never trusting data from third-party APIs without validation.

**Keep secrets out of source code.** API keys, database passwords, and other secrets never go in our code repositories. Use environment variables or secret management systems, add `.env` files to `.gitignore`, rotate secrets regularly, and use different secrets for development, staging, and production. Red flags include hardcoded passwords or API keys in code, committing `.env` files to git, or sharing production credentials in Slack or email.

**Vet your dependencies.** Every third-party library we add gets evaluated for security and licensing. Check for known vulnerabilities, review the license terms, consider the maintenance status, and look at the project's security track record. Our CI pipeline includes automated vulnerability scanning, license compliance checking, and regular dependency updates.

## Development Workflow Security

---

**Every code change gets reviewed.** No code reaches production without another developer looking at it for security vulnerabilities, proper input validation, secrets or sensitive data in code, and adherence to our secure coding standards.

**Main branches are protected.** Our main branches have protection rules that prevent direct pushes and require passing tests. Pull requests must be approved, all CI checks must pass, no force pushes are allowed, and branches must be up to date before merging.

**Automate security scanning.** Our CI pipeline automatically checks for security issues before code gets deployed, including source code vulnerabilities, dependency issues, Docker image problems, and infrastructure code misconfigurations.

## Data Protection

---

**PHI never leaves production.** We never use real patient data in development or testing environments. Instead, we use synthetic test data that looks real but isn't, data masking tools for necessary production debugging, and separate databases for development that never contain PHI.

**Log thoughtfully.** Our logs help us debug issues without exposing sensitive information. We log application errors and exceptions, performance metrics, and user actions (without personal details). We never log passwords or API keys, personal health information, credit card numbers or SSNs, or full request/response bodies with sensitive data.

**Lock down containers.** Our Docker images follow security best practices by running as non-root users, using minimal base images, never baking secrets into images, and getting regular base image updates.

## When Security Issues Happen

---

**Fix the root cause, not just the symptom.** When we find a security issue, we don't just patch it - we understand why it happened and prevent it from happening again. Our process: fix the immediate issue, understand how it happened, check for similar problems elsewhere, update our processes to prevent recurrence, and document lessons learned.

**Escalate quickly.** Security issues get escalated immediately to the security team, product owners, infrastructure team (if needed), and compliance team (for PHI-related issues). Don't bury security problems in a backlog.

This keeps us focused on what we as developers actually control while building secure, reliable software for our users.

## Revision History

---

- 2025-10-02: @Owner - Complete rewrite with current standards
