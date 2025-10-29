\# Design Decisions – DevOps Stage 2



\##  Failover Strategy

Used Nginx’s `backup` directive to designate Green as the standby instance. Configured `max\_fails=1` and `fail\_timeout=3s` to detect failure quickly and trigger failover.



\##  Retry Logic

Enabled `proxy\_next\_upstream` for error, timeout, and 5xx responses. Limited retries to 2 to ensure fast fallback without long delays.



\##  Header Forwarding

Used `proxy\_pass\_request\_headers on` to ensure app-level headers (`X-App-Pool`, `X-Release-Id`) are preserved and forwarded to clients.



\##  Config Templating

Used `envsubst` in `entrypoint.sh` to inject environment variables into the Nginx config dynamically. This allows switching between Blue and Green pools without modifying the config manually.



\##  Docker Compose

All services (Nginx, Blue, Green) are orchestrated via Docker Compose. Ports are mapped to allow direct access for chaos testing and CI grading.



\##  Chaos Testing

The grader can trigger downtime on Blue via `/chaos/start`, and Nginx will automatically route traffic to Green with zero failed requests.



\##  Environment Variables

All configuration is parameterized via `.env`, including image names, release IDs, and active pool. This makes the setup flexible and CI-friendly.

