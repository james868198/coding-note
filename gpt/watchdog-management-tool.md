# Watchdog Management System

last update: 4/20/2023

## Overview

---

This document describes the design of a Watchdog Management System that allows managing and monitoring daemon/watchdog processes running on multiple servers/VMs. The system provides a centralized user interface for starting, stopping, and checking the status of the watchdogs.

## System Architecture

---

The system architecture consists of the following components:

1. [Frontend UI](#frontend-ui)
2. [Backend API Server (UserRequestHandler)](#backend-api-server-userrequesthandler)
3. [Backend API Server (HeartbeatHandler)](#backend-api-server-userrequesthandler)
4. [NATS Messaging System](#nats-messaging-system)
5. [MariaDB Database](#mariadb-database)
6. [Load Balancer (optional)](#load-balancer-optional)

### Frontend UI

The frontend UI is a web-based interface built using React and Ant Design. It communicates with the Backend API Server (UserRequestHandler) to perform operations like starting, stopping, and checking the status of the watchdog processes.

The UI can be organized into multiple pages or views, such as:

- Dashboard: Displays an overview of the daemons, including their current status, recent events, and any alerts.
- Daemon List: Displays a list of all daemons, allowing users to start, stop, and view details for each daemon.
- Daemon Details: Displays detailed information about a specific daemon, including its status history, configuration, and logs.
- User Management: Allows administrators to manage user accounts, access control, and permissions.

### Backend API Server (UserRequestHandler)

The UserRequestHandler server is responsible for handling user requests from the frontend UI. It manages tasks like starting/stopping daemons, fetching daemon status, and updating configurations. The frontend UI communicates with this server using RESTful API calls.

The server can be implemented using Node.js and Express, with various API endpoints for different actions:

- GET /daemons: Retrieve a list of all daemons and their status
- POST /daemons/:id/start: Start a daemon with the specified ID
- POST /daemons/:id/stop: Stop a daemon with the specified ID
- GET /daemons/:id: Retrieve detailed information about a specific daemon

### Backend API Server (HeartbeatHandler)

The HeartbeatHandler server is responsible for processing heartbeat messages from the daemons. It listens to NATS subjects for heartbeat messages and updates the daemon status in the database accordingly. NATS queue groups can be used to load balance message processing across multiple instances of this server if needed.

The server can be implemented using Node.js and a NATS client library, with the following key components:

- NATS connection: Connects to the NATS messaging system
- Heartbeat subscription: Subscribes to a NATS subject for receiving heartbeat messages from daemons
- Message processing: Processes incoming heartbeat messages and updates the daemon status in the MariaDB database

### NATS Messaging System

NATS is a high-performance messaging system used for communication between the daemons and the HeartbeatHandler server. Daemons send heartbeat messages to the HeartbeatHandler server through NATS, which then processes these messages and updates the daemon status in the database.

Daemons should be configured to send heartbeat messages to a specific NATS subject, with a unique identifier for each daemon included in the message payload. The HeartbeatHandler server should subscribe to this subject and process the messages accordingly.

### MariaDB Database

MariaDB is used to store information about the daemons, including their status and change history. The schema should include tables for storing daemon information, status updates, and user actions (start/stop).

Example schema:

- `daemons`: Stores information about each daemon, including its unique identifier, description, and configuration details.

  - id: Unique identifier for the daemon
  - name: Descriptive name for the daemon
  - description: A brief description of the daemon's purpose

details for the daemon

- `status_updates`: Stores status updates received from daemons via heartbeat messages.
  - `id`: Unique identifier for the status update
  - `daemon_id`: Foreign key referencing the associated daemon
  - `status`: Current status of the daemon (e.g., running, stopped, error)
  - `timestamp`: Timestamp of when the status update was received

- `user_actions`: Stores a record of user actions, such as starting and stopping daemons.
  - `id`: Unique identifier for the user action
  - `user_id`: Foreign key referencing the user who performed the action
  - `daemon_id`: Foreign key referencing the associated daemon
  - `action`: The action performed (e.g., start, stop)
  - `timestamp`: Timestamp of when the action was performed

### Load Balancer (optional)

A load balancer can be used to distribute incoming traffic to multiple instances of the Backend API Server (UserRequestHandler) and Backend API Server (HeartbeatHandler) to ensure high availability and better performance. 

An example load balancing solution for this system could be using Nginx. Nginx can be configured to act as a reverse proxy for the Backend API Servers, distributing incoming requests between multiple instances of the servers based on their current load or other criteria.

## Transaction Flow

---

1. The frontend UI sends user requests (start/stop/check status) to the Backend API Server (UserRequestHandler) via RESTful API calls.
2. The UserRequestHandler server processes the requests, updating the MariaDB database as needed, and sends commands to the corresponding daemons via NATS messages.
3. Daemons receive the commands via NATS, perform the requested action, and send periodic heartbeat messages to the Backend API Server (HeartbeatHandler) via NATS.
4. The HeartbeatHandler server processes the heartbeat messages, updating the daemon status in the MariaDB database accordingly.
5. The frontend UI periodically polls the Backend API Server (UserRequestHandler) for updated daemon status, displaying the latest information to the user.

## Implementation Plan

---

The following steps outline the implementation plan for the Watchdog Management System:

1. Design and develop the Frontend UI using React and Ant Design.
2. Implement the Backend API Server (UserRequestHandler) using Node.js and Express.
3. Implement the Backend API Server (HeartbeatHandler) using Node.js and a NATS client library.
4. Set up and configure the NATS messaging system.
5. Set up and configure the MariaDB database, including schema design.
6. (Optional) Set up and configure a load balancer using Nginx.
7. Test and deploy the system to the production environment.

By following this plan, the Watchdog Management System can be efficiently built and deployed, providing a centralized and user-friendly solution for managing and monitoring daemon/watchdog processes across multiple servers and VMs.
