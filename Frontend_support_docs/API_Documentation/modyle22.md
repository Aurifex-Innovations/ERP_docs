# 📋 API Documentation

---

## Module 22 – Fleet & Live Tracking

### Table of Contents

- [22.1 Fleet Tracking Dashboard](#221-fleet-tracking-dashboard)
- [22.2 Technician Logistics & Travel Log](#222-technician-logistics--travel-log)

---

### 22.1 Fleet Tracking Dashboard

| Method | Endpoint                          | Purpose                                              |
| ------ | --------------------------------- | ---------------------------------------------------- |
| `GET`  | `/api/v1/live-tracking/dashboard` | Fetch live or historical fleet tracking data by date |
| `GET`  | `/api/v1/live-tracking/routes`    | Fetch map routes (polylines) for a specific date     |

---

### 22.2 Technician Logistics & Travel Log

> Supports **daily**, **weekly**, and **monthly** log views.

| Method | Endpoint                                                |
| ------ | ------------------------------------------------------- |
| `GET`  | `/api/v1/live-tracking/technicians/{userId}/travel-log` |

---

_Last updated: April 2026_
