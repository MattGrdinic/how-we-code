# The Importance of Documentation

|                    |                 |
| ------------------ | --------------- |
| **Content Owners** | @Owner |
| **Last Updated**   | 2025-09-30      |

---

## Related Resources

- [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)

## Documentation That Actually Helps

We've learned the hard way that poor documentation leads to operational nightmares, frustrated developers, and services that nobody wants to touch. Here's how we do documentation right.

**Remember:** _Stale documentation is worse than no documentation._

The README must be updated with every change to the project.

## Project README Requirements

---

Every project must have a README.md file that follows this exact structure. No exceptions.

```markdown
# Project Name

> **Maturity Level**: [Emerging|Basic|Mature] - (a short sentence fragement for context)

---

A sentence describing the project. Two at most.

## Usage

## How it works

## Key Considerations

## Development Considerations

### Quick Start

### Building & running

### Testing

### Versioning
```

### README Guidelines

- **No emojis** – Avoid them except for very rare info and checkboxes
- **Working links only** – Test every markdown link before committing
- **Never repeat documentation** – Reference other docs, don't duplicate
- **No file tree documentation** – It's unhelpful and gets stale immediately
- **Explain versioning** – Always document how the project is versioned (usually git tags)
- **No workstation config management** – Include version ranges, not setup instructions
- **Write for the reader** – What do they need to know?
- **Start with the most important thing** – Usage comes first
- **Be specific** – "Configure the database" is useless, "Set DATABASE_URL environment variable" is helpful
- **Include examples** – Show, don't just tell

## Operational Documentation

---

Regular maintenance and operational tasks are tracked in our team's OpsBook. Each ops task has a Jira Automation that creates tickets on schedule.

This includes tasks such as

- Checking the logs for any anomalies
- Renewing licenses or certificates
- Updating API keys for 3rd party services

## Documentation Types & When to Use Them

---

### README.md

- **When:** Every project, no exceptions
- **What:** Project overview, usage, development setup
- **Audience:** Developers, operators, anyone using the project

### CHANGELOG.md

- **When:** Every project that gets versioned releases
- **What:** What changed between versions
- **Audience:** Users upgrading, maintainers tracking changes

### Confluence Pages

- **When:** Process documentation, Ops Books, architecture decisions, team knowledge
- **What:** How we work, why we made decisions, operational procedures
- **Audience:** Team members, stakeholders, future team members

### Runbooks

- **When:** Production services
- **What:** Troubleshooting guides, emergency procedures, and monitoring setup
- **Audience:** On-call engineers, operations team

## Revision History

---

- 2025-09-30: @Owner - Complete rewrite with current standards
