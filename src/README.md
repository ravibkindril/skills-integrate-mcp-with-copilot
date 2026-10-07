# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Teacher-only student registration and unregistration
- Teacher login with an HTTP-only session cookie

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Run the application:

   ```
   uvicorn app:app --app-dir src --reload
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc
   - Activities page: http://localhost:8000

4. Create `src/teachers.json` with the teacher usernames and assigned passwords:

   ```json
   {
     "teacher": "replace-with-a-unique-password"
   }
   ```

   This file is excluded from Git so passwords are not committed. Keep it private
   and use strong, unique passwords. Set `TEACHER_CREDENTIALS_FILE` to use a
   different path. Without this file, public activity viewing works, but teacher
   login is unavailable.

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| GET    | `/auth/status`                                                    | Check whether the current browser has a teacher session             |
| POST   | `/auth/login`                                                     | Start a teacher session                                             |
| POST   | `/auth/logout`                                                    | End a teacher session                                               |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Teacher-only student registration                                   |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Teacher-only student unregistration                              |

## Data Model

The application uses a simple data model with meaningful identifiers:

1. **Activities** - Uses activity name as identifier:

   - Description
   - Schedule
   - Maximum number of participants allowed
   - List of student emails who are signed up

2. **Students** - Uses email as identifier:
   - Name
   - Grade level

All data is stored in memory, which means data will be reset when the server restarts.
