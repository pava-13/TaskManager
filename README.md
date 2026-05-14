# TaskManager
🚀 TaskFlow — Full-Stack Task Manager

A modern team task management system built with React, Node.js, and SQLite featuring:

🔐 Role-Based Access Control
📊 Analytics Dashboard
✅ Task Assignment & Tracking
🔎 Real-Time Filtering
📝 Activity Audit Logs
⚡ REST API Architecture


📁 Project Structure
taskflow/
│
├── frontend/                 # React Frontend
│   └── src/
│       └── App.jsx
│
├── backend/                  # Node.js + SQLite Backend
│   ├── server.js
│   ├── package.json
│   └── taskflow.db
│
└── README.md


🛠️ Tech Stack
Frontend
React.js
JavaScript
CSS / Tailwind (optional)
Backend
Node.js
Express.js
SQLite (better-sqlite3)
JWT Authentication
bcrypt Password Hashing


🗄️ Database Schema

Users Table

CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  name TEXT NOT NULL,
  email TEXT NOT NULL UNIQUE,
  password TEXT NOT NULL,
  role TEXT NOT NULL DEFAULT 'member'
       CHECK(role IN ('leader','member')),
  avatar TEXT,
  color TEXT DEFAULT '#6366f1',
  created_at TEXT NOT NULL DEFAULT (datetime('now'))
);


Tasks Table

CREATE TABLE tasks (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  title TEXT NOT NULL,
  description TEXT,
  assignee_id INTEGER REFERENCES users(id) ON DELETE SET NULL,
  created_by INTEGER REFERENCES users(id) ON DELETE SET NULL,
  deadline TEXT,
  priority TEXT NOT NULL DEFAULT 'medium'
           CHECK(priority IN ('critical','high','medium','low')),
  status TEXT NOT NULL DEFAULT 'todo'
         CHECK(status IN ('todo','in-progress','completed')),
  category TEXT NOT NULL DEFAULT 'Engineering',
  created_at TEXT NOT NULL DEFAULT (datetime('now')),
  updated_at TEXT NOT NULL DEFAULT (datetime('now'))
);


Activity Log Table

CREATE TABLE activity_log (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER REFERENCES users(id),
  task_id INTEGER REFERENCES tasks(id) ON DELETE CASCADE,
  action TEXT NOT NULL,
  detail TEXT,
  created_at TEXT NOT NULL DEFAULT (datetime('now'))
);


⚙️ Installation & Setup

1️⃣ Backend Setup

cd backend
npm install
Start Server
node server.js

OR using nodemon:

npm run dev
Backend runs on:
http://localhost:3001

The SQLite database (taskflow.db) is automatically created with seed data during first run.

2️⃣ Frontend Setup

npx create-react-app frontend
cd frontend

Replace:

src/App.js

with:

TaskManager.jsx

Start React app:

npm start
Frontend runs on:
http://localhost:3000


🔗 Connecting Frontend to Backend

Replace the in-memory DB object inside TaskManager.jsx with API calls using fetch().

Example — Login API
const res = await fetch("http://localhost:3001/api/auth/login", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify({
    email: "alex@team.io",
    password: "leader123"
  })
});

const { token, user } = await res.json();

localStorage.setItem("token", token);
Example — Fetch Tasks
const token = localStorage.getItem("token");

const res = await fetch(
  "http://localhost:3001/api/tasks?status=todo",
  {
    headers: {
      Authorization: `Bearer ${token}`
    }
  }
);

const tasks = await res.json();


🔐 Authentication
All protected routes require:
Authorization: Bearer <jwt_token> 


📡 API Endpoints
Method	Endpoint	Access	Description

POST	/api/auth/login	Public	Login & get JWT

GET	/api/auth/me	Any	Get current user

GET	/api/users	Any	List users

POST	/api/users	Leader	Create user

DELETE	/api/users/:id	Leader	Delete user

GET	/api/tasks	Any	Get tasks

POST	/api/tasks	Leader	Create task

PATCH	/api/tasks/:id	Any	Update task

DELETE	/api/tasks/:id	Leader	Delete task

GET	/api/analytics/summary	Leader	Full analytics

GET	/api/analytics/my	Any	Personal analytics

GET	/api/activity	Leader	Activity logs


👥 Role Permissions
Feature	Team Leader	Team Member
View all tasks	✅	❌
Create tasks	✅	❌
Assign tasks	✅	❌
Edit tasks	✅	Status only
Delete tasks	✅	❌
View analytics	✅	Personal only
Manage users	✅	❌
🧪 Demo Accounts
Role	Email	Password
Team Leader	alex@team.io	leader123
Team Member	jordan@team.io	member123
Team Member	sam@team.io	member456
Team Member	morgan@team.io	member789
📈 Features
✅ Secure JWT Authentication
✅ Role-Based Access
✅ Task CRUD Operations
✅ Task Status Tracking
✅ Deadline Management
✅ Priority Levels
✅ Analytics Dashboard
✅ Activity Audit Logs
✅ Responsive UI
✅ Real-Time Filtering


🗃️ Migration to PostgreSQL
To migrate from SQLite → PostgreSQL:
Replace
better-sqlite3
with
pg
Update Queries
SQLite
db.prepare(sql).get(...)
PostgreSQL
await pool.query(sql, params)
Schema Changes
SQLite	PostgreSQL
INTEGER PRIMARY KEY AUTOINCREMENT	SERIAL PRIMARY KEY
datetime('now')	NOW()
PRAGMA foreign_keys = ON	Not required

🚀 Future Improvements
WebSocket Real-Time Updates
Notifications System
File Attachments
Dark Mode
Kanban Drag & Drop
Email Reminders
Team Chat Integration

📄 License
This project is licensed under the MIT License.

👨‍💻 Author

Developed with ❤️ using React + Node.js + SQLite.
