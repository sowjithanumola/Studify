# Smart Study Planner - Project Documentation

## Project Overview

**Project Name**: Smart Study Planner  
**Type**: Full-stack React Web Application  
**Core Functionality**: A student productivity application for organizing study schedules, managing tasks, tracking subjects, and improving focus with a Pomodoro timer.

## Tech Stack

- **Frontend**: React 18, TypeScript, Vite, Tailwind CSS
- **Backend/Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth (Email + Password)
- **State Management**: Zustand
- **Charts**: Recharts
- **Icons**: Lucide React

## Features Implemented

### 1. Authentication
- Email/password signup and login
- Password reset functionality
- Protected routes

### 2. Dashboard
- Total study tasks counter
- Completed/remaining tasks
- Today's study time
- Study streak counter
- Weekly progress bar chart
- Productivity percentage
- Task distribution pie chart

### 3. Subject Manager
- Add/edit/delete subjects
- Color-coded subjects
- Subject descriptions
- Data stored in Supabase

### 4. Task Manager
- Create study tasks with title, priority, due date, study time
- Mark tasks as complete
- Filter by subject or priority
- Search tasks
- CRUD operations

### 5. Calendar View
- Monthly calendar display
- Tasks highlighted by priority color
- Click date to view tasks
- Add tasks from calendar

### 6. Pomodoro Timer
- 25-minute focus sessions
- 5-minute breaks
- Session counter
- Study time tracking (saved to Supabase)

### 7. Notes Section
- Quick note creation
- Edit and delete notes
- Auto-save functionality

### 8. Settings
- Theme toggle (dark/light)
- Export data as JSON
- Sign out
- Delete account

## Database Schema

```sql
-- subjects table
CREATE TABLE subjects (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  name TEXT NOT NULL,
  color TEXT NOT NULL DEFAULT '#3B82F6',
  description TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- tasks table
CREATE TABLE tasks (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  subject_id UUID REFERENCES subjects(id) ON DELETE SET NULL,
  title TEXT NOT NULL,
  priority TEXT NOT NULL DEFAULT 'Medium',
  due_date DATE,
  study_time INTEGER DEFAULT 0,
  completed BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- notes table
CREATE TABLE notes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  content TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- study_sessions table
CREATE TABLE study_sessions (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID REFERENCES auth.users(id) ON DELETE CASCADE NOT NULL,
  duration INTEGER NOT NULL,
  date DATE NOT NULL,
  created_at TIMESTAMPTZ DEFAULT NOW()
);
```

## Running Locally

1. Install dependencies:
   ```bash
   npm install
   ```

2. Start development server:
   ```bash
   npm run dev
   ```

3. Build for production:
   ```bash
   npm run build
   ```

## Deployment

The project can be deployed to Vercel, Netlify, or any static hosting platform.

1. Build the project: `npm run build`
2. Deploy the `dist` folder to your preferred hosting platform

## Supabase Configuration

- **Project URL**: https://atktdchuhyshgpoomgyo.supabase.co
- **Anon Key**: Configured in src/lib/supabase.ts

## Folder Structure

```
src/
├── App.tsx              # Main app with routing
├── main.tsx             # Entry point
├── index.css            # Global styles
├── lib/
│   └── supabase.ts      # Supabase client & types
├── store/
│   ├── authStore.ts     # Authentication state
│   └── appStore.ts      # App data state
├── components/
│   └── Sidebar.tsx      # Navigation sidebar
└── pages/
    ├── LoginPage.tsx
    ├── SignupPage.tsx
    ├── ForgotPasswordPage.tsx
    ├── DashboardPage.tsx
    ├── SubjectsPage.tsx
    ├── TasksPage.tsx
    ├── CalendarPage.tsx
    ├── NotesPage.tsx
    ├── TimerPage.tsx
    └── SettingsPage.tsx
```

## Environment Variables

The Supabase credentials are configured in `src/lib/supabase.ts`. For production deployment, consider using environment variables:

```typescript
const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY;
```
