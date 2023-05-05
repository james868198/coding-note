
# Solution: APM Timechart

last update: 2023/05/04

author: huangucy

## Outline

---

- [Introduction](#introduction)
- [Requirement](#requirement)
- [Data Model](#data-model)
- [Frontend](#frontend)
- [Backend](#backend)
- [Frontend](#frontend)
- [Table Schema](#table-schema)
- [References & Resources](#references--resources)

## introduction

---

APM Timechart is a web application designed to manage timecharts and their tasks. The application incorporates a timechart engine. The timechart engine is a component of the APM timechart web application that automates the management of task statuses. It operates as a finite state machine and performs the following tasks:

- Identifies tasks ready to execute based on their start dates.
- Handles tasks that are about to expire by sending notifications or taking appropriate actions.
- Manages expired tasks by updating their statuses.

By automating these processes, the timechart engine simplifies task management and enhances the efficiency of the APM timechart system.

### `pros:`

- Streamlined Timechart Management: Simplified creation, updating, and deletion of timecharts and tasks.
- Task Assignment and Collaboration: Easy task assignment and collaboration among team members.
- Automated Task Status Management: Automatic tracking of task statuses for timely updates.
- [Future] Can be integrated with task automation, allowing tasks to be automatically completed at specified times through timer integration, reducing manual intervention.
- [Future] Generates APM reports automatically, ensuring standardized formatting.

### `cons:`

- Implementation and Customization Effort: Tailoring the system to meet specific team requirements.
- Learning Curve: Initial familiarity required with the interface and functionalities.
- Maintenance and Updates: Ongoing effort for system maintenance and updates.

## Requirement:

---

### `Functional Requirement:`

Timechart Management:

- Search for timecharts.
- Create, update, and remove timecharts.
- Perform deep copy of timecharts.

Task Management:

- Search for tasks within timecharts.
- create, update, and remove tasks within timecharts.
- Perform deep copy of tasks within timecharts.

Timechart Engine:

- Show real-time progress of tasks within timecharts.
- Notify sponsors when tasks are ready to execute.
- Support automatic execution and checking of tasks.
- Allow users to update task status.
- Allow hosts to update timecharts.

History:
- Record user operations when updating timecharts, tasks, and task status.

### `Non-Functional Requirement:`

- Atomicity: Ensure that steps are executed as a single, indivisible operation.
- Performance:
- Simple manual execution should have a response time of less than 100ms.
- Auto operations like task checking or execution should be completed in less than 2 minutes.
- Usability: The system should have a user-friendly interface and be easy to operate and manage.
- Extensibility: The system should support integration with other tools and services and allow for component changes.
- Reliability: The system should be highly available and resilient to failures, particularly during APM.
- Flexibility: The system should accommodate changing APM requirements and future features beyond task checking and execution.
- Scalability: The system should handle an increasing number of APM tasks and store history data efficiently.
- Security: Implement robust access control measures to ensure system security.

## Data Model

---

### general

```json
// Products (only administrator can add them)
{
  "id": "product name", // Product name is the ID, not an auto-generated ID
  "description": "description",
  "created_at": "created_at",
  "updated_at": "updated_at"
}

// Notification for personal
{
  "id": "notification id",
  "user_id": "user id",
  "message": "message content",
  "created_at": "created_at"
}

// Notification for timechart engine
{
  "id": "notification id",
  "timechart_id": "timechart id",
  "user_id": "user id",
  "message": "message content",
  "created_at": "created_at"
}

```

### timechart

```json

// Timechart
{
  "id": "unique id",
  "name": "name",
  "description": "description",
  "start_time": "start time",
  "end_time": "end time",
  "created_at": "created_at",
  "updated_at": "updated_at",
  "state": "[Todo, Ready, In Progress, Evaluating, Completed]",
  "lock_by": "user id (the person who is editing)"
}

// Timechart Owner
{
  "task_id": "task id",
  "owner": "owner user id",
  "created_at": "created_at"
}

// Timechart Comment (optional)
{
  "comment_id": "unique id",
  "timechart_id": "timechart id",
  "author": "user id",
  "content": "text",
  "created_at": "created_at",
  "updated_at": "updated_at"
}

```

### Task

```json
// Task
{
  "id": "unique id",
  "timechart_id": "timechart_id",
  "name": "name",
  "description": "description",
  "planned_start_time": "planned start time",
  "planned_end_time": "planned end time",
  "start_time": "start time",
  "end_time": "end time",
  "created_at": "created_at",
  "updated_at": "created_at",
  "state": "[Todo, Ready, In Progress, Executed, Checked, Completed]",
  "product": "product",
  "lock_by": "user id (the person who is editing)"
}

// Task Pointer
{
  "from_id": "task id",
  "to_id": "task id"
}

// Task Owner
{
  "task_id": "task id",
  "user": "user_id",
  "created_at": "created_at"
}

// Action
{
  "id": "action id (its name)", // Follow naming rules
  "description": "action description",
  "service_url": "service API URL"
}

// Task Action
{
  "task_id": "task id",
  "action": "action_id",
  "action_data": "parameters for the service (can be a JSON string)"
}

// Task Comment (optional)
{
  "comment_id": "unique id",
  "timechart_id": "timechart id",
  "author": "user id",
  "content": "comment content"
}

```

### history

```json

// History
{
  "id": "unique id",
  "user_id": "user id",
  "item_type": "item type [TIMECHART, TASK]",
  "item_id": "item id",
  "operation": "change operation description written by the rule", // e.g., [action][resource][sub resource]
  "input_data": "string or blob of the data that was changed",
  "created_at": "created_at"
}

```

## Frontend

---

### `Pages:`

- Home/Dashboard: This is a page that provides an overview of upcoming timecharts, tasks, and notifications. It serves as the landing page after logging in.
- Timechart List: This page displays a list of all timecharts, allowing users to browse and select a specific timechart to view or edit.
- Calendar: This page provides a calendar view, similar to Google Calendar, where users can see the schedule and events of their timecharts.
- Timechart (Gantt chart view): This page displays a Gantt chart/list view representation of a selected timechart, showing the tasks and their durations over time.

### `Modals:`

- Timechart Details (modal): This modal displays detailed information about a selected timechart, including its title, description, and other relevant details.
- Task Details (modal): This modal shows detailed information about a selected task, including its title, description, assigned users, and status.
- Task Creation/Update (modal): This modal allows users to create or update tasks within a timechart. Users can input task details, assign users, and set other task properties.
- Reports (modal): This modal allows users to generate APM reports. Users can customize report parameters, such as time range or specific timecharts, and view or download the generated reports.

## Backend

---

The backend architecture consists of separate services that communicate with each other through APIs and possibly a message queue. The Timechart Service and Timechart Engine Service work together to manage timecharts and tasks, while the Notification Service handles the delivery of notifications. The Authentication Service ensures secure access to the APIs, and the Message Queue Service allows for asynchronous communication between services.

### Timechart Service:

This service handles the CRUD (Create, Read, Update, Delete) operations related to timecharts and tasks. It provides APIs for creating, updating, and retrieving timechart and task information. It interacts with the database to store and retrieve data.

### Timechart Engine Service:

This service is responsible for managing the execution and status of tasks within timecharts. It includes an in-process scheduler that periodically checks the status of tasks and performs actions based on predefined rules. The Timechart Engine Service interacts with the Timechart Service to retrieve timechart and task information, and it may also send notifications to users through the Notification Service.

### Notification Service:

The Notification Service handles the sending and delivery of notifications to users. It can receive notifications from other services and send them via various channels such as email, SMS, or push notifications. The Notification Service can also store notifications for users, allowing them to view notifications at their convenience.

### Authentication Service:

If you are using an existing Keycloak service for authentication, it can be integrated into your architecture as the Authentication Service. It handles user authentication, authorization, and user management. The Authentication Service ensures that only authorized users can access the Timechart and Task APIs.

### Message Queue Service (e.g., NATS): 

If needed, a message queue service like NATS can be incorporated to facilitate asynchronous communication between services. It enables services to publish and subscribe to events, enabling loose coupling and scalability in the system.

## Table Schema

---

```sql
-- Products
CREATE TABLE products (
  id VARCHAR(255) PRIMARY KEY,
  description TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

-- Personal Notifications
CREATE TABLE personal_notifications (
  id INT PRIMARY KEY AUTO_INCREMENT,
  user_id VARCHAR(255),
  message TEXT,
  created_at TIMESTAMP
);

-- Timechart Engine Notifications
CREATE TABLE timechart_engine_notifications (
  id INT PRIMARY KEY AUTO_INCREMENT,
  timechart_id VARCHAR(255),
  user_id VARCHAR(255),
  message TEXT,
  created_at TIMESTAMP
);

-- Timechart
CREATE TABLE timecharts (
  id VARCHAR(255) PRIMARY KEY,
  name VARCHAR(255),
  description TEXT,
  start_time TIMESTAMP,
  end_time TIMESTAMP,
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
  state VARCHAR(20),
  lock_by VARCHAR(255)
);

-- Timechart Owners
CREATE TABLE timechart_owners (
  task_id VARCHAR(255),
  owner VARCHAR(255),
  created_at TIMESTAMP,
  PRIMARY KEY (task_id, owner)
);

-- Timechart Comments
CREATE TABLE timechart_comments (
  comment_id INT PRIMARY KEY AUTO_INCREMENT,
  timechart_id VARCHAR(255),
  author VARCHAR(255),
  content TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

-- Tasks
CREATE TABLE tasks (
  id VARCHAR(255) PRIMARY KEY,
  timechart_id VARCHAR(255),
  name VARCHAR(255),
  description TEXT,
  planned_start_time TIMESTAMP,
  planned_end_time TIMESTAMP,
  start_time TIMESTAMP,
  end_time TIMESTAMP,
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
  state VARCHAR(20),
  product VARCHAR(255),
  lock_by VARCHAR(255)
);

-- Task Pointers
CREATE TABLE task_pointers (
  from_id VARCHAR(255),
  to_id VARCHAR(255),
  PRIMARY KEY (from_id, to_id)
);

-- Task Owners
CREATE TABLE task_owners (
  task_id VARCHAR(255),
  user VARCHAR(255),
  created_at TIMESTAMP,
  PRIMARY KEY (task_id, user)
);

-- Actions
CREATE TABLE actions (
  id VARCHAR(255) PRIMARY KEY,
  description TEXT,
  service_url VARCHAR(255)
);

-- Task Actions
CREATE TABLE task_actions (
  task_id VARCHAR(255),
  action VARCHAR(255),
  action_data TEXT,
  PRIMARY KEY (task_id, action)
);

-- Task Comments
CREATE TABLE task_comments (
  comment_id INT PRIMARY KEY AUTO_INCREMENT,
  timechart_id VARCHAR(255),
  author VARCHAR(255),
  content TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);

-- History
CREATE TABLE history (
  id INT PRIMARY KEY AUTO_INCREMENT,
  user_id VARCHAR(255),
  item_type VARCHAR(50),
  item_id VARCHAR(255),
  operation VARCHAR(255),
  input_data TEXT,
  created_at TIMESTAMP
);

-- Indexes
CREATE INDEX idx_timechart_id ON timechart_owners (timechart_id);
CREATE INDEX idx_task_id ON task_owners (task_id);
CREATE INDEX idx_from_id ON task_pointers (from_id);
CREATE INDEX idx_to_id ON task_pointers (to_id);
CREATE INDEX idx_timechart_id_comments ON timechart_comments (timechart_id);
CREATE INDEX idx_timechart_id_task_comments ON task_comments (timechart_id);
CREATE INDEX idx_item_id_history ON history (item_id);

-- Constraints
ALTER TABLE timechart_owners
  ADD FOREIGN KEY (task_id) REFERENCES tasks (id)
  ON DELETE CASCADE;

ALTER TABLE timechart_comments
  ADD FOREIGN KEY (timechart_id) REFERENCES timecharts (id)
  ON DELETE CASCADE;

ALTER TABLE tasks
  ADD FOREIGN KEY (timechart_id) REFERENCES timecharts (id)
  ON DELETE CASCADE;

ALTER TABLE task_pointers
  ADD FOREIGN KEY (from_id) REFERENCES tasks (id)
  ON DELETE CASCADE,
  ADD FOREIGN KEY (to_id) REFERENCES tasks (id)
  ON DELETE CASCADE;

ALTER TABLE task_owners
  ADD FOREIGN KEY (task_id) REFERENCES tasks (id)
  ON DELETE CASCADE;

ALTER TABLE task_actions
  ADD FOREIGN KEY (task_id) REFERENCES tasks (id)
  ON DELETE CASCADE,
  ADD FOREIGN KEY (action) REFERENCES actions (id)
  ON DELETE CASCADE;

ALTER TABLE task_comments
  ADD FOREIGN KEY (timechart_id) REFERENCES timecharts (id)
  ON DELETE CASCADE;

ALTER TABLE history
  ON DELETE CASCADE,
  ADD FOREIGN KEY (item_type) REFERENCES items (type)
  ON DELETE CASCADE,
  ADD FOREIGN KEY (item_id) REFERENCES items (id)
  ON DELETE CASCADE;
```

## References & Resources

---

- [gantt-task-react](https://github.com/MaTeMaTuK/gantt-task-react)
- [dhtmlx](https://dhtmlx.com/docs/products/licenses.shtml)