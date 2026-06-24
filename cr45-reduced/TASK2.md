# The Next Level

Yes, we are going to continue working on the Dockerization repo. You have already containerized the given full-stack application (I hope) using Docker Compose! It is now time to take it to the next level—preparing the application for a production environment.

This task will challenge your understanding of reverse proxies, Docker image optimization, horizontal scalability, and CI/CD automation—all essential for running modern cloud-native applications in production.


## Task 2 Requirements

### 1. Reverse Proxy with Nginx and HTTPS

Set up an Nginx container that:

- Serves the React frontend at the root path (`/`)
- Proxies API requests (`/api`) to the Rust backend
- Enforces HTTPS using self-signed SSL certificates and HSTS
- Manages CORS headers properly
- Enables gzip compression for performance
- Add the nginx service in docker-compose

### 2. Jenkins CI/CD Pipeline

Automate your development workflow with Jenkins:

- Run linters and tests for both the frontend and backend on every push and pull request
- Build and push Docker images for all services to Docker Hub on every push to the `main` branch
- Automatically deploy updated services using Docker Compose upon successful image builds. Utilize the Docker Hub for image fetching rather than rebuilding it on the deployment.

### 3. Production Optimization

Review and make changes to your setup, if necessary, that optimize your application and ensures production-ready best practices, including:

- Docker Image & Container Optimization
- Nginx Performance Tuning
- CI/CD Pipeline Enhancements
- Security Best Practices in Docker, Nginx, Jenkins as well
- Observability & Resiliency in Docker and Nginx

## More Biriyani? Hell yeah!!

### 4. Horizontal Scaling

Production applications should remain available even when individual instances fail. Scale your backend service and validate that your deployment can handle failures gracefully.

- Run at least 3 backend replicas using Docker Compose
- Configure Nginx load balancing between backend instances
- Verify requests are distributed across all replicas
- Demonstrate application availability after stopping one backend instance
- Document your load-balancing and failover strategy
- Include screenshots or recordings showing traffic distribution and failure recovery

### 5. Performance Testing & Analysis

Evaluate the performance characteristics of your deployment and compare the impact of horizontal scaling.

- Use a load testing tool such as k6, ApacheBench (ab), JMeter, or wrk
- Benchmark both a single-backend deployment and a multi-replica deployment
- Measure requests per second (RPS), latency, and error rates
- Test with multiple concurrency levels and document the results
- Compare performance between the two deployment configurations
- Identify potential bottlenecks in the application or infrastructure
- Include test scripts, graphs, and a short analysis report

### 6. Security Exploration

Explore production container security practices and apply them where practical.

- Run containers as non-root users where possible
- Investigate read-only container filesystems
- Explore Linux capability restrictions
- Scan Docker images for vulnerabilities using tools such as Trivy, Grype, or Docker Scout
- Document identified risks and applied mitigations
- Include scan reports and a brief summary of findings


## Submission Guidelines

- Use the same GitHub repository from Task 1 to push all updated code and configuration files.
    - Update the root level `DOCS.md` explaining the setup, architecture, and workflow with screenshots. (A screen recording would be helpful as well, upload it to Drive/YouTube)
    - It is important that whatever you do is documented in `DOCS.md` and supported by the files used (for example, the Jenkinsfile for 2, the load balancing config for 4, the load testing script for 5, and so on).
- Share the GitHub repository link as your final submission
- **1, 2, 3 must be fully implemented** for the task to be considered completion
- More attention will be given to thoughtful inclusions and modifications in 3 - 6, especially those following industry best practices. *Attention is all you need*

## Resources (do not confine yourself to this list!)

### 1. Nginx

- https://nginx.org/en/docs/
- https://www.youtube.com/watch?v=iInUBOVeBCc
- https://medium.com/@mathur.danduprolu/securing-your-web-server-with-nginx-https-and-best-practices-part-5-7-99ad19bf5b1f

### 2. Jenkins

- https://www.youtube.com/watch?v=6YZvp2GwT0A
- https://medium.com/@prateek.malhotra004/essential-jenkins-best-practices-for-developers-streamline-your-ci-cd-process-dc3aa38f9928
- https://www.jenkins.io/doc/book/pipeline/pipeline-best-practices/

### 3. Docker

- https://snyk.io/blog/10-docker-image-security-best-practices/
- https://sysdig.com/learn-cloud-native/dockerfile-best-practices/
- https://docs.docker.com/docker-hub/

### Ingredients for Biriyani

- Load balancing: https://nginx.org/en/docs/http/load_balancing.html
- Load testing: https://grafana.com/docs/k6/latest/
- Security: https://trivy.dev/docs/

*You are free to use any alternatives to the ones listed here. The biriyani must ultimately taste good.*