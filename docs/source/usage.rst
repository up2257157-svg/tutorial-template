Usage
=====

User Accounts
-------------

The user account system is the entry point for all functionality within UniSoc. Before accessing any
part of the application, students must register for an account using their university email address,
UP number, and a chosen password. The registration process is designed to be straightforward, and once
completed the user can log in immediately. On successful login the system issues a JWT token which is
used to authenticate all subsequent requests made during that session, ensuring that only verified users
can access protected parts of the application.

Profile management is available at any time through the account settings screen. Users can update their
email address, change their password, and adjust their notification preferences without needing to
contact an administrator. Password changes are validated and re-hashed before being saved, maintaining
the same level of security as the original registration process. If a user forgets their password
entirely, a reset flow is available from the login screen which sends a secure link to their registered
email address, allowing them to set a new password and regain access to their account.

Society Management
------------------

The society directory is where users spend much of their time within the application. It presents a
browsable list of all university societies, each showing its name, category, type, and a brief
description. A search bar at the top of the directory allows users to find a specific society by name,
while filter controls let them narrow the list down by category or society type, making it much easier
to discover groups that match their interests without scrolling through every entry manually.

Joining a society is done directly from the society's detail page. Once a user becomes a member, the
society's upcoming events appear in their calendar view and they begin receiving notifications according
to their preferences. The system prevents a user from joining a society they already belong to, which
keeps membership records clean and avoids duplicate notification entries. Leaving a society is equally
simple and can be done at any time from the same detail page. When a user leaves, they are removed from
the membership list immediately and notifications from that society stop without any further action
required.

The calendar view gives users a clear overview of all upcoming events across the societies they have
joined, arranged by date. Tapping on any event opens a detail page showing the full event information
including the title, date, time, location, description, and the number of spaces remaining. This gives
users everything they need to decide whether they want to attend before committing. To confirm
attendance, users RSVP directly from the event detail page, and the remaining capacity updates
automatically to reflect their registration. RSVPs can also be cancelled from the same page if plans
change, freeing up the space for another student.

Notifications
-------------

The notification system in UniSoc is designed to keep users informed about society activity without
overwhelming them with alerts they did not ask for. When a user joins a society, they begin receiving
notifications about that society's events and activities by default, but they can opt out at any time
through the account settings screen. Preferences are managed on a per-society basis, meaning a user
can choose to receive notifications from some societies while silencing others, giving them precise
control over what the application communicates to them.

Notifications are triggered when a new event is created by a society the user belongs to, when an
upcoming event is approaching, and when an event the user has RSVPed to is edited or cancelled.
This last case is particularly important for trust in the platform, as users need to be reliably
informed if plans change after they have already committed to attending. When a user leaves a society,
their notification preferences for that society are deactivated automatically, so they never receive
alerts from a group they are no longer part of.

Messaging
---------

The in-app messaging feature allows users to communicate directly with society admins without leaving
the application or relying on external platforms. From any society's detail page, users can open a
conversation with the society's admin and send messages through the built-in chat interface. All
conversations are stored within the app and accessible from the messages tab, where users can see
their full message history across all societies. This feature was introduced because feedback during
requirements elicitation made clear that communication between students and society committees was
fragmented across too many different channels, and a single centralised messaging system within
the app was a much more practical solution.

Admin Features
--------------

Society admins have access to an admin dashboard that provides a dedicated set of tools for managing
their society's presence within the application. The dashboard is the central hub for all
administrative activity and is only accessible to users who have been assigned an admin role during
registration or by the system.

Event management is the primary function of the admin dashboard. Admins can create new events by
providing a title, date, time, location, description, and a capacity limit. Once published, the event
appears immediately on the main calendar page for all members of that society. If details need to
change after publication, admins can edit any field and the update is reflected on the calendar
straight away. Events can also be removed entirely when necessary, and when this happens the system
deletes the event from the database, removes it from the calendar, and notifies any users who had
already RSVPed so they are not left expecting an event that no longer exists.

Attendance tracking is also available through the admin dashboard. Admins can see live RSVP counts
and remaining capacity figures for each event, which update in real time as users confirm or cancel
their attendance. Capacity limits set during event creation are enforced by the system automatically,
so once an event is full no further RSVPs are accepted. For planning and reporting purposes, admins
can also generate attendance reports for any event, which draw on the RSVP and audit log data stored
in the database to produce a record of who registered and when.

Beyond events, admins can manage their society's profile information through the dashboard. This
includes updating the society's name, description, category, and committee details, with all changes
saved to the database and shown immediately on the society's public page. Admins can also update
their own account settings in the same way as standard users, including changing their password and
adjusting their personal notification preferences independently of their administrative role.
