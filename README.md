# Supporting Services: How We Operate

|                    |                 |
| ------------------ | --------------- |
| **Content Owners** | @Owner |
| **Last Updated**   | 2025-09-29      |

---

## External Resources

Many - if not all - of these principles come from already published best practices. Such sources include, but are not limited to:

- <https://www.12factor.net/>
- <https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project>
- <https://owasp.org/www-project-top-10-for-large-language-model-applications/>
- <https://agilemanifesto.org/>

## Introduction

This series of documents outlines how we strive to operate on a day to day basis. Our goal is to do things "right" the first time, every time. We also understand that there are always exceptions, and we sometimes we need to handle things differently. The ultimate goal is that we deliver safe software solutions (security) that do what they are intended to do (quality) within a reasonable timeframe (productivity).

## Our philosophy

---

1. **Security is foundational**: We build secure solutions that keep our users and intellectual property safe. We work hard to reduce risks by limiting where attacks can happen and closing off weak spots.
2. **Quality is more than passing tests**: We create solutions that do exactly what's needed. We make sure we really understand why the software is needed and what problems it solves. Just having a spec isn't enough; it has to make sense. We help the team make sure it does.
3. **Delivery**: We deliver solutions that work well. We carefully follow our definition of done. Our first release should be well documented and made for the right people. It needs to be easy to maintain, with a clear build and release process. We include the tools and steps needed to run it smoothly. It should also be easy to monitor, with alerts and paging ready from the start.

### What about "On Time"?

Yes, we'll work hard to deliver our solution on time like we agreed. It's really important to know what work is needed so we can estimate the amount of work needed so we don't risk our goals for security, quality, and delivery. But we also understand things happen; if any surprises come up that might delay us, we will quickly let everyone involved know

> __Our philosophy is simple__: if our solution, delivered when we said it would be, breaks any of these three principles, then we actually delivered it too soon, not on time!

## Topics

---

### Core Principles

1. [Security in Our Development Process](./docs/security-in-development.md)
2. [Deliver Solutions that Work as Expected](./docs/deliver-solutions-that-work.md)
3. [The Importance of Documentation](./docs/importance-of-documentation.md)

### Development Practices

1. [Managing Our Source](./docs/managing-our-source.md)
2. [Versioning Our Solutions](./docs/versioning-our-solutions.md)
3. [Builds and Deployments](./docs/builds-and-deployments.md)

### Observability

1. [Observability: Logging](./docs/observability-logging.md)
2. [Observability: Distributed Tracing](./docs/observability-distributed-tracing.md)
3. [Observability: Heartbeats, Readiness, and Liveness](./docs/observability-heartbeats-readiness-liveness.md)

### Operations

1. [Flexible Application Configuration](./docs/flexible-application-configuration.md)
2. [Scale & High Availability](./docs/scale-and-high-availability.md)
