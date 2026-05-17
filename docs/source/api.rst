API Documentation
=================

Overview
--------

The UniSoc API provides endpoints for managing users, societies, events, memberships, notifications,
and messaging. All requests and responses use JSON format. The API is served over HTTPS via a single
API Gateway which acts as the sole entry point from client applications to the backend services. This
design keeps internal service endpoints hidden from direct client access, centralising request routing
and authentication checks in one place. Every endpoint except registration and login requires a valid
JWT token passed as a Bearer token in the Authorization header. Any request missing a valid token or
attempting to access a resource the authenticated user does not have permission to reach will receive
an appropriate HTTP error response with a JSON body describing the issue.

Authentication
--------------

The authentication endpoints handle account creation and login for both standard users and admins.
When a new user registers, their password is hashed immediately using Django's authentication framework
before being written to the database, ensuring plain-text credentials are never stored. On successful
login the API returns a JWT token which the client must include in the Authorization header for all
subsequent authenticated requests.

- **Login:** POST ``/api/auth/login/``

  - Request: ``{"username": "<your_up_number_or_email>", "password": "<password>"}``
  - Response: ``{"token": "<jwt_token>"}``

- **Register:** POST ``/api/auth/register/``

  - Request: ``{"name": "", "email": "", "up_number": "", "password": ""}``
  - Response: ``{"id": "<user_id>", "name": "<name>", "email": "<email>"}``

- **Request Password Reset:** POST ``/api/auth/password-reset/``

  - Request: ``{"email": "<registered_email>"}``
  - Response: ``{"message": "Password reset link sent."}``

- **Confirm Password Reset:** POST ``/api/auth/password-reset/confirm/``

  - Request: ``{"token": "<reset_token>", "new_password": "<new_password>"}``
  - Response: ``{"message": "Password updated successfully."}``

Users
-----

The user endpoints allow clients to retrieve and update profile information for a given account. The
GET endpoint returns the user's name, email address, UP number, role, notification preferences, and
the list of societies they currently belong to. The PUT endpoint allows users to update their password
or notification settings, with password changes re-hashed before storage.

- **Get Profile:** GET ``/api/users/<user_id>/``

  - Returns user information (name, societies joined, notification preferences)

- **Update Profile:** PUT ``/api/users/<user_id>/``

  - Update password or notification preferences.

Societies
---------

The society endpoints expose the full society directory and support all membership management
operations. The list endpoint supports optional query parameters for filtering by name, category, and
type. Joining a society creates a new Membership record; if one already exists the endpoint returns a
409 Conflict response, enforcing the rule that a user cannot join the same society twice. Leaving a
society removes the membership record and immediately deactivates notifications for that society.

- **List Societies:** GET ``/api/societies/``

  - Returns all societies with details like name, type, category
  - Supports ``?name=``, ``?category=``, and ``?type=`` query parameters

- **Get Society Details:** GET ``/api/societies/<society_id>/``

- **Join Society:** POST ``/api/societies/<society_id>/join/``

- **Leave Society:** POST ``/api/societies/<society_id>/leave/``

- **Update Society (Admin only):** PUT ``/api/societies/<society_id>/``

  - Update society name, description, category, or committee details.

Events
------

The event endpoints cover the full lifecycle of an event from creation through to deletion. The RSVP
endpoint enforces capacity atomically, returning a 409 Conflict response if an event is full rather
than accepting an overbooking. Admin-only endpoints support event creation, editing, and deletion.
When an event is deleted, affected users who had RSVPed are notified automatically.

- **List Events:** GET ``/api/events/``

  - Returns upcoming events for societies the user is a member of

- **Get Event Details:** GET ``/api/events/<event_id>/``

  - Returns full event information including attendance count and spaces remaining

- **RSVP to Event:** POST ``/api/events/<event_id>/rsvp/``

  - Request: ``{"attending": true}``
  - Set to ``false`` to cancel an existing RSVP

- **Create Event (Admin only):** POST ``/api/events/``

- **Edit Event (Admin only):** PUT ``/api/events/<event_id>/``

- **Delete Event (Admin only):** DELETE ``/api/events/<event_id>/``

- **Get Attendance Report (Admin only):** GET ``/api/events/<event_id>/attendance/``

  - Returns a list of users who RSVPed along with their attendance status and timestamp

Notifications
-------------

The notification endpoints allow clients to retrieve recent notifications and manage preferences on a
per-society basis. Preference changes take effect immediately and are reflected in the notification
service's dispatch logic from that point forward, giving users granular control over which societies
can send them alerts.

- **List Notifications:** GET ``/api/notifications/``

  - Returns recent notifications for the authenticated user

- **Update Notification Preferences:** PUT ``/api/notifications/<user_id>/``

  - Request: ``{"society_id": "<society_id>", "notifications_enabled": false}``

Messaging
---------

The messaging endpoints support direct communication between users and society admins within the
application. All conversations are stored in the database and accessible at any time through the
messages tab, providing a centralised record of all communication between users and society committees.

- **List Conversations:** GET ``/api/messages/``

  - Returns all active conversations for the authenticated user, including the last message and timestamp

- **Get Conversation:** GET ``/api/messages/<conversation_id>/``

  - Returns the full message history for a conversation between a user and a society admin

- **Send Message:** POST ``/api/messages/``

  - Request: ``{"society_id": "<society_id>", "message": "<message_text>"}``
  - Response: ``{"message_id": "<message_id>", "sent_at": "<timestamp>"}``

Notes
-----

- All endpoints require authentication (JWT token) except registration and login.
- Only admins can create, edit, or delete events, update society information, and access attendance reports.
- Attempting to join a society you are already a member of returns a 409 Conflict response.
- Attempting to RSVP to a full event returns a 409 Conflict response.
- API errors return standard HTTP status codes and JSON error messages.
- All timestamps are returned in ISO 8601 format using UTC.
