# Raksha Bandhu
### Fire Safety & Emergency Response Infrastructure Platform

Raksha Bandhu is a fire safety and emergency response platform developed for **Avishkar: Maharashtra State Inter-University Research Convention**.

The platform digitizes communication between citizens and fire department authorities through Fire NOC management, inspection scheduling, emergency alerting, and fire safety awareness.

The application was extended with a **multi-container Docker architecture** that separates frontend delivery from backend services, enabling independent service execution, container health monitoring, network isolation, and streamlined deployment using Docker Compose.

## Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Key Features](#key-features)
- [Technology Stack](#technology-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Container Management](#container-management)
- [Health Monitoring](#health-monitoring)
- [Resource Optimization](#resource-optimization)
- [Emergency Response Workflow](#emergency-response-workflow)
- [Security Considerations](#security-considerations)
- [Future Improvements](#future-improvements)
- [Team](#team)
- [License](#license)

---

## Overview

Raksha Bandhu combines public-facing fire safety services with a containerized application infrastructure.

Citizens can access fire safety resources, submit Fire NOC applications, request inspections, and use emergency contact and SOS features.

Fire department authorities can use the administrative interface to manage relevant application workflows.

From an infrastructure perspective, the application separates the frontend, NOC backend, and inspection backend into individual containers. Docker Compose coordinates these services, while a dedicated Docker network supports communication between application components.

### Project Objectives

- Digitize fire safety and Fire NOC workflows.
- Improve access to emergency contacts and fire safety information.
- Support inspection request management.
- Provide an emergency alerting interface.
- Apply containerization and service separation.
- Improve deployment consistency through Docker Compose.
- Explore container health monitoring and resource management.

## Architecture

The application uses a multi-container architecture consisting of an NGINX frontend and two Node.js backend services.

### Infrastructure Diagram

```text
                     +----------------------+
                     |       Browser        |
                     |   User Interaction   |
                     +----------+-----------+
                                |
                                v
                     +----------------------+
                     |   Frontend Service   |
                     |        NGINX         |
                     |   Host Port: 8080    |
                     +----------+-----------+
                                |
                  +-------------+-------------+
                  |                           |
                  v                           v
       +----------------------+    +----------------------+
       |    NOC Backend API   |    |  Inspection Backend  |
       |      Node.js         |    |      Node.js         |
       |     Express.js       |    |     Express.js       |
       |     Port: 5000       |    |     Port: 5001       |
       +----------------------+    +----------------------+
                  |                           |
                  +-------------+-------------+
                                |
                     +----------------------+
                     |    Docker Network    |
                     |   raksha-network     |
                     +----------------------+
```

### Service Responsibilities

| Service | Technology | Responsibility |
|---|---|---|
| Frontend | HTML, CSS, JavaScript, NGINX | Serves the web interface |
| NOC Backend | Node.js, Express.js | Handles Fire NOC-related API operations |
| Inspection Backend | Node.js, Express.js | Handles fire safety inspection workflows |
| Docker Compose | Docker | Coordinates application containers |
| Docker Network | Bridge networking | Provides network connectivity between services |

The exact routing behavior depends on the NGINX configuration and the API endpoints implemented by the application.

## Key Features

### Fire Safety Application

- Online Fire NOC application submission and tracking.
- Fire safety inspection request management.
- Emergency contacts directory.
- Fire safety awareness and educational resources.
- SOS emergency alert interface.
- WhatsApp-based emergency alert initiation.
- Administrative interface for fire department workflows.
- GPS-based incident location sharing.

### DevOps and Infrastructure

- Multi-container Docker architecture.
- Docker Compose-based orchestration.
- Dedicated Docker bridge network.
- Separate frontend and backend containers.
- Container health checks.
- Entrypoint initialization scripts.
- Lightweight Alpine-based container images.
- Non-root container execution where configured.
- Runtime resource monitoring using Docker Stats.
- Independent backend services for NOC and inspection workflows.

## Technology Stack

### Frontend

- HTML5
- CSS3
- JavaScript
- Tailwind CSS
- Font Awesome
- NGINX

### Backend

- Node.js
- Express.js
- REST APIs

### DevOps and Infrastructure

- Docker
- Docker Compose
- Alpine Linux
- Docker Bridge Networking
- Container Health Checks
- Linux Shell Scripting
- NGINX Configuration

### External Integrations

- WhatsApp click-to-chat links
- Browser Geolocation API
- Google Maps location links

The availability and behavior of these integrations depend on browser permissions, application configuration, and the relevant external services.

## Project Structure

```text
Fire-Department-Software/
|
|-- backend/
|   Backend service for Fire NOC workflows
|
|-- inspectionBackend/
|   Backend service for inspection workflows
|
|-- docker/
|   |
|   |-- nginx/
|   |   |-- frontend.conf
|   |
|   |-- scripts/
|       |-- startup.sh
|
|-- Dockerfile.backend
|-- Dockerfile.inspection
|-- Dockerfile.frontend
|
|-- docker-compose.yml
|-- .dockerignore
|
|-- index.html
|-- adminPage.html
|-- contactsPage.html
|-- inspectionPage.html
|-- nocPage.html
|-- safety_tips.html
|
|-- README.md
```

## Getting Started

Follow the steps below to run the application locally using Docker Compose.

### Prerequisites

Install the following tools:

- Git
- Docker Engine or Docker Desktop
- Docker Compose

Verify your Docker installation:

```bash
docker --version
docker compose version
```

### 1. Clone the Repository

```bash
git clone https://github.com/Atharavkumkar/Fire-Department-Software.git

cd Fire-Department-Software
```

### 2. Build and Start the Services

Run:

```bash
docker compose up --build
```

This command builds the configured images and starts the services defined in `docker-compose.yml`.

To start the containers in detached mode:

```bash
docker compose up --build -d
```

### 3. Access the Application

Once the containers are running, use the following endpoints.

| Service | URL |
|---|---|
| Frontend | http://localhost:8080 |
| NOC Backend | http://localhost:5000 |
| Inspection Backend | http://localhost:5001 |

The backend URLs are direct host-access endpoints if the corresponding ports are published in Docker Compose. Actual API paths depend on the implemented routes.

## Container Management

Docker Compose simplifies the management of the application's services.

### View Running Containers

```bash
docker ps
```

### View All Containers

```bash
docker ps -a
```

### View Container Logs

```bash
docker compose logs
```

View logs for a specific service:

```bash
docker compose logs -f
```

To follow the logs of an individual service, replace `<service-name>` with the corresponding service name from `docker-compose.yml`:

```bash
docker compose logs -f <service-name>
```

### Monitor Resource Usage

```bash
docker stats
```

This command displays real-time container resource usage, including CPU, memory, network I/O, and block I/O.

### Inspect the Docker Network

First, identify the actual network name:

```bash
docker network ls
```

Then inspect it:

```bash
docker network inspect fire-department-software_raksha-network
```

The network name may differ depending on the Compose project name and configuration.

### Stop the Application

Stop the running containers:

```bash
docker compose stop
```

Stop and remove the containers and Compose-created network:

```bash
docker compose down
```

To rebuild and restart the application after making changes:

```bash
docker compose up --build -d
```

## Health Monitoring

The infrastructure includes Docker health monitoring for application containers where health checks are configured.

Health checks can help identify whether a containerized service is responding as expected.

The relevant configuration is defined through Dockerfile health-check instructions or the `healthcheck` section in `docker-compose.yml`.

### Inspect Container Health

```bash
docker ps
```

For more detailed container information:

```bash
docker inspect <container-name>
```

Check the health status in the command output when a health check is configured.

**Important:** Docker health checks report the result of the configured test. They do not automatically guarantee application correctness or end-to-end service availability.

## Resource Optimization

The project uses lightweight container images and separates application components into individual services.

### Optimization Techniques

- **Lightweight Base Images:** Alpine-based images help reduce image size where compatible.
- **Service Separation:** Frontend, NOC, and inspection services run independently.
- **Resource Monitoring:** Docker Stats provides visibility into runtime resource consumption.
- **Container Initialization:** Startup scripts can prepare the environment before the application starts.
- **Non-Root Execution:** Running containers as non-root users reduces unnecessary privileges where configured.

### Resource Usage

Actual resource consumption depends on the container image, application workload, runtime configuration, and host environment.

Although lightweight containers can have a small memory footprint, resource usage should be measured under realistic workloads rather than assumed from the base image alone.

## Emergency Response Workflow

The SOS workflow is designed to help users initiate an emergency alert with location information.

### Workflow

1. The user activates the SOS feature.
2. The application initiates the configured emergency countdown.
3. The browser requests location information when required.
4. The application prepares an incident message containing the available coordinates.
5. A WhatsApp click-to-chat link can be opened with the prepared message.
6. The message can include a Google Maps location link.
7. Incident information may be recorded locally, depending on the implementation.

### Location and Alerting Considerations

Browser geolocation generally requires user permission and a secure context, such as HTTPS or localhost.

A WhatsApp click-to-chat link opens a prepared conversation or message. It does not, by itself, guarantee automatic message delivery.

This project should not be treated as a replacement for official emergency services or a verified emergency dispatch system.

## Security Considerations

The project incorporates several infrastructure practices intended to improve deployment security.

### Container Isolation

Docker provides separate container environments for the frontend and backend services.

### Network Separation

A dedicated bridge network provides connectivity between the application's containers.

### Non-Root Execution

Where configured, running application processes as non-root users reduces the privileges available inside containers.

### Additional Recommendations

For production deployment, consider:

- Avoiding unnecessary exposure of backend ports to the public internet.
- Using HTTPS for externally accessible services.
- Validating and sanitizing all API inputs.
- Storing sensitive configuration outside source control.
- Applying least-privilege permissions.
- Keeping container images and dependencies updated.
- Implementing authentication and authorization for administrative operations.
- Restricting access to emergency and location information.
- Adding centralized monitoring and alerting.
- Testing recovery behavior when a backend service becomes unavailable.

These recommendations should be evaluated against the current implementation before the application is deployed in a real emergency-response environment.

## Future Improvements

Potential enhancements include:

- Adding automated CI/CD workflows for container builds.
- Introducing vulnerability scanning for Docker images.
- Adding automated API and integration tests.
- Publishing versioned container images to a container registry.
- Introducing infrastructure deployment automation.
- Adding structured application logging.
- Implementing centralized monitoring and alerting.
- Improving backend health checks and failure recovery.
- Adding automated dependency and configuration security checks.
- Deploying the platform to a cloud environment with HTTPS and controlled access.

These are potential improvements rather than claims about existing functionality.

## Team

Developed by:

- **Atharva Kumkar**
- **Abha Naktode**
- **Riya Wagh**
- **Riddhi Totala**

Developed for **Avishkar: Maharashtra State Inter-University Research Convention**.

## License

This project was developed as part of an academic research and innovation initiative.

All rights are reserved by the authors unless otherwise specified.

For reuse, distribution, or modification, contact the project authors for permission.
