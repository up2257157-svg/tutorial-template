Implementation
==============

Overview
--------

This chapter outlines the technologies and tools used to implement the UniSoc application. The implementation
is built across a Flutter frontend and a Django REST backend, with PostgreSQL serving as the underlying
relational database. Each technology was chosen deliberately based on the requirements gathered in the
previous chapter, the team's existing skillset, and the practical constraints of delivering a working
system within the project timeline. Before committing to the full stack, the team conducted prototype
testing to confirm that the chosen technologies could integrate correctly and support the range of features
the application required. This validation step helped surface integration issues early and gave the team
confidence in the chosen direction before significant development effort was invested.

Frontend
--------

Flutter was chosen for the frontend because it allows a single codebase to compile and run natively on
both iOS and Android. This significantly reduced the amount of duplicate work required compared to
maintaining two separate platform-specific codebases, and ensured that the user experience remained
consistent regardless of which device a student was using. Flutter's rendering engine produces natively
compiled interfaces rather than relying on web views or bridges, which means the application performs
smoothly even on older or lower-end devices commonly used by students.

The framework's widget library made it practical to build the more visually structured parts of the
application, particularly the calendar-style event view and the society directory with its search and
filter controls. Flutter's declarative UI approach also made it straightforward to keep the interface
in sync with backend data, such as updating remaining event capacity in real time after a user RSVPs.
The team's prior familiarity with Flutter and Dart was also a meaningful factor in this decision, as it
removed the need for an extended onboarding period and allowed the team to focus on delivering features
rather than learning a new framework from scratch.

Backend
-------

Django was selected as the backend framework because it provided strong built-in support for the features
most critical to this project. Its authentication system offered a secure, well-tested foundation for
password hashing, user session management, and JWT token generation, meaning the team did not need to
implement these security-sensitive components from scratch. This was particularly important given the
requirement that passwords must never be stored in plain text, a constraint that Django's authentication
framework enforces by default through its PBKDF2-based hashing mechanism.

Django REST Framework extended Django's capabilities with a structured approach to building API endpoints,
including built-in serialisation, input validation, and permission handling. This allowed the team to
define clear API contracts between the frontend and backend and to enforce role-based access control
without writing large amounts of custom middleware. Django's ORM also integrated cleanly with PostgreSQL,
making it straightforward to translate the relational data model into database operations without resorting
to raw SQL for most tasks. The team's existing experience with Python was a further practical advantage,
allowing backend code to be written and reviewed efficiently throughout the project.

The backend was organised into logical service areas covering authentication, society and membership
management, events and attendance, notifications, and audit logging. Although these are not separately
deployed microservices, maintaining clear internal boundaries between these areas kept the codebase easier
to navigate, test, and extend as requirements evolved during development.

Database
--------

PostgreSQL was selected as the database for its reliability, mature support for relational data modelling,
and its first-class integration with Django's ORM. The UniSoc data model is inherently relational, with
users, societies, memberships, events, and RSVPs all linked through foreign key relationships, and
PostgreSQL handles this kind of schema efficiently and safely. Its strong ACID compliance ensures that
data remains consistent even under concurrent load, which was important for features like RSVP handling
where multiple users might interact with the same event record at the same time.

Two database-level constraints were particularly significant to the correctness of the application. The
Membership table uses a unique_together constraint on the user and society fields, which enforces at the
database level that a user cannot join the same society more than once. Similarly, the RSVP table uses
a unique_together constraint on the event and user fields to prevent duplicate attendance records.
Implementing these constraints at the database level rather than relying solely on application-layer
validation means they are enforced consistently regardless of how requests reach the backend, providing
a reliable safety net against data integrity issues.

Development Approach
--------------------

The team followed an iterative development approach throughout the project, revisiting and refining both
the system architecture and the implementation as new requirements and challenges emerged. At the start of
the project, prototype testing was used to validate the core technology choices and confirm that the
Flutter frontend and Django backend could communicate correctly over the planned REST API. This early
validation helped the team identify integration issues before they became embedded in the codebase and
gave a clear basis for the API contracts that would govern communication between the two layers throughout
development.

APIs were tested independently as they were built, verifying that request and response formats matched
expectations before each endpoint was integrated with the frontend. This approach reduced the number of
integration bugs encountered later in the project and made it easier to isolate the source of problems
when they did occur. Iterative development also meant that the architecture could be adjusted in response
to feedback and new understanding. The system went through four distinct architectural iterations, moving
from a simple service separation in the first version through to the final design which explicitly
separates user and admin responsibilities and maps each backend component to a defined set of system
requirements.

A deliberate pragmatic decision was made not to implement the backend as a set of separately deployed
microservices, despite the architectural model describing distinct service areas. Deploying and managing
multiple independent services would have introduced significant operational overhead that was not
justified by the scale of the project. Instead, the logical service boundaries were maintained within
a single Django application, keeping the system straightforward to develop, test, and deploy while still
preserving the separation of concerns that the architecture was designed to achieve.

Conclusion
----------

The combination of Flutter, Django REST Framework, and PostgreSQL provided a robust and well-integrated
foundation for the UniSoc application. Each technology was chosen for concrete reasons rooted in the
project's requirements and the team's capabilities rather than novelty or convention. Flutter's
cross-platform rendering, Django's security features and ORM integration, and PostgreSQL's relational
data handling together supported the full range of features the application needed, from secure account
management and society membership through to event scheduling, attendance tracking, in-app messaging,
and targeted notifications. The iterative development approach ensured that both the technology choices
and the architecture remained grounded in real requirements and practical constraints throughout the life
of the project.
