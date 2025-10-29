\# Blue/Green Deployment with Nginx



\## Overview

This setup runs two Node.js services (Blue and Green) behind Nginx with automatic failover and manual chaos testing.



\## How to Run



1\. Copy `.env.example` to `.env` and fill in your image names and release IDs.

2\. Run:

&nbsp;  ```bash

&nbsp;  docker-compose up

\- Access the service:

\- Main entry: http://localhost:8080/version

\- Blue direct: http://localhost:8081

\- Green direct: http://localhost:8082

\- Green direct: http://localhost:8082



Chaos Testing

To simulate failure on Blue:

curl -X POST http://localhost:8081/chaos/start?mode=error





To stop chaos:

curl -X POST http://localhost:8081/chaos/stop





Expected Behavior

\- All traffic goes to Blue by default.

\- On failure, Nginx retries to Green with no failed requests.

\- Headers X-App-Pool and X-Release-Id are preserved.





