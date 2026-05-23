# Gisul — Course Management Platform

A fullstack course management platform where admins create and manage courses, and students can enroll, track progress, and complete lessons.

---

## 🚀 Deployment

### Production Deployment: Vercel (Frontend) + Render (Backend)

**Start here:** [VERCEL_RENDER_DEPLOYMENT.md](VERCEL_RENDER_DEPLOYMENT.md)

- **Frontend:** Deployed on Vercel (`*.vercel.app`)
- **Backend:** Deployed on Render (`*.onrender.com`)
- **Database:** SQLite (auto-seeded with demo data)
- **Auto-Deploy:** Push to GitHub, both services redeploy automatically

---

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | React 19, React Router v7, Axios, CSS Variables |
| Backend | Python, FastAPI, Uvicorn |
| Database | SQLite (file-based) |
| ORM | SQLAlchemy 2.0 |
| Auth | JWT + bcrypt |
| Deployment | Vercel + Render |

---

## ✨ Features

### Core Features
- **Authentication** — Register & login for Admin and Student roles using JWT + bcrypt
- **Course Management (Admin)** — Create, edit, delete courses with title, description, category, thumbnail, and lessons
- **Course Catalog (Student)** — Browse courses, full detail pages with all lessons
- **Enrollment** — Students enroll in courses with one click, view enrolled courses on dashboard
- **Search & Filter** — Search courses by title, filter by category

### Additional Features
- **Role-Based Access Control** — Admin-only routes, student-only actions
- **Protected Routes** — Frontend and backend validation
- **Dark Theme** — Custom CSS with CSS Variables
- **Auto-Seeding** — Demo data loads on first run
- **CORS Enabled** — Frontend and backend communication

---

## 📁 Project Structure

```
gisul-platform/
├── frontend/                   # React application
│   ├── src/
│   │   ├── components/        # Navbar, CourseCard
│   │   ├── pages/             # Login, Register, Catalog, Dashboards
│   │   ├── context/           # AuthContext
│   │   ├── utils/             # api.js (Axios with interceptors)
│   │   └── App.js
│   ├── vercel.json
│   └── .env.production        # Backend URL for production
│
├── backend-fastapi/           # FastAPI backend
│   ├── app/
│   │   ├── models.py          # User, Course, Lesson, Enrollment
│   │   ├── schemas.py         # Pydantic models
│   │   ├── auth.py            # JWT + bcrypt functions
│   │   ├── database.py        # SQLAlchemy setup
│   │   ├── seed.py            # Demo data
│   │   └── routes/            # API endpoints
│   ├── main.py
│   └── requirements.txt
│
├── render.yaml                # Render backend deployment
├── vercel.json                # Vercel frontend deployment
├── VERCEL_RENDER_DEPLOYMENT.md # Deployment guide
└── README.md
```

---

## 🏃 Local Development

### Prerequisites
- Python 3.11+
- Node.js 16+
- Git

### Backend Setup
```bash
cd backend-fastapi
pip install -r requirements.txt
python main.py
# Backend runs on http://localhost:5000
# API docs at http://localhost:5000/docs
```

### Frontend Setup
```bash
cd frontend
npm install
npm start
# Frontend runs on http://localhost:3000
# Uses backend at http://localhost:5000
```

---

## 🔑 Demo Credentials

```
ADMIN
Email:    admin@gisul.com
Password: admin123

STUDENT
Email:    student@gisul.com
Password: student123
```

---

## 📚 API Endpoints

### Authentication
- `POST /api/auth/register` — Create new account
- `POST /api/auth/login` — Get JWT token
- `GET /api/auth/me` — Get current user

### Courses
- `GET /api/courses/` — List all courses (search/filter)
- `GET /api/courses/{id}` — Get course details with lessons
- `POST /api/courses/` — Create course (admin only)
- `PUT /api/courses/{id}` — Update course (admin only)
- `DELETE /api/courses/{id}` — Delete course (admin only)

### Enrollments
- `GET /api/enrollments/my` — Get student's enrolled courses
- `POST /api/enrollments/` — Enroll in a course
- `DELETE /api/enrollments/{id}` — Unenroll from course

---

## 🌍 Deployment Instructions

### Option 1: Vercel + Render (Recommended)
**→ [See Full Guide](VERCEL_RENDER_DEPLOYMENT.md)**

**Part 1:** Deploy backend on Render (10 min)
**Part 2:** Deploy frontend on Vercel (5 min)
**Total:** ~20 minutes, fully automated after that

### Quick Summary:
1. Push code to GitHub
2. Go to render.com → Create web service → Select repo → Deploy ✅
3. Go to vercel.com → Import project → Add Render URL → Deploy ✅
4. Both services auto-redeploy on every GitHub push

---

## 🔧 Environment Variables

### Frontend (.env.production)
```
REACT_APP_API_URL=https://your-render-backend.onrender.com
```

### Backend (Render Settings)
```
SQLALCHEMY_DATABASE_URL=sqlite:///./gisul.db
SECRET_KEY=your-secret-key-here
ALGORITHM=HS256
ACCESS_TOKEN_EXPIRE_MINUTES=10080
PORT=8000
```

---

## 📝 API Documentation

Once deployed, visit:
- `https://your-backend.onrender.com/docs` — Swagger UI with all endpoints
- `https://your-backend.onrender.com/redoc` — ReDoc alternative

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/your-feature`
3. Commit changes: `git commit -m "Add feature"`
4. Push to branch: `git push origin feature/your-feature`
5. Open a Pull Request

---

## 📄 License

MIT License - feel free to use for your projects!

---

## 🙋 Support

- **Issue:** Open a GitHub issue
- **Question:** Check [VERCEL_RENDER_DEPLOYMENT.md](VERCEL_RENDER_DEPLOYMENT.md) for help
- **Docs:** FastAPI - https://fastapi.tiangolo.com/ | React - https://react.dev/
cd gisul-platform
```

### 2. Set up the Backend

```bash
cd backend
cp .env.example .env
# Edit .env if needed (default values work for local dev)
npm install
npm start
```

The backend runs on **http://localhost:5000**. The SQLite database is auto-created and seeded with demo data on first run.

### 3. Set up the Frontend

Open a new terminal:

```bash
cd frontend
cp .env.example .env
# Ensure REACT_APP_API_URL=http://localhost:5000/api
npm install
npm start
```

The frontend runs on **http://localhost:3000**.

---

## Environment Variables

### Backend (`backend/.env`)

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT` | `5000` | Express server port |
| `JWT_SECRET` | `gisul-jwt-secret-2024` | Secret for signing JWTs — **change in production** |
| `FRONTEND_URL` | `http://localhost:3000` | Allowed CORS origin |

### Frontend (`frontend/.env`)

| Variable | Default | Description |
|----------|---------|-------------|
| `REACT_APP_API_URL` | `http://localhost:5000/api` | Backend API base URL |

---

## Demo Credentials

The database is automatically seeded on first run with these accounts:

| Role | Email | Password |
|------|-------|----------|
| **Admin** | admin@gisul.com | admin123 |
| **Student** | student@gisul.com | student123 |

Demo data includes **5 courses** across 5 categories (Technology, Data Science, Design, Business, Marketing), each with 4–5 lessons. The demo student is pre-enrolled in 2 courses.

---

## API Endpoints

### Auth
| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/register` | Register new user |
| POST | `/api/auth/login` | Login |
| GET | `/api/auth/me` | Get current user |

### Courses
| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/api/courses` | Public | List all courses (supports `?search=&category=`) |
| GET | `/api/courses/:id` | Public | Get course + lessons |
| POST | `/api/courses` | Admin | Create course |
| PUT | `/api/courses/:id` | Admin | Update course |
| DELETE | `/api/courses/:id` | Admin | Delete course |

### Enrollments
| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| GET | `/api/enrollments/my` | Student | Get enrolled courses with progress |
| POST | `/api/enrollments/:courseId` | Student | Enroll in course |
| GET | `/api/enrollments/:courseId/status` | Student | Check enrollment |
| GET | `/api/enrollments/:courseId/progress` | Student | Get completed lesson IDs |
| POST | `/api/enrollments/lessons/:lessonId/complete` | Student | Mark lesson done |
| DELETE | `/api/enrollments/lessons/:lessonId/complete` | Student | Unmark lesson |

---

## Deployment

### Backend (Render)

1. Create a new **Web Service** on [render.com](https://render.com)
2. Connect your GitHub repo, set root to `backend/`
3. Build command: `npm install`
4. Start command: `node server.js`
5. Add environment variables: `JWT_SECRET`, `FRONTEND_URL` (your Vercel URL)

### Frontend (Vercel)

1. Create a new project on [vercel.com](https://vercel.com)
2. Set root directory to `frontend/`
3. Add environment variable: `REACT_APP_API_URL` = your Render backend URL + `/api`
4. Deploy

---

## Known Limitations

- The SQLite database is file-based and stored on the server filesystem. On Render's free tier, the filesystem is ephemeral — the database resets on each redeploy. For persistent production use, switch to PostgreSQL (e.g. via Render's managed Postgres or Supabase).
- No image upload — course thumbnails are set via URL only.
- No email verification or password reset flow.
- Admin can see all courses; multi-instructor filtering is not implemented.
