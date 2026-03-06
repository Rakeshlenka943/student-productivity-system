# System Architecture - Student Productivity & Attendance Management System

## Overview

This document describes the complete architecture of the Student Productivity & Attendance Management System, including system design, component interactions, data flow, and deployment strategy.

## 1. High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     Frontend (React + Vite)                      │
│  ┌─────────────────────────────────────────────────────────────┐│
│  │  Dashboard │ Calendar │ Timetable │ Analytics │ Study Planner││
│  └─────────────────────────────────────────────────────────────┘│
└──────────────────────────────────────────────────────────────────┘
                            │
                    (REST API + WebSocket)
                            │
        ┌───────────────────┴───────────────────┐
        │                                       │
┌───────▼──────────────────┐        ┌──────────▼─────────────┐
│  Backend (Spring Boot)    │        │  WebSocket Server      │
│  ┌──────────────────────┐ │        │  ┌──────────────────┐  │
│  │ Controllers          │ │        │  │ Notifications    │  │
│  │ Services             │ │        │  │ Real-time Events │  │
│  │ Repositories         │ │        │  │ Attendance Sync  │  │
│  │ JWT Auth             │ │        │  └──────────────────┘  │
│  └──────────────────────┘ │        └────────────────────────┘
└───────┬──────────────────┘
        │
        │ (JDBC)
        │
┌───────▼──────────────────┐
│   PostgreSQL Database    │
│  ┌──────────────────────┐│
│  │ Users                ││
│  │ Timetable Entries    ││
│  │ Attendance Records   ││
│  │ Events               ││
│  │ Exams                ││
│  │ Study Tasks          ││
│  │ Notifications        ││
│  └──────────────────────┘│
└──────────────────────────┘
```

## 2. Component Architecture

### 2.1 Frontend Architecture

```
src/
├── components/
│   ├── Dashboard/
│   │   ├── Dashboard.jsx
│   │   ├── StatsCard.jsx
│   │   └── QuickStats.jsx
│   ├── Calendar/
│   │   ├── Calendar.jsx
│   │   ├── CalendarDay.jsx
│   │   └── EventModal.jsx
│   ├── Timetable/
│   │   ├── Timetable.jsx
│   │   ├── TimeSlot.jsx
│   │   ├── ClassBlock.jsx
│   │   └── DragDropContext.jsx
│   ├── Attendance/
│   │   ├── AttendanceView.jsx
│   │   ├── AttendanceCell.jsx
│   │   └── AttendanceHistory.jsx
│   ├── Analytics/
│   │   ├── AnalyticsDashboard.jsx
│   │   ├── AttendancePieChart.jsx
│   │   ├── SubjectBarChart.jsx
│   │   └── TrendLineChart.jsx
│   ├── StudyPlanner/
│   │   ├── StudyPlanner.jsx
│   │   ├── TaskList.jsx
│   │   └── TaskForm.jsx
│   ├── ExamManager/
│   │   ├── ExamManager.jsx
│   │   ├── ExamCard.jsx
│   │   ├── ExamForm.jsx
│   │   └── CountdownTimer.jsx
│   ├── Notifications/
│   │   ├── NotificationCenter.jsx
│   │   ├── NotificationItem.jsx
│   │   └── NotificationPreferences.jsx
│   ├── Auth/
│   │   ├── Login.jsx
│   │   ├── Register.jsx
│   │   └── AuthGuard.jsx
│   ├── Layout/
│   │   ├── Header.jsx
│   │   ├── Sidebar.jsx
│   │   └── Layout.jsx
│   └── Common/
│       ├── Button.jsx
│       ├── Modal.jsx
│       ├── Loader.jsx
│       └── Toast.jsx
├── pages/
│   ├── DashboardPage.jsx
│   ├── CalendarPage.jsx
│   ├── TimetablePage.jsx
│   ├── AnalyticsPage.jsx
│   ├── StudyPlannerPage.jsx
│   ├── ExamManagerPage.jsx
│   ├── LoginPage.jsx
│   └── ProfilePage.jsx
├── services/
│   ├── api.js (Axios instance)
│   ├── authService.js
│   ├── timetableService.js
│   ├── attendanceService.js
│   ├── eventService.js
│   ├── examService.js
│   ├── taskService.js
│   ├── analyticsService.js
│   ├── notificationService.js
│   └── wsService.js (WebSocket)
├── hooks/
│   ├── useAuth.js
│   ├── useNotifications.js
│   ├── useTimetable.js
│   ├── useAttendance.js
│   └── useAsync.js
├── utils/
│   ├── constants.js
│   ├── helpers.js
│   ├── dateUtils.js
│   ├── colorUtils.js
│   └── validators.js
├── styles/
│   ├── global.css
│   ├── variables.css
│   ├── animations.css
│   └── responsive.css
└── main.jsx
```

### 2.2 Backend Architecture

```
src/main/java/com/productivity/
├── StudentProductivityApplication.java
├── config/
│   ├── JwtConfig.java
│   ├── SecurityConfig.java
│   ├── CorsConfig.java
│   ├── WebSocketConfig.java
│   └── AppConfig.java
├── controllers/
│   ├── AuthController.java
│   ├── UserController.java
│   ├── TimetableController.java
│   ├── AttendanceController.java
│   ├── EventController.java
│   ├── ExamController.java
│   ├── TaskController.java
│   ├── NotificationController.java
│   └── AnalyticsController.java
├── services/
│   ├── AuthService.java
│   ├── UserService.java
│   ├── TimetableService.java
│   ├── AttendanceService.java
│   ├── EventService.java
│   ├── ExamService.java
│   ├── TaskService.java
│   ├── NotificationService.java
│   └── AnalyticsService.java
├── repositories/
│   ├── UserRepository.java
│   ├── TimetableRepository.java
│   ├── AttendanceRepository.java
│   ├── EventRepository.java
│   ├── ExamRepository.java
│   ├── TaskRepository.java
│   ├── NotificationRepository.java
│   └── NotificationPreferenceRepository.java
├── models/
│   ├── User.java
│   ├── TimetableEntry.java
│   ├── AttendanceRecord.java
│   ├── Event.java
│   ├── Exam.java
│   ├── StudyTask.java
│   ├── Notification.java
│   └── NotificationPreference.java
├── dto/
│   ├── AuthRequest.java
│   ├── AuthResponse.java
│   ├── UserDTO.java
│   ├── TimetableDTO.java
│   ├── AttendanceDTO.java
│   ├── EventDTO.java
│   ├── ExamDTO.java
│   ├── TaskDTO.java
│   └── NotificationDTO.java
├── websocket/
│   ├── WebSocketHandler.java
│   ├── NotificationWebSocketHandler.java
│   └── WebSocketMessage.java
├── security/
│   ├── JwtTokenProvider.java
│   ├── JwtAuthFilter.java
│   └── UserDetailsServiceImpl.java
├── exception/
│   ├── GlobalExceptionHandler.java
│   ├── ResourceNotFoundException.java
│   ├── AuthenticationException.java
│   └── ValidationException.java
├── util/
│   ├── DateTimeUtils.java
│   ├── ValidationUtils.java
│   └── Constants.java
└── resources/
    └── application.properties
```

## 3. Data Flow Diagrams

### 3.1 User Registration & Login Flow

```
User Input
    │
    ▼
RegisterRequest/LoginRequest
    │
    ▼
AuthController
    │
    ▼
AuthService
    │
    ├─► Password Validation/Hashing
    │
    ▼
UserRepository (Save/Query)
    │
    ▼
PostgreSQL
    │
    ▼
JWT Token Generation
    │
    ▼
AuthResponse
    │
    ▼
Frontend (Store Token)
```

### 3.2 Timetable & Attendance Flow

```
User Creates/Updates Timetable
    │
    ▼
TimetableController.createEntry()
    │
    ▼
TimetableService.saveEntry()
    │
    ▼
TimetableRepository.save()
    │
    ▼
PostgreSQL: INSERT timetable_entries
    │
    ▼
WebSocket Broadcast: Timetable Updated
    │
    ▼
All Connected Clients Notified
    │
    ▼
Frontend Updates Real-time
    │
    ▼
User Marks Attendance
    │
    ▼
AttendanceController.markAttendance()
    │
    ▼
AttendanceService.recordAttendance()
    │
    ▼
AttendanceRepository.save()
    │
    ▼
PostgreSQL: INSERT attendance_records
    │
    ▼
WebSocket: Attendance Changed Alert
    │
    ▼
Analytics Recalculated
```

### 3.3 Real-time Notification Flow

```
Event Triggered (Exam Date, Class Reminder, etc)
    │
    ▼
NotificationService.createNotification()
    │
    ▼
NotificationRepository.save()
    │
    ▼
WebSocketHandler.sendNotification()
    │
    ▼
Connected Client Socket
    │
    ▼
Frontend Receives Notification
    │
    ▼
Toast/Alert Displayed
    │
    ▼
NotificationCenter Updated
```

## 4. Database Schema Design

### Entity Relationships

```
┌─────────────┐
│    User     │ (1)
└──────┬──────┘
       │
       ├─────────────────────┬──────────────┬──────────────┐
       │                     │              │              │
    (N)│ (N)              (N)│ (N)       (N)│          (N)│
       │                     │              │              │
┌──────▼────────┐  ┌─────────▼────┐  ┌────▼────────┐  ┌──▼──────────┐
│   Events      │  │  Timetable   │  │   Exams     │  │ StudyTasks │
│               │  │   Entries    │  │             │  │             │
└───────────────┘  └──────┬───────┘  └─────────────┘  └─────────────┘
                          │ (1)
                          │
                       (N)│
                          │
                   ┌──────▼──────────┐
                   │ Attendance      │
                   │ Records         │
                   └─────────────────┘

┌──────────────┐
│ Notification │◄──(1-to-N)── User
│              │
└──────────────┘

┌──────────────────────┐
│ Notification         │◄──(1-to-1)── User
│ Preferences          │
└──────────────────────┘
```

## 5. API Endpoints

### Authentication
- POST /api/auth/register
- POST /api/auth/login
- POST /api/auth/logout
- POST /api/auth/refresh-token

### User Management
- GET /api/users/profile
- PUT /api/users/profile
- GET /api/users/{id}

### Timetable
- GET /api/timetable
- POST /api/timetable
- PUT /api/timetable/{id}
- DELETE /api/timetable/{id}
- GET /api/timetable/week/{weekStart}

### Attendance
- GET /api/attendance
- POST /api/attendance
- PUT /api/attendance/{id}
- GET /api/attendance/statistics
- GET /api/attendance/subject/{subjectId}

### Events
- GET /api/events
- POST /api/events
- PUT /api/events/{id}
- DELETE /api/events/{id}
- GET /api/events/month/{year}/{month}

### Exams
- GET /api/exams
- POST /api/exams
- PUT /api/exams/{id}
- DELETE /api/exams/{id}
- GET /api/exams/upcoming

### Study Tasks
- GET /api/tasks
- POST /api/tasks
- PUT /api/tasks/{id}
- DELETE /api/tasks/{id}
- PUT /api/tasks/{id}/complete

### Notifications
- GET /api/notifications
- PUT /api/notifications/{id}/read
- DELETE /api/notifications/{id}
- GET /api/notifications/preferences
- PUT /api/notifications/preferences

### Analytics
- GET /api/analytics/attendance-summary
- GET /api/analytics/subject-attendance
- GET /api/analytics/attendance-trend
- GET /api/analytics/safe-absence
- GET /api/analytics/export-pdf

## 6. Security Architecture

### Authentication & Authorization

1. **JWT Token-based Authentication**
   - User logs in → receives JWT token
   - Token stored in localStorage/sessionStorage (frontend)
   - Token sent in Authorization header for each request
   - Token includes user ID and role

2. **Password Security**
   - Passwords hashed using BCrypt
   - Minimum 8 characters, mixed case, numbers, special chars
   - Never stored in plain text

3. **CORS & CSRF Protection**
   - CORS configured for frontend origin
   - CSRF tokens for state-changing operations

4. **WebSocket Security**
   - WebSocket handshake includes JWT token
   - Per-connection authentication
   - Message validation before processing

## 7. Scalability Considerations

1. **Database Optimization**
   - Indexed queries for fast lookups
   - Connection pooling (HikariCP)
   - Query optimization and caching

2. **Caching Strategy**
   - Redis for user session cache (optional)
   - Browser cache for static assets
   - Service worker for offline caching

3. **Load Distribution**
   - Stateless backend design
   - Horizontal scaling capability
   - Load balancer ready

4. **Real-time Features**
   - WebSocket connection pooling
   - Message broadcasting optimization
   - Connection cleanup on disconnect

## 8. Deployment Architecture

### Development Environment
- Local PostgreSQL
- Spring Boot development server (port 8080)
- React dev server (port 5173)

### Production Environment
- Docker containers for isolation
- PostgreSQL on managed service or self-hosted
- Spring Boot on dedicated server/container
- React static build served via Nginx
- SSL/TLS encryption
- Environment variables for configuration

## 9. Technology Rationale

| Component | Choice | Reason |
|-----------|--------|--------|
| Backend | Spring Boot | Enterprise-grade, mature, extensive ecosystem |
| Frontend | React | High performance, component reusability, large community |
| Database | PostgreSQL | Open-source, reliable, advanced features, free |
| Auth | JWT | Stateless, scalable, industry standard |
| Real-time | WebSocket | Low latency, persistent connection, real-time updates |
| PWA | Service Worker | Offline capability, installability, no app store |

## 10. Performance Metrics & Targets

- API Response Time: < 200ms
- WebSocket Message Delivery: < 100ms
- Database Query Time: < 50ms
- Frontend Load Time: < 3s
- Concurrent Users: 10,000+
- Uptime: 99.5%

---

**Last Updated:** 2026-03-06