# Mergington High School Activities API

A super simple FastAPI application that allows students to view and sign up for extracurricular activities.

## Features

- View all available extracurricular activities
- Teacher-only student enrollment and unenrollment
- Student rosters remain publicly viewable

## Teacher Access

Set `TEACHER_CREDENTIALS` in the server environment to a JSON object mapping
each teacher username to a password, for example
`{"teacher1":"...","teacher2":"..."}`. Supply this value through your
deployment's secret manager; do not store credentials in the repository. The
teacher login is held in the browser session memory and is cleared when the
page is reloaded or the teacher logs out.

Run the app over HTTPS when it is accessible beyond a trusted local machine,
because teacher credentials are sent to the API for each enrollment change.

## Getting Started

1. Install the dependencies:

   ```
   pip install fastapi uvicorn
   ```

2. Run the application:

   ```
   python app.py
   ```

3. Open your browser and go to:
   - API documentation: http://localhost:8000/docs
   - Alternative documentation: http://localhost:8000/redoc

## API Endpoints

| Method | Endpoint                                                          | Description                                                         |
| ------ | ----------------------------------------------------------------- | ------------------------------------------------------------------- |
| GET    | `/activities`                                                     | Get all activities with their details and current participant count |
| GET    | `/teacher/session`                                                | Verify teacher credentials                                          |
| POST   | `/activities/{activity_name}/signup?email=student@mergington.edu` | Teacher-only student enrollment                                     |
| DELETE | `/activities/{activity_name}/unregister?email=student@mergington.edu` | Teacher-only student unenrollment                               |

Enrollment and unenrollment require HTTP Basic credentials matching the
configured teacher username and password. The API enforces this regardless of
whether requests originate from the website or another client.

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
