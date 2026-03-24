API Documentation
=================

Overview
--------
The UniSoc API provides endpoints for managing users, societies, events, memberships, and notifications. All requests and responses use JSON format.

Authentication
--------------
- **Login:** POST `/api/auth/login/`
  - Request: `{"username": "<your_up_number_or_email>", "password": "<password>"}`
  - Response: `{"token": "<jwt_token>"}`

- **Register:** POST `/api/auth/register/`
  - Request: `{"name": "", "email": "", "up_number": "", "password": ""}`
  - Response: `{"id": "<user_id>", "name": "<name>", "email": "<email>"}`

Users
-----
- **Get Profile:** GET `/api/users/<user_id>/`
  - Returns user information (name, societies joined, notification preferences)
- **Update Profile:** PUT `/api/users/<user_id>/`
  - Update password or notification preferences.

Societies
---------
- **List Societies:** GET `/api/societies/`
  - Returns all societies with details like name, type, category
- **Get Society Details:** GET `/api/societies/<society_id>/`
- **Join Society:** POST `/api/societies/<society_id>/join/`
- **Leave Society:** POST `/api/societies/<society_id>/leave/`

Events
------
- **List Events:** GET `/api/events/`
  - Returns upcoming events for societies the user is a member of
- **RSVP to Event:** POST `/api/events/<event_id>/rsvp/`
  - Request: `{"attending": true}`
- **Create Event (Admin only):** POST `/api/events/`
- **Edit Event (Admin only):** PUT `/api/events/<event_id>/`
- **Delete Event (Admin only):** DELETE `/api/events/<event_id>/`

Notifications
-------------
- **List Notifications:** GET `/api/notifications/`
- **Update Notification Preferences:** PUT `/api/notifications/<user_id>/`

Notes
-----
- All endpoints require authentication (JWT token) except registration and login.
- Only admins can create, edit, or delete events.
- API errors return standard HTTP status codes and JSON error messages.

