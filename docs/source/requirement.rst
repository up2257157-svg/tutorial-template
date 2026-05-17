User Requirements
=================

Functional Requirements
-----------------------

The UniSoc application was designed around a set of functional requirements gathered directly from student
users and society administrators. These requirements reflect the core activities that users need to perform
within the system and formed the foundation for all design and implementation decisions throughout the
project. The requirements were refined iteratively as the architecture evolved, with each iteration
bringing the system closer to a design that could realistically satisfy the needs identified during
elicitation.

Users must be able to create an account using a secure registration process. During the requirements
elicitation phase it became clear that a reliable and trustworthy registration flow was essential to
getting students to engage with the platform. Users provide their university email address, UP number,
and a password when signing up. The system hashes passwords immediately before storage using Django's
authentication framework, ensuring that plain-text credentials are never retained in the database. Role
selection was also added to the registration process to distinguish between standard users and society
admins, allowing the system to enforce different levels of access across the application.

Users must be able to join and leave societies. A central feature of UniSoc is allowing students to
discover and become members of university societies. The system prevents a user from joining a society
they have already joined by enforcing a unique constraint at the database level on the Membership table,
ensuring that membership records remain accurate and that attendance and notification data is not
corrupted by duplicate entries. Users can leave a society at any time, and when they do the system
immediately removes them from the membership list and stops all notifications from that society without
requiring any additional action from the user.

Users must be able to filter societies and view events clearly on a calendar-style interface. Students
at a university may face dozens of societies and hundreds of events across a semester, and without clear
navigation tools this information quickly becomes overwhelming. The system provides search and filter
functionality allowing users to find societies by name, type, or category, and events are displayed in
a calendar-style view that makes it easy to see what is coming up and when. These features were added
in direct response to feedback that existing solutions made it difficult to discover and track relevant
activities.

Users must be able to see event details including description, location, and attendance figures. When a
user selects an event from the calendar, they are shown a detail page containing the event title, date,
time, location, full description, and the number of spaces remaining. Users reported that across other
platforms event information was often incomplete or scattered, requiring them to visit multiple sources
to get a clear picture. Centralising this information on a single detail page addresses that frustration
directly and gives users everything they need to decide whether to attend.

Users must be able to communicate with society admins through an in-app messaging feature. Prior to
UniSoc, communication between students and society committees typically relied on a mixture of group
chats, social media pages, and email threads, which made it difficult to track conversations or get
timely responses. The messaging feature built into the application provides a direct channel between
users and society admins, keeping all communication organised within a single platform and reducing
the friction involved in asking questions or raising concerns about events.

Non-Functional Requirements
----------------------------

Beyond the core features of the system, a number of non-functional requirements were identified that
govern how the application must behave in terms of security, privacy, and performance. These
requirements are just as important as the functional ones, as they define the conditions under which
the system must operate reliably and safely.

Passwords must be hashed and never stored in plain text. This was treated as a non-negotiable security
requirement throughout the project. All passwords, whether belonging to standard users or admins, are
processed through Django's authentication framework and stored as hashed values using PBKDF2 with
SHA-256 before being written to the database. The system was designed from the outset to enforce this
at the backend level rather than relying on the frontend to handle it, meaning there is no code path
through which a plain-text password could be persisted.

Data privacy must be ensured across all parts of the system. UniSoc handles personal information
including email addresses, university identification numbers, and society membership records, all of
which must be stored securely and exposed only where strictly necessary. API responses are designed to
return only the data required for a given operation, avoiding unnecessary exposure of personal details.
Audit logging was also built into the system architecture to record significant actions such as joining
or leaving a society and changing notification preferences, providing a record of system activity that
supports accountability and troubleshooting without compromising user privacy.

The system must support up to 2000 simultaneous users. Given that UniSoc is intended for use across an
entire university student population, the system must remain stable and responsive under significant
concurrent load. Database queries for society filtering and event lookups use indexed fields to keep
response times acceptable as the dataset grows, and atomic database updates are used during RSVP
operations to prevent race conditions when multiple users attempt to register attendance for the same
near-capacity event at the same time. These decisions were made during the architectural design phase
specifically to ensure the system could scale to the expected user base without degrading in
reliability or performance.
