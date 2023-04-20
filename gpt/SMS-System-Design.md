# Script Management System Design

## Problem Statement

The company currently lacks a centralized system to manage and control multiple scripts written in various languages on both Windows and Linux servers. There is a need for an efficient way to deploy, control, and monitor these scripts.

## Requirements

### Functional Requirements

1. Start/stop original scripts.
2. Add new scripts.

### Non-Functional Requirements

1. Support web and app interfaces.
2. Ensure scalability.
3. Prioritize security and availability.
4. Implement authorization and access control.
5. Monitor metrics of scripts.
6. Support both Linux and Windows old servers.

## How to Solve the Problem?

Design and implement a Script Management System (SMS) that centralizes the management and control of various scripts. The SMS will provide a unified way to deploy, control, and monitor scripts while ensuring scalability, security, and availability.

## System Design

### Tools, Frameworks, Libraries, and Programming Languages
- Frontend: React, Ant Design
- Backend: Node.js, Express.js
- Database: MariaDB
- Monitoring and Logging: Prometheus, Elasticsearch
- Authentication: Keycloak

### Architecture
A microservices-based architecture will be employed, with separate services for managing scripts, monitoring, and user authentication.

### Frontend Design

#### Language, Frameworks, and UI Libraries
- Language: JavaScript (ES6+)
- Framework: React
- UI Library: Ant Design

#### Wireframe Description and Components
The frontend will consist of the following components:

1. Dashboard: Displays an overview of scripts, their statuses, and metrics.
2. Scripts: Allows users to start/stop, and add new scripts.
3. Monitoring: Shows script metrics and logs.
4. Authentication: Handles user login and registration through Keycloak.
5. Authorization: Manages user roles and permissions.

Pros:
- React offers a component-based architecture, making the UI modular and easy to maintain.
- Ant Design provides a clean and modern design, speeding up development.

Cons:
- React has a steeper learning curve for new developers.

### Backend

#### Language, Frameworks, and Tools
- Language: JavaScript (Node.js)
- Framework: Express.js

#### Architecture
The backend will be designed using a microservices architecture, with the following services:

1. Script Management Service: Manages script deployment, starting, and stopping.
2. Monitoring Service: Monitors script metrics and logs using Prometheus and Elasticsearch.
3. Authentication Service: Handles user authentication and authorization through Keycloak.



### Database

#### Choice: MariaDB
MariaDB is chosen for its compatibility with the existing infrastructure and its ability to handle relational data effectively.

### Integration with Prometheus and Elasticsearch
The Monitoring Service will utilize Prometheus for metrics collection and Elasticsearch for logging. The metrics and logs will be available in the Monitoring section of the frontend.

### Integration with Keycloak
User authentication and authorization will be managed through Keycloak. The frontend will integrate with Keycloak for login and registration, and the backend services will use Keycloak for verifying user access.

### Script Language
Considering your company uses scripts in PowerShell, bat, and Korn shell, it's crucial to continue supporting these languages. However, for any new scripts, you might consider using Python, as it is a versatile, widely-used language with a rich ecosystem of libraries and frameworks.

## Timeline with Priority

1. Integrate backend services with Keycloak, Prometheus, and Elasticsearch (3 weeks).
2. Develop Script Management Service for deployment, starting, and stopping scripts (4 weeks).
3. Create frontend components and integrate them with backend services (3 weeks).
4. Implement security measures,

## microservices

---

Microservices Structure
Script Management Service
Monitoring Service
Each microservice is a standalone application responsible for a specific set of functionalities in the system.

Components and Functions
1. Script Management Service
This microservice is responsible for handling script deployment, starting, stopping, and updating.

Script Controller: Handles incoming HTTP requests from the API Gateway, processes them, and sends responses back to the API Gateway.
Script Service: Contains the business logic to manage scripts, such as deploying, starting, and stopping scripts on different VMs.
Script Repository: Interacts with the database to store and retrieve script metadata.
VM Manager: Manages the communication with VMs where the scripts are running. It sends commands to start or stop scripts and retrieves the script output.
Event Publisher: Publishes events to Kafka when a script is deployed, started, or stopped. These events can be consumed by other components, such as the Monitoring Service.
2. Monitoring Service
This microservice is responsible for monitoring script metrics and logs.

Monitoring Controller: Handles incoming HTTP requests from the API Gateway for script metrics and logs, processes them, and sends responses back to the API Gateway.
Metrics Service: Communicates with Prometheus to store and retrieve script metrics.
Logs Service: Communicates with Elasticsearch to store and retrieve script logs.
Event Consumer: Listens for events from Kafka, such as script deployment, starting, and stopping events. It updates the metrics and logs accordingly.
Kafka Integration
Kafka serves as the event queue in the system, allowing different components to communicate asynchronously. The Event Publisher component in the Script Management Service publishes events to Kafka, while the Event Consumer component in the Monitoring Service listens for and processes these events.

Scripts in Different VMs
Scripts are deployed and managed on different VMs, depending on their platform (Windows or Linux) and the requirements. The VM Manager component within the Script Management Service is responsible for communicating with these VMs. When a script needs to be started or stopped, the VM Manager sends a command to the appropriate VM, which executes the action. The output of the script is then sent back to the Script Management Service, which can update the script status, metrics, and logs accordingly.

This design ensures that the microservices are decoupled, allowing them to evolve independently while maintaining efficient communication through Kafka and the API Gateway.

## more

---

Protocols
Frontend to API Gateway: HTTPS (HTTP over TLS) for secure communication.
API Gateway to Microservices: HTTP or HTTPS, depending on the internal network security requirements.
Microservices to Kafka: Kafka protocol with SSL/TLS for secure communication.
Microservices to Database (MariaDB): MariaDB protocol with SSL/TLS for secure communication.
Microservices to Prometheus and Elasticsearch: HTTPS (HTTP over TLS) for secure communication.
VM Manager to VMs: SSH for secure command execution on VMs.
Ways to Reduce Monitoring Cost
Request Throttling: Limit the rate of monitoring requests from the frontend to avoid overloading the backend services.
Caching: Implement caching on the frontend and backend to store frequently accessed data and reduce the number of requests to Prometheus and Elasticsearch.
Data Aggregation: Aggregate monitoring data before sending it to the frontend. Instead of sending raw metrics and logs, send summarized data to reduce the amount of data transferred and processed.
Load Balancing: Use load balancing to distribute monitoring requests across multiple instances of the Monitoring Service, reducing the load on individual instances.
Optimize Queries: Optimize queries to Prometheus and Elasticsearch to retrieve only the necessary data and reduce the processing load.
Structure for High Availability
Microservices: Deploy multiple instances of each microservice and use a load balancer to distribute incoming requests evenly among them. This ensures that if one instance fails, the others can continue processing requests.
Kafka: Set up a Kafka cluster with multiple brokers and replication to ensure message durability and availability in case of broker failures.
Database: Use MariaDB with a master-slave or master-master replication setup to ensure data availability in case of failures.
Prometheus and Elasticsearch: Deploy Prometheus and Elasticsearch clusters with replication to ensure high availability of monitoring data.
API Gateway: Deploy multiple instances of the API Gateway behind a load balancer to distribute incoming requests and ensure availability in case of failures.
VMs: Use VM replication or backup strategies to ensure script availability in case of VM failures.
This design aims to provide a highly available and fault-tolerant system by utilizing load balancing, replication, and multiple instances of services.


