# Observability: Heartbeats, Readiness, and Liveness

|                    |                 |
| ------------------ | --------------- |
| **Content Owners** | @Owner |
| **Last Updated**   | 2025-09-29      |

---

## Other Resources

- [Kubernetes: Configure Liveness, Readiness and Startup Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

## What is a heartbeat?

---

Every service needs a way to tell us how it's doing. Each service also needs to document what can go wrong, set up alerts for those situations, and have clear steps to fix things when they break. We need to think about auto-healing, auto-scaling, and helping the ops team troubleshoot problems. This endpoint can also be used as a readiness and health endpoint in Kubernetes.

## How to Define the Heartbeat

---

Every service we create should expose a heartbeat endpoint. You can use the following OpenAPI spec to incorporate that into your service.

```yaml
openapi: 3.0.3
info:
  title: Service Heartbeat API
  description: Standard health check endpoint for all services
  version: 1.0.0
paths:
  /ops/heartbeat:
    get:
      summary: Get service health status
      description: Returns the current health status of the service and its dependencies
      responses:
        "200":
          description: Service health information
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/HealthResponse"
        "503":
          description: Service unavailable or critical health issue
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/HealthResponse"
components:
  schemas:
    HealthResponse:
      type: object
      required:
        - Status
        - URL
        - Machine
        - UtcDateTime
      properties:
        Status:
          type: string
          enum: [OK, WARNING, ERROR]
          description: Overall health status
        Message:
          type: string
          description: Optional details about current health condition
        URL:
          type: string
          format: uri
          description: The actual endpoint URL that responded
        Machine:
          type: string
          description: Hostname or container name (never IP address)
        UtcDateTime:
          type: string
          format: date-time
          description: ISO 8601 timestamp when response was generated
        RequestDuration:
          type: number
          minimum: 0
          description: How long the health check took (milliseconds)
        Dependencies:
          type: array
          description: Health status of direct dependencies
          items:
            $ref: "#/components/schemas/DependencyHealth"
    DependencyHealth:
      type: object
      required:
        - Status
        - URL
        - UtcDateTime
      properties:
        Status:
          type: string
          enum: [OK, WARNING, CRITICAL]
        URL:
          type: string
          format: uri
        UtcDateTime:
          type: string
          format: date-time
        RequestDuration:
          type: number
          minimum: 0
```

## Revision History

---

- 2025-09-29: @Owner - Initial Release
