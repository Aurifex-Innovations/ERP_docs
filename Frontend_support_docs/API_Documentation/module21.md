# 📋 API Documentation

---

## Module 21 – Task Management

### Table of Contents

- [21.1 Task Calendar Dashboard](#211-task-calendar-dashboard)
- [21.2 Daily Task View](#212-daily-task-view)
- [21.3 Add / Create Task](#213-add--create-task)
- [21.4 View Task Detail](#214-view-task-detail)
- [21.5 Edit Task](#215-edit-task)
- [21.6 Task Completion & Material Log](#216-task-completion--material-log)
- [21.7 Reschedule / Reassign Task](#217-reschedule--reassign-task)

---

### 21.1 Task Calendar Dashboard

| Method | Endpoint                 |
| ------ | ------------------------ |
| `GET`  | `/api/v1/tasks/calendar` |

---

### 21.2 Daily Task View

| Method | Endpoint        |
| ------ | --------------- |
| `GET`  | `/api/v1/tasks` |

---

### 21.3 Add / Create Task

#### Tab 1 – From Sales Order

| Method | Endpoint                             | Purpose                                                          |
| ------ | ------------------------------------ | ---------------------------------------------------------------- |
| `POST` | `/api/v1/tasks/multiple`             | Create multiple tasks _(primary)_                                |
| `POST` | `/api/v1/tasks`                      | Create single task _(optional — not required per documentation)_ |
| `GET`  | `/api/v1/company/branches/dropdown`  | Branch dropdown                                                  |
| `GET`  | `/api/v1/sales-orders/dropdown`      | Sales order dropdown                                             |
| `GET`  | `/api/v1/tasks/available-technician` | Available technician list for task assignment                    |

#### Tab 2 – From Customer Tickets (Re-Task)

| Method | Endpoint                           | Purpose                             |
| ------ | ---------------------------------- | ----------------------------------- |
| `POST` | `/api/v1/tasks`                    | Create re-task from customer ticket |
| `GET`  | `/api/v1/support/tickets/dropdown` | Ticket support dropdown             |

---

### 21.4 View Task Detail

| Method | Endpoint                   | Purpose            |
| ------ | -------------------------- | ------------------ |
| `GET`  | `/api/v1/tasks`            | Fetch task details |
| `GET`  | `/api/v1/tasks/export-pdf` | Download task PDF  |

---

### 21.5 Edit Task

| Method | Endpoint                             | Purpose                                       |
| ------ | ------------------------------------ | --------------------------------------------- |
| `PUT`  | `/api/v1/tasks`                      | Update task details                           |
| `GET`  | `/api/v1/company/branches/dropdown`  | Branch dropdown                               |
| `GET`  | `/api/v1/sales-orders/dropdown`      | Sales order dropdown                          |
| `GET`  | `/api/v1/tasks/available-technician` | Available technician list for task assignment |

---

### 21.6 Task Completion & Material Log

| Method | Endpoint                   | Purpose                       |
| ------ | -------------------------- | ----------------------------- |
| `GET`  | `/api/v1/tasks`            | Fetch task completion details |
| `GET`  | `/api/v1/tasks/export-pdf` | Download task PDF             |

---

### 21.7 Reschedule / Reassign Task

| Method | Endpoint                             | Purpose                                       |
| ------ | ------------------------------------ | --------------------------------------------- |
| `POST` | `/api/v1/tasks/reschedule`           | Reschedule task to a new time                 |
| `POST` | `/api/v1/tasks/reassign`             | Reassign task to a different technician       |
| `GET`  | `/api/v1/tasks/available-technician` | Available technician list for task assignment |

---

_Last updated: April 2026_
