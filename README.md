# AI Recruitment Platform - LEAN IT

A comprehensive AI-powered recruitment platform with role-based access control, intelligent candidate matching, automated screening calls, and analytics dashboard.

## 📋 Table of Contents

- [Overview](#overview)
- [Architecture](#architecture)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Key Features](#key-features)
- [Authentication & Authorization](#authentication--authorization)
- [Data Models](#data-models)
- [API Endpoints](#api-endpoints)
- [Key Workflows](#key-workflows)
- [Setup Instructions](#setup-instructions)
- [Environment Variables](#environment-variables)
- [Deployment](#deployment)

---

## 🎯 Overview

The AI Recruitment Platform is a full-stack application designed to streamline the recruitment process through:

- **AI-Powered Candidate Matching**: Vector-based semantic search using embeddings
- **Automated Screening Calls**: Integration with Bolna AI for phone screenings
- **Role-Based Access Control**: Admin, Manager, and Recruiter roles with distinct permissions
- **Resume Parsing**: Automated extraction of candidate information from PDFs/DOCX files
- **Analytics Dashboard**: Comprehensive insights into job postings, applications, and matches
- **AI Chatbot**: Interactive assistant for user queries

---

## 🏗️ Architecture

### Frontend Architecture

**Framework**: Next.js 16 (App Router)
- **Routing**: File-based routing with dynamic segments (`[id]`, `[candidateId]`)
- **State Management**: React Context API (`AuthContext`)
- **Styling**: Tailwind CSS with glassmorphism design
- **UI Components**: Shadcn/ui (Radix UI primitives)
- **Charts**: Recharts for analytics visualization
- **API Integration**: Centralized API client (`lib/api.js`)

**Key Frontend Patterns**:
- Client-side authentication with JWT tokens stored in localStorage
- Protected routes based on user roles
- Server-side rendering (SSR) for initial page loads
- Client-side navigation with Next.js router

### Backend Architecture

**Framework**: Express.js (Node.js)
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT (JSON Web Tokens)
- **Vector Database**: MongoDB Atlas Vector Search
- **File Upload**: Multer for resume parsing
- **Scheduled Tasks**: Node-cron for automated jobs

**Key Backend Patterns**:
- RESTful API design
- Middleware-based authentication and authorization
- Service layer for business logic
- Controller layer for request handling
- Model layer for data schemas

---

## 🛠️ Tech Stack

### Frontend
- **Next.js 16** - React framework with App Router
- **React 19** - UI library
- **TypeScript** - Type safety
- **Tailwind CSS 4** - Utility-first CSS
- **Shadcn/ui** - Component library
- **Recharts** - Chart library
- **Lucide React** - Icons
- **Sonner** - Toast notifications
- **React Hook Form** - Form management
- **Zod** - Schema validation

### Backend
- **Node.js** - Runtime environment
- **Express.js** - Web framework
- **MongoDB** - NoSQL database
- **Mongoose** - ODM for MongoDB
- **JWT** - Authentication tokens
- **bcryptjs** - Password hashing
- **Multer** - File upload handling
- **pdf-parse** - PDF text extraction
- **mammoth** - DOCX text extraction
- **@xenova/transformers** - ML embeddings
- **@qdrant/js-client-rest** - Vector database client
- **LangChain** - LLM integration
- **OpenAI API** - LLM services
- **Google APIs** - Calendar integration
- **Nodemailer** - Email service
- **Node-cron** - Scheduled tasks
- **Axios** - HTTP client

---

## 📁 Project Structure

```
AI Recruitment/
├── client/                          # Next.js frontend application
│   ├── app/                         # App Router pages
│   │   ├── dashboard/               # Dashboard pages by role
│   │   │   ├── admin/               # Admin-specific pages
│   │   │   │   ├── candidate/       # Candidate management
│   │   │   │   ├── manage-job-posting/  # Job posting management
│   │   │   │   ├── user-management/     # User CRUD operations
│   │   │   │   ├── reports/         # Analytics and reports
│   │   │   │   └── top-applicants/  # Top candidates view
│   │   │   ├── manager/             # Manager-specific pages
│   │   │   │   ├── candidate/       # Candidate management
│   │   │   │   ├── manage-job-posting/  # Job posting management
│   │   │   │   ├── user-management/     # User management
│   │   │   │   ├── reports/         # Analytics
│   │   │   │   └── settings/        # Settings page
│   │   │   ├── recruiter/           # Recruiter-specific pages
│   │   │   │   ├── candidate/       # Candidate management
│   │   │   │   ├── manage-job-posting/  # Job posting management
│   │   │   │   ├── calendar/        # Calendar view
│   │   │   │   ├── tasks/           # Task management
│   │   │   │   ├── messages/        # Messages
│   │   │   │   ├── calcom-credentials/  # Cal.com integration
│   │   │   │   └── settings/        # Settings
│   │   │   ├── login/               # Login page
│   │   │   ├── jobs/                # Public job listings
│   │   │   ├── profile/             # User profile
│   │   │   └── page.tsx             # Landing page
│   │   ├── layout.tsx               # Root layout with AuthProvider
│   │   └── globals.css              # Global styles
│   ├── components/                  # React components
│   │   ├── Sidebar.jsx              # Navigation sidebar
│   │   ├── Navbar.jsx               # Top navigation
│   │   ├── Chatbot.jsx              # AI chatbot component
│   │   ├── dashboard/               # Dashboard-specific components
│   │   │   ├── UserAnalyticsPage.jsx    # Analytics visualization
│   │   │   ├── JobsPageContent.jsx      # Jobs listing
│   │   │   ├── ReportsPageContent.jsx   # Reports view
│   │   │   └── TopApplicantsPageContent.jsx  # Top applicants
│   │   └── ui/                      # Shadcn/ui components
│   ├── contexts/                    # React contexts
│   │   └── AuthContext.jsx          # Authentication context
│   ├── lib/                         # Utility libraries
│   │   ├── api.js                   # API client functions
│   │   ├── utils.ts                 # Utility functions
│   │   └── timeFormatter.js         # Time formatting
│   ├── config/                      # Configuration files
│   │   └── roleMenuConfig.js        # Role-based menu configuration
│   └── public/                      # Static assets
│       └── LEAN AI POWERED orginal.png         # Logo
│
├── server/                          # Express.js backend application
│   ├── server.js                    # Entry point
│   ├── config/                      # Configuration
│   │   └── database.js              # MongoDB connection
│   ├── middleware/                  # Express middleware
│   │   └── auth.middleware.js       # JWT authentication
│   ├── user/                        # User module
│   │   ├── model.js                 # User schema
│   │   ├── controller.js            # User controllers
│   │   └── route.js                 # User routes
│   ├── candidates/                  # Candidate module
│   │   ├── model.js                 # Candidate schema
│   │   ├── controller.js            # Candidate controllers
│   │   └── route.js                 # Candidate routes
│   ├── job_posting/                 # Job posting module
│   │   ├── model.js                 # Job posting schema
│   │   ├── controller.js            # Job posting controllers
│   │   └── route.js                 # Job posting routes
│   ├── matching/                    # Matching module
│   │   ├── model.js                 # Match schemas
│   │   ├── controller.js            # Matching controllers
│   │   └── route.js                 # Matching routes
│   ├── resume_parser/               # Resume parser module
│   │   ├── controller.js            # Resume parsing logic
│   │   └── route.js                 # Resume parser routes
│   ├── bolna/                       # Bolna AI integration
│   │   ├── model.js                 # Bolna call schema
│   │   ├── controller.js            # Bolna controllers
│   │   └── route.js                 # Bolna routes
│   ├── recruiter_availability/      # Recruiter availability
│   │   ├── model.js                 # Availability schema
│   │   ├── controller.js            # Availability controllers
│   │   └── route.js                 # Availability routes
│   ├── recruiter_tasks/             # Recruiter tasks
│   │   ├── model.js                 # Task schema
│   │   ├── controller.js            # Task controllers
│   │   └── route.js                 # Task routes
│   ├── candidate_progress/         # Candidate progress tracking
│   │   ├── model.js                 # Progress schema
│   │   ├── controller.js            # Progress controllers
│   │   └── route.js                 # Progress routes
│   ├── dashboard/                   # Dashboard module
│   │   ├── controller.js            # Dashboard controllers
│   │   └── route.js                 # Dashboard routes
│   ├── chat/                        # Chat module
│   │   ├── controller.js            # Chat controllers
│   │   └── route.js                 # Chat routes
│   ├── calcom_credentials/         # Cal.com integration
│   │   ├── model.js                 # Credentials schema
│   │   ├── controller.js            # Credentials controllers
│   │   └── route.js                 # Credentials routes
│   ├── services/                    # Business logic services
│   │   ├── matchingService.js       # Candidate-job matching
│   │   ├── embeddingService.js      # Vector embeddings
│   │   ├── emailService.js          # Email notifications
│   │   ├── calServices.js           # Calendar services
│   │   ├── cronJobs.js              # Scheduled tasks
│   │   └── matchingCronJobs.js      # Matching automation
│   ├── utils/                       # Utility functions
│   │   └── timeFormatter.js         # Time formatting
│   └── scripts/                     # Utility scripts
│       └── seedAdmin.js             # Admin user seeding
│
└── README.md                        # This file
```

---

## ✨ Key Features

### 1. **Role-Based Access Control (RBAC)**
   - **Admin**: Full system access, user management, all job postings
   - **Manager**: Team oversight, job posting management, candidate review
   - **Recruiter**: Job posting creation, candidate management, interview scheduling

### 2. **AI-Powered Candidate Matching**
   - Vector embeddings for semantic similarity
   - Match score calculation (0-100)
   - Filtering by experience, role, skills
   - Automatic match refresh

### 3. **Automated Screening Calls (Bolna AI)**
   - Phone screening automation
   - Transcript analysis with LLM
   - Interview outcome tracking
   - Email notifications

### 4. **Resume Parsing**
   - PDF and DOCX support
   - Automatic field extraction (name, email, skills, experience)
   - Optional database storage

### 5. **Analytics Dashboard**
   - Job posting statistics
   - Application trends
   - Role and skill distribution
   - Experience analysis
   - Animated pie charts

### 6. **AI Chatbot**
   - Integration with n8n webhook
   - Session-based conversations
   - Context-aware responses

### 7. **Calendar Integration (Cal.com)**
   - Recruiter availability management
   - Event type configuration
   - Interview scheduling

### 8. **Candidate Progress Tracking**
   - Stage-based workflow
   - Automated progress updates from screening calls
   - Status history

---

## 🔐 Authentication & Authorization

### Authentication Flow

1. **Login**:
   ```
   POST /api/users/login
   Body: { email, password }
   Response: { token, user }
   ```

2. **Token Storage**: JWT stored in `localStorage` on frontend

3. **Token Validation**: 
   - Frontend: `AuthContext` checks token on mount
   - Backend: `authenticate` middleware validates JWT

4. **Protected Routes**: 
   - Frontend: Role-based redirects in `useEffect`
   - Backend: `authenticate` middleware on protected routes

### Authorization

**Middleware**: `server/middleware/auth.middleware.js`
- `authenticate`: Validates JWT token
- `authorize(...roles)`: Checks user role against allowed roles

**Role Permissions**:

| Feature | Admin | Manager | Recruiter |
|---------|-------|---------|-----------|
| User Management | ✅ | ✅ | ❌ |
| Job Posting (All) | ✅ | ✅ | ❌ |
| Job Posting (Own) | ✅ | ✅ | ✅ |
| Candidate Management | ✅ | ✅ | ✅ |
| Analytics | ✅ | ✅ | ❌ |
| Settings | ✅ | ✅ | ✅ |

---

## 📊 Data Models

### User Model
```javascript
{
  name: String (required),
  email: String (required, unique),
  password: String (required, hashed),
  role: String (enum: ["admin", "recruiter", "manager"]),
  assignedManager: ObjectId (ref: User), // For recruiters
  assignedAdmin: ObjectId (ref: User)
}
```

### Candidate Model
```javascript
{
  name: String (required),
  email: String (required, unique),
  phone_no: String (required),
  image: String, // Profile photo URL
  skills: [String],
  experience: Number, // Years
  resume_url: String,
  role: [String], // Free-form roles
  bio: String,
  is_active: Boolean (default: true),
  social_links: {
    linkedin: String,
    github: String,
    portfolio: String
  },
  vector: [Number] // 384-dimensional embedding (hidden by default)
}
```

### Job Posting Model
```javascript
{
  id: String (required, unique), // e.g., "JOB-1001"
  title: String (required),
  description: String (required),
  company: String (required),
  role: [String],
  ctc: String, // e.g., "15-25 LPA"
  exp_req: Number, // Years
  job_type: String (enum: ["Full time", "Internship"]),
  skills: [String],
  vector: [Number], // 384-dimensional embedding (hidden by default)
  primary_recruiter_id: ObjectId (ref: User),
  secondary_recruiter_id: [ObjectId] (ref: User),
  status: String (enum: ["draft", "open", "closed"])
}
```

### Matching Models

**JobMatches**:
```javascript
{
  job_id: ObjectId (ref: JobPosting),
  candidate_id: ObjectId (ref: Candidate),
  match_score: Number (0-100),
  status: String (enum: ["pending", "applied", "rejected"]),
  applied_at: Date
}
```

**CandidateMatches**:
```javascript
{
  candidate_id: ObjectId (ref: Candidate),
  job_id: ObjectId (ref: JobPosting),
  match_score: Number (0-100),
  status: String
}
```

### Bolna Call Model
```javascript
{
  execution_id: String (unique),
  job_id: ObjectId (ref: JobPosting),
  candidate_id: ObjectId (ref: Candidate),
  recruiter_id: ObjectId (ref: User),
  status: String,
  transcript: String,
  score: Number,
  outcome: String,
  feedback: String
}
```

---

## 🔌 API Endpoints

### Authentication
- `POST /api/users/login` - User login
- `GET /api/users/me` - Get current user (authenticated)

### User Management
- `GET /api/users` - Get all users (Admin/Manager)
- `GET /api/users/:id` - Get user by ID
- `POST /api/users` - Create user (Admin)
- `PUT /api/users/:id` - Update user
- `DELETE /api/users/:id` - Delete user (Admin)
- `GET /api/users/recruiters` - Get all recruiters
- `GET /api/users/managers` - Get all managers

### Candidates
- `GET /api/candidates` - Get all candidates
- `GET /api/candidates/:id` - Get candidate by ID
- `POST /api/candidates` - Create candidate
- `PUT /api/candidates/:id` - Update candidate
- `DELETE /api/candidates/:id` - Delete candidate
- `GET /api/candidates/role/:role` - Get candidates by role

### Job Postings
- `GET /api/job-postings` - Get all job postings (with filters)
- `GET /api/job-postings/:id` - Get job posting by ID
- `POST /api/job-postings` - Create job posting
- `PUT /api/job-postings/:id` - Update job posting
- `DELETE /api/job-postings/:id` - Delete job posting

### Matching
- `GET /api/matching/job/:jobId/candidates` - Get matched candidates for job
- `POST /api/matching/job/:jobId/refresh` - Refresh matches for job
- `GET /api/matching/candidate/:candidateId/jobs` - Get matched jobs for candidate
- `POST /api/matching/candidate/:candidateId/refresh` - Refresh matches for candidate
- `POST /api/matching/job/:jobId/candidate/:candidateId/apply` - Mark candidate as applied

### Resume Parser
- `POST /api/resume-parser/url` - Parse resume from URL
- `POST /api/resume-parser/upload` - Parse resume from file upload

### Bolna (Screening Calls)
- `POST /api/bolna/schedule-call` - Schedule a screening call
- `POST /api/bolna/schedule-calls-batch` - Schedule multiple calls
- `POST /api/bolna/check-calls` - Check call statuses
- `POST /api/bolna/call/:executionId/stop` - Stop a call
- `POST /api/bolna/job/:jobId/stop-all` - Stop all calls for a job
- `GET /api/bolna/call/:executionId/status` - Get call status
- `GET /api/bolna/job/:jobId/calls` - Get all calls for a job
- `GET /api/bolna/job/:jobId/screenings` - Get screening results
- `GET /api/bolna/job/:jobId/interviews` - Get interview results
- `POST /api/bolna/call/:executionId/outcome` - Update interview outcome

### Recruiter Availability
- `POST /api/recruiter-availability` - Create/update availability
- `GET /api/recruiter-availability/job/:jobId` - Get availability by job
- `GET /api/recruiter-availability/job/:jobId/all` - Get all availability for job
- `GET /api/recruiter-availability/recruiter/my-availability` - Get my availability
- `PATCH /api/recruiter-availability/job/:jobId/slots` - Update availability slots
- `DELETE /api/recruiter-availability/job/:jobId` - Delete availability

### Recruiter Tasks
- `GET /api/recruiter-tasks` - Get tasks (with filters)
- `GET /api/recruiter-tasks/stats` - Get task statistics
- `GET /api/recruiter-tasks/candidate/:candidateId/interviews` - Get candidate interviews
- `GET /api/recruiter-tasks/recruiter/:recruiterId/booked-slots` - Get booked slots
- `PATCH /api/recruiter-tasks/:taskId/status` - Update task status
- `POST /api/recruiter-tasks/:taskId/cancel` - Cancel interview

### Dashboard
- `GET /api/dashboard/stats` - Get dashboard statistics
- `GET /api/dashboard/analytics/:userId` - Get user analytics

### Chat
- `POST /api/chat/message` - Send chat message
- `GET /api/chat/history/:sessionId?` - Get chat history

### Cal.com Credentials
- `POST /api/calcom-credentials/save-key` - Save API secret key
- `GET /api/calcom-credentials/:recruiterId/event-types` - Get event types
- `POST /api/calcom-credentials/save-event-type` - Save event type
- `GET /api/calcom-credentials/:recruiterId` - Get credentials

### Candidate Progress
- `GET /api/candidate-progress/candidate/:candidateId/job/:jobPostingId` - Get or create progress
- `GET /api/candidate-progress/job/:jobPostingId` - Get progress by job
- `GET /api/candidate-progress/candidate/:candidateId` - Get progress by candidate

---

## 🔄 Key Workflows

### 1. User Registration & Login Flow

```
1. Admin creates user via User Management
2. User receives credentials (email/password)
3. User logs in via /login page
4. Backend validates credentials
5. JWT token generated and returned
6. Token stored in localStorage
7. User redirected to role-specific dashboard
```

### 2. Job Posting Creation Flow

```
1. Recruiter/Manager/Admin creates job posting
2. Job details entered (title, description, skills, etc.)
3. Backend generates vector embedding for job
4. Job saved to database with status "draft" or "open"
5. Job appears in job listings
```

### 3. Candidate Matching Flow

```
1. Job posting created/updated
2. Vector embedding generated for job
3. Matching service searches candidate vectors
4. Cosine similarity calculated
5. Top matches stored in JobMatches collection
6. Matches displayed in "AI Match" tab
7. Recruiter can refresh matches manually
```

### 4. Screening Call Flow (Bolna)

```
1. Recruiter selects candidate for screening
2. Recruiter schedules call via Bolna API
3. Bolna initiates phone call to candidate
4. Call transcript generated
5. LLM analyzes transcript against job description
6. Score (0-100) calculated
7. Outcome stored (pass/fail/review)
8. Candidate progress updated automatically
9. Email notification sent
```

### 5. Resume Parsing Flow

```
1. User uploads resume (PDF/DOCX) or provides URL
2. Backend extracts text from file
3. LLM parses text to extract fields:
   - Name, Email, Phone
   - Skills, Experience
   - Roles, Bio
4. Optional: Save to database as candidate
5. Vector embedding generated
6. Candidate available for matching
```

### 6. Candidate Progress Tracking Flow

```
1. Candidate matched to job
2. Screening call scheduled
3. Call completed → Progress: "Screening"
4. If passed → Progress: "Interview Scheduled"
5. Interview completed → Progress: "Interview Completed"
6. Outcome recorded (hired/rejected/pending)
7. Progress history maintained
```

---

## 🚀 Setup Instructions

### Prerequisites

- Node.js 18+ and npm
- MongoDB (local or Atlas)
- OpenAI API key (for embeddings and LLM)
- Bolna API credentials (for screening calls)
- Cal.com API credentials (optional, for calendar)

### Backend Setup

1. **Navigate to server directory**:
   ```bash
   cd server
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Create `.env` file**:
   ```env
   PORT=5000
   MONGODB_URI=mongodb://localhost:27017/ai-recruitment
   JWT_SECRET=your-secret-key-here
   OPENAI_API_KEY=your-openai-api-key
   BOLNA_API_KEY=your-bolna-api-key
   N8N_WEBHOOK_URL=https://your-n8n-webhook-url
   NODE_ENV=development
   ```

4. **Seed admin user** (optional):
   ```bash
   npm run seed:admin
   ```

5. **Start server**:
   ```bash
   # Development
   npm run dev

   # Production
   npm start
   ```

### Frontend Setup

1. **Navigate to client directory**:
   ```bash
   cd client
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Create `.env.local` file**:
   ```env
   NEXT_PUBLIC_API_URL=http://localhost:5000/api
   ```

4. **Start development server**:
   ```bash
   npm run dev
   ```

5. **Access application**:
   - Frontend: http://localhost:3000
   - Backend API: http://localhost:5000

---

## 🔧 Environment Variables

### Backend (`.env`)

| Variable | Description | Required |
|----------|-------------|----------|
| `PORT` | Server port | Yes |
| `MONGODB_URI` | MongoDB connection string | Yes |
| `JWT_SECRET` | Secret for JWT signing | Yes |
| `OPENAI_API_KEY` | OpenAI API key for embeddings/LLM | Yes |
| `BOLNA_API_KEY` | Bolna API key for screening calls | Yes |
| `N8N_WEBHOOK_URL` | n8n webhook URL for chatbot | Yes |
| `NODE_ENV` | Environment (development/production) | No |

### Frontend (`.env.local`)

| Variable | Description | Required |
|----------|-------------|----------|
| `NEXT_PUBLIC_API_URL` | Backend API base URL | Yes |

---

## 📦 Deployment

### Backend Deployment

1. **Build**: No build step required (Node.js runtime)
2. **Environment**: Set environment variables on hosting platform
3. **Database**: Use MongoDB Atlas for production
4. **Process Manager**: Use PM2 or similar for process management

### Frontend Deployment

1. **Build**:
   ```bash
   cd client
   npm run build
   ```

2. **Start**:
   ```bash
   npm start
   ```

3. **Deploy to Vercel** (recommended for Next.js):
   - Connect GitHub repository
   - Set environment variables
   - Deploy automatically on push

---

## 📝 Additional Notes

### Vector Search Setup

MongoDB Atlas Vector Search requires:
1. Atlas cluster with vector search enabled
2. Search index on `candidates` collection:
   - Field: `vector`
   - Type: `knnVector`
   - Dimensions: `384`
   - Similarity: `cosine`

### Cron Jobs

Automated tasks run via `node-cron`:
- Match refresh (configurable schedule)
- Email notifications
- Data cleanup

### File Uploads

Resume files are:
- Parsed in memory (not stored on disk)
- Text extracted and processed
- Optional database storage

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

---

## 📄 License

This project is proprietary software for LEAN IT.

---

## 👥 Support

For issues or questions, contact the development team.

---

**Last Updated**: January 2025
**Version**: 1.0.0

# ai-recruitment-project
