# CampTH frontend + backend source

## What's included
- `frontend/index.html`: the current CampTH single-file frontend (HTML, CSS and JavaScript).
- `backend/server.js`: Node.js + Express API with SQLite persistence, password hashing, JWT login, role permissions, and role-based complaint escalation.

## Run the backend
1. Install Node.js 20+.
2. Open a terminal in `backend/`.
3. Run `npm install`.
4. Copy `.env.example` to `.env` and set a long random `JWT_SECRET` (at least 24 characters).
5. Run `npm start`.
6. Check `http://localhost:3000/api/health`.

Demo API login accounts (change these before deployment):
- `student / 1234`
- `teacher / 1234`
- `admin / 1234`
- `warden / 1234`
- `chief_warden / 1234`
- `hostel_manager / 1234`
- `mess_staff / 1234`

## API endpoints
- `POST /api/auth/login` — `{ "username":"student", "password":"1234" }`
- `GET /api/auth/me` — Bearer token required
- `POST /api/complaints` — create hostel or academic complaint (student/teacher)
- `GET /api/complaints` — returns complaints visible to the signed-in user
- `POST /api/complaints/:id/escalate` — current assignee escalates to next role
- `POST /api/complaints/:id/resolve` — current assignee resolves complaint

Hostel complaint route: Warden → Chief Warden → Hostel Manager → Admin.
Academic complaint route: selected teacher → Admin / Academic Coordinator review.

## Important integration note
The supplied frontend file is the existing CampTH UI and currently retains its browser-local demo login/complaint storage. The backend is a separately runnable API source; it is not yet wired into every frontend page. To use real multi-user data, the frontend login and complaint functions must call these `/api` endpoints with the JWT Bearer token. The API is a starter backend, not a production deployment: add HTTPS, rate limiting, password reset, account administration, audit retention, input validation, backups, and real staff/student accounts before public use.
