# Observability: Distributed Tracing

|                    |                 |
| ------------------ | --------------- |
| **Content Owners** | @Owner |
| **Last Updated**   | 2025-09-29      |

---

## Other Resources

[OpenTelemetry Instrumentation Guides](https://opentelemetry.io/docs/instrumentation/)

## What is Distributed Tracing?

---

We rarely have an isolated singular service. Services use other services; a single user request might touch 5, 10, or even 20 different services. Distributed tracing tracks that request's entire journey, showing you exactly where time is spent and where things go wrong. Think of it like GPS tracking for your requests - you can see the complete path from start to finish, including any detours or traffic jams along the way. Without distributed tracing, debugging microservices is like trying to solve a mystery with half the clues missing. You might know something went wrong, but good luck figuring out where or why.

Distributed tracing gives you:

- **End-to-end visibility**: See exactly how requests flow through your system
- **Performance insights**: Spot slow services, database calls, or network issues
- **Error context**: When something breaks, see exactly where and what caused it
- **Dependency mapping**: Understand which services depend on what

For tracing to work, each service needs to pass the trace context to the next service. This happens automatically with most modern tracing libraries - they add headers to HTTP requests, message queue messages, etc.

## What to Trace

---

### Always Trace

- **External calls**: HTTP requests, database queries, message publishing
- **Service boundaries**: Incoming requests and outgoing responses
- **Critical business operations**: Payment processing, user authentication, etc.

### Consider Tracing

- **Expensive operations**: File I/O, complex calculations, third-party API calls
- **Error-prone areas**: Places where things often go wrong
- **Performance bottlenecks**: Operations you're trying to optimize

### Don't Over-Trace

Tracing adds overhead, so be selective. You don't need to trace every function call - focus on the operations that matter for debugging and performance monitoring.

## Implementation with OpenTelemetry

---

OpenTelemetry is the industry standard for distributed tracing. It works with all major programming languages and integrates with most observability platforms.

### Auto-Instrumentation

Most languages offer auto-instrumentation that automatically traces common operations like:

- HTTP requests (both incoming and outgoing)
- Database calls
- Message queue operations
- Popular framework operations

Start with auto-instrumentation - it gives you 80% of what you need with minimal effort.

### Custom Instrumentation

For business-specific operations, add custom spans around important code sections. This is where you'll trace things like:

- Complex business logic
- Internal service calls
- Custom integrations

### Sampling

Don't trace every single request - it's expensive and usually unnecessary. Use sampling to trace a representative subset:

- **Head-based sampling**: Decide at the start of a trace
- **Tail-based sampling**: Decide after seeing the complete trace (useful for keeping all error traces)

Start with a 10% sampling and adjust based on your traffic volume and needs.

### Span Naming

- Use descriptive, consistent names: `GET /users/{id}` not `GET /users/12345`
- Include the operation type: `db.query.select_user`
- Keep names stable - don't include dynamic values

## Revision History

---

- 2025-09-29: @Owner - Initial Release
