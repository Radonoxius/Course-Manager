# Course Management System Backend API

A production-ready RESTful API for managing academic entities such as students, professors, departments, courses, semesters, enrollments, and grades.

Built using **Node.js**, **Express.js**, and **PostgreSQL** with scalable modular architecture.

---

## Overview

This project provides a complete backend system for a university or college course management platform.

It supports:

- Department management
- Student management
- Professor management
- Course management
- Semester tracking
- Student enrollments
- Grade assignment
- Health monitoring
- JWT-based route protection
- Structured error handling
- API documentation with Swagger

---

## Tech Stack

| Layer | Technology |
|---|---|
| Runtime | Node.js |
| Framework | Express.js |
| Database | MySQL |
| Authentication | JWT (jsonwebtoken) |
| Security | Helmet, CORS |
| Logging | Morgan |
| Documentation | Swagger UI |
| Environment | dotenv |

---

## Project Structure

```text
course-management-system/
├── src/
│   ├── config/
│   │   ├── db.js
│   │   └── env.js
│   │
│   ├── modules/
│   │   ├── department/
│   │   ├── professor/
│   │   ├── student/
│   │   ├── course/
│   │   ├── semester/
│   │   ├── enrollment/
│   │   └── grade/
│   │
│   ├── middlewares/
│   │   ├── auth.middleware.js
│   │   └── error.middleware.js
│   │
│   ├── utils/
│   │   ├── apiResponse.js
│   │   └── asyncHandler.js
│   │
│   ├── docs/
│   │   └── swagger.js
│   │
│   ├── app.js
│   └── server.js
│
├── database/
│   ├── schema.sql
│   └── seed.sql
│
├── docs/
│   └── api-docs.md
│
├── .env
├── package.json
└── README.md