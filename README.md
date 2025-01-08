# Distributed Notification and Alert System

## Objective
Build a backend system to manage and deliver notifications based on user preferences and rules. This includes immediate notifications, scheduled alerts, and personalized delivery times, as well as support for query-driven notification logic.

---

## Task Requirements

### 1. Notification Ingestion and Validation Service (Node.js / Go)
- **Endpoint**: Create a `/notify` API endpoint in Node.js to receive notification requests.
- **Data Handling**: Each notification request should gather basic notification-related details like time, message, etc.
- **Kafka Integration**: Publish validated requests to a Kafka topic (`notifications`) for further processing.

### 2. Notification Processing and Scheduling Engine (Node.js / Go)
- **Real-Time Notifications**: Implement logic to immediately process high-priority notifications or notifications without a `send_time`.
- **Scheduled Notifications**: Store notification details in MongoDB and implement a scheduler to periodically check for pending notifications.

### 3. User Preferences and Rules Storage (MongoDB + Elasticsearch)
- **User Preferences**:
  - Store user preferences in MongoDB, such as:
    - Preferred notification channels (e.g., email, SMS, push).
    - Quiet hours (e.g., Do Not Disturb between 10 PM and 8 AM).
    - Notification limits (e.g., max 3 notifications per hour).
- **Content Filtering**:
  - Store notification content in Elasticsearch with fields like message, type, priority, send_time, and user_id to enable efficient search queries.

### 4. Query Requirements
- **Query 1: Throttling Notifications**
  - Restrict notifications based on user-defined limits.
  - Use MongoDB to check recent notifications sent to a user, ensuring the count does not exceed the hourly limit.
  - If the limit is reached, either discard or defer the notification.

- **Query 2: Quiet Hours Filtering**
  - Detect user-defined quiet hours, preventing notifications from being sent within these hours.
  - Reschedule notifications for the user’s next active hour.

- **Query 3: Deduplication of Similar Alerts**
  - Deduplicate alerts if a similar one has been sent recently (e.g., suppress identical error notifications sent within the last 1 hour).

- **Query 4: Scheduled Notification Aggregation**
  - Aggregate low-priority notifications due in the same hour into a single summary message.
  - Retrieve all low-priority notifications from MongoDB scheduled within the same hour and create a summary before sending.

- **Query 5: Urgent Alerts in Real Time**
  - Prioritize urgent alerts and process them immediately, bypassing scheduling.
  - Use MongoDB’s priority field to query high-priority items and move them to the delivery queue.

### 5. Notification Delivery Service (Node.js)
- **Channels**:
  - Implement functionality to send notifications via:
    - Email: Use a mock email service or API.
    - SMS: Integrate with a mock SMS service.
    - Push Notifications: Simulate push notifications.
- **Retry Mechanism**: Implement a retry mechanism for failures.
- **Logging**: Record delivery statuses in MongoDB.

### 6. Containerization and Deployment
- **Docker Setup**: Dockerize each component (Node.js / Go services, MongoDB, Kafka, Elasticsearch).
- **Docker Compose**: Create a `docker-compose.yml` file for easy deployment.

### 7. Bonus: Analytics and Monitoring Dashboard API
- **Endpoint**: Implement an endpoint (`/analytics`) to provide:
  - Delivery Stats: Total notifications sent, failed, and retried.
  - User Engagement: Average delivery time and response rate.

---

## Submissions

- **GitHub Repository**: A project repository on GitHub.
- **README.md**: This file should include a description of your implementation, the design choices you made, and any known issues present at the time of submission.
- **FutureScope.md**: This file should outline future enhancements you believe would improve the project.
- **Scale.md**: This file should include your perspective on scaling the project for high-volume deployment. Describe the changes you would make to meet scaling demands and explain the rationale behind each change.
