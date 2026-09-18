# FamilyHub

## Full-Stack Family Events & Memories Platform

**Specification v1.0**

---

# 1. Product Vision

**FamilyHub** is a private, invitation-only web application that gives a family one shared place to:

* Record important family events
* Organize celebrations
* Manage birthdays
* Invite family members
* Track RSVPs
* Share event information
* Coordinate attendees
* Share photos and videos
* Send announcements
* Receive reminders
* Preserve family memories
* Build a permanent family timeline

### Core principle

> **Every important family moment should have a place.**

The system should work equally well for a simple birthday dinner and a 200-person wedding.

---

# 2. Product Goals

### Primary goals

1. Centralize family events.
2. Make upcoming events immediately visible.
3. Make event creation extremely easy.
4. Keep the family directory connected to events.
5. Provide private RSVP and invitation management.
6. Create a long-term family history.
7. Make photos and memories associated with the events that created them.
8. Work beautifully on desktop, tablet, and mobile.
9. Keep family data private.
10. Use a scalable architecture that can support multiple families.

---

# 3. Non-Goals

The first version should **not** attempt to become:

* A public social network
* A dating platform
* A general-purpose event marketplace
* A wedding vendor marketplace
* A public family tree
* A replacement for WhatsApp
* A full wedding-planning SaaS
* A public photo-sharing network

The product should remain focused on:

**Family + Events + Celebrations + Memories.**

---

# 4. Technology Stack

## Frontend

**Next.js**

* App Router
* TypeScript
* React
* Server Components where appropriate
* Client Components for interactive UI
* Responsive design
* PWA support

### UI

* Tailwind CSS
* shadcn/ui or equivalent accessible component primitives
* Lucide icons
* Responsive layout system
* Dark/light themes

---

# 5. Backend

## Supabase

Use Supabase as the primary backend platform.

### Supabase services

**PostgreSQL**

* Core database
* Relationships
* Events
* RSVPs
* Family members
* Notifications
* Memories

**Supabase Auth**

* Authentication
* Sessions
* Password reset
* Email verification
* OAuth if enabled

**Supabase Storage**

* Event photos
* Profile photos
* Videos
* Documents
* Event attachments

**Supabase Realtime**

* RSVP changes
* New announcements
* Comments
* Event updates
* Family activity

**Edge Functions**

* Notification processing
* Invitation processing
* Scheduled tasks
* Email operations
* Secure server-side workflows

---

# 6. High-Level Architecture

```text
                    FAMILYHUB WEB APP
                           │
                           ▼
                    Next.js Frontend
                           │
             ┌─────────────┴─────────────┐
             │                           │
             ▼                           ▼
       Supabase Auth                Server Actions
             │                           │
             └─────────────┬─────────────┘
                           ▼
                    Supabase Backend
                           │
       ┌──────────┬────────┼────────┬──────────┐
       ▼          ▼        ▼        ▼          ▼
   PostgreSQL   Storage  Realtime  Edge     Auth
                                  Functions
```

---

# 7. Multi-Family Architecture

The platform should be designed as a **multi-tenant application** from day one.

Top-level entity:

```text
Family
```

Everything belongs to a family.

```text
Family
 ├── Members
 ├── Events
 ├── Invitations
 ├── Announcements
 ├── Memories
 ├── Photos
 └── Notifications
```

A user may eventually belong to multiple families.

For example:

```text
User
 ├── Senne Family
 └── Partner Family
```

Therefore, do not make `user_id` the primary ownership mechanism for family data.

Use:

```text
family_id
```

throughout the database.

---

# 8. User Roles

## Family Owner

The person who created the family.

Can:

* Manage family
* Invite members
* Remove members
* Create events
* Edit events
* Delete events
* Manage permissions
* Manage family settings
* Manage administrators

---

## Family Admin

Can:

* Create events
* Edit events
* Manage invitations
* Manage attendees
* Create announcements
* Moderate comments
* Manage family members

---

## Family Member

Can:

* View family events
* RSVP
* View family members
* Add memories
* Upload photos
* Comment
* Receive notifications
* Create events if family settings permit

---

## Event Organizer

A family member assigned to a specific event.

Can:

* Manage that event
* Manage event guests
* Edit event details
* Manage event photos
* Post event announcements

They do not receive global family administration privileges.

---

## Guest

A guest can be invited to a specific event without becoming a full family member.

Guest access should be limited to:

* Event details
* RSVP
* Event location
* Event schedule
* Event announcements

---

# 9. Authentication

## Supported authentication

MVP:

* Email + password
* Magic link

Future:

* Google
* Apple
* Phone authentication

---

# 10. Family Creation

After registration:

```text
Create your family
```

Fields:

* Family name
* Family photo
* Description
* Country
* Time zone

Example:

```text
Senne Family
Pretoria, South Africa
```

The creator becomes:

```text
Family Owner
```

---

# 11. Family Invitation System

Admins can invite family members.

Invitation methods:

### Email

```text
You've been invited to join
the Senne Family
```

### Invitation link

Example conceptual URL:

```text
familyhub.app/invite/XXXXXXXX
```

Invitation states:

```text
Pending
Accepted
Expired
Revoked
```

Invitation tokens must be:

* Random
* Non-guessable
* Expirable
* Single-use where appropriate

---

# 12. Main Navigation

Desktop:

```text
┌──────────────────────────────────────────────────────┐
│ FamilyHub                              🔔   👤       │
├──────────────────────────────────────────────────────┤
│                                                      │
│  Home     Calendar     Events     Family     Memories│
│                                                      │
└──────────────────────────────────────────────────────┘
```

Mobile:

```text
Home
Calendar
Events
Family
Memories
```

Floating:

```text
＋
```

for creating an event.

---

# 13. Home Dashboard

The home screen should answer:

> **What's happening with our family?**

### Sections

**Welcome**

```text
Good morning, Koketso
Senne Family
```

### Next event

Large card:

```text
💍

Thabo & Lerato
Wedding

12 December
14:00
Pretoria

23 days to go
```

### Upcoming

Horizontal/vertical cards:

```text
🎂 Mom's Birthday
24 Sep

🎓 Kabelo's Graduation
10 Oct

🎉 Family Reunion
22 Nov
```

### Birthdays

```text
Upcoming birthdays

24 Sep   Mom
28 Sep   Kabelo
30 Sep   Neo
```

### Family activity

```text
Sarah added 12 photos
John RSVP'd to Christmas
Mom created a birthday event
```

---

# 14. Calendar

The calendar is a major feature.

Views:

* Month
* Week
* Day
* Agenda

### Event color/category

The UI may visually distinguish event types.

Example:

```text
🎂 Birthday
💍 Wedding
🎓 Graduation
👶 Baby
🎉 Celebration
🍽 Gathering
✈️ Trip
🕯 Memorial
📅 Other
```

Avoid making the interface overly colorful. The category should remain secondary to the event itself.

---

# 15. Event Types

Built-in event taxonomy:

### Birthdays

* Birthday
* Milestone birthday

### Weddings

* Engagement
* Traditional ceremony
* Wedding
* Reception
* Anniversary

### Children

* Baby shower
* Gender reveal
* Baby arrival
* Baptism/christening
* First birthday

### Education

* Graduation
* Graduation party
* School achievement

### Family

* Family reunion
* Family meeting
* Family dinner
* Family gathering

### Celebrations

* Anniversary
* Promotion
* Achievement
* Retirement
* Welcome home
* Farewell

### Memorial

* Memorial
* Funeral
* Celebration of life

### Seasonal

* Christmas
* New Year
* Easter
* Family holiday

### Other

* Trip
* Picnic
* Party
* Custom event

---

# 16. Event Creation

The primary action:

```text
+ Create Event
```

### Step 1

```text
What are we celebrating?
```

Select event type.

### Step 2

```text
Event details
```

Fields:

* Event name
* Description
* Date
* Start time
* End time
* Time zone
* Cover image

### Step 3

```text
Location
```

Fields:

* Venue name
* Address
* City
* Province/state
* Country
* Coordinates
* Additional directions

### Step 4

```text
People
```

Choose:

* Family members
* Guests
* Everyone in family

### Step 5

```text
RSVP
```

Options:

```text
Required
Optional
Disabled
```

### Step 6

```text
Publish
```

---

# 17. Recurring Events

Support:

* Every year
* Every month
* Every week
* Custom recurrence

Birthdays should automatically recur annually.

For example:

```text
Mom's Birthday
Every 24 September
```

The system should generate occurrences without duplicating the underlying person record.

---

# 18. Event Page

Every event receives a dedicated page.

Example:

```text
┌────────────────────────────────────┐
│                                    │
│          EVENT COVER IMAGE         │
│                                    │
├────────────────────────────────────┤
│                                    │
│ Thabo & Lerato's Wedding           │
│                                    │
│ Saturday, 12 December 2026         │
│ 14:00                              │
│                                    │
│ 📍 Pretoria                         │
│                                    │
│ ────────────────────────────────── │
│                                    │
│ About                              │
│ Join us as we celebrate...         │
│                                    │
│ [ RSVP ]       [ Add to Calendar ]│
│                                    │
└────────────────────────────────────┘
```

---

# 19. Event Information

An event can contain:

### Overview

* Title
* Description
* Date
* Time
* Location

### Schedule

Example:

```text
14:00  Guest arrival
15:00  Ceremony
17:00  Photos
18:00  Reception
20:00  Dinner
```

### Dress code

```text
Formal
Traditional
Smart casual
Custom
```

### Important information

```text
Parking available
Children welcome
Bring identification
```

### Contact

Event organizer.

---

# 20. RSVP System

Supported responses:

```text
Going
Maybe
Can't attend
```

Optional:

```text
Number of adults
Number of children
Dietary requirements
Guest names
```

Event organizer dashboard:

```text
Going        62
Maybe         8
Not attending 11
Pending      23
```

---

# 21. Guest Management

Each event should have a guest list.

Example:

```text
Family
├── Going
├── Maybe
├── Can't attend
└── Not responded
```

Guest records should support:

* Name
* Email
* Phone
* Relationship
* RSVP
* Plus-one
* Notes

---

# 22. Event Announcements

Organizers can publish announcements.

Example:

> **Wedding Update**
>
> The ceremony will now begin at 15:30.
>
> Parking is available behind the venue.

Members receive a notification.

---

# 23. Comments

Events can have a lightweight conversation.

Example:

```text
Sarah:
Can't wait! ❤️

Kabelo:
Do we need to bring chairs?

Mom:
No, everything is arranged.
```

Admins can moderate/delete comments.

---

# 24. Family Directory

Main screen:

```text
Family

Senne Family

Parents
────────
Mom
Dad

Children
────────
Koketso
Kabelo
Neo

Extended Family
────────
...
```

Search:

```text
🔍 Search family
```

---

# 25. Family Profiles

Each member has:

```text
Profile photo

Name
Relationship

Birthday
Contact information

Upcoming events
Past events
Photos
```

Privacy controls should determine which information is visible.

---

# 26. Birthday System

Birthdays deserve their own experience.

### Birthday dashboard

```text
September

🎂 Mom
24 September

🎂 Kabelo
28 September

🎂 Neo
30 September
```

Countdown:

```text
Mom's birthday
6 days
```

Birthday reminders can automatically generate notifications.

---

# 27. Birthday Privacy

Users should be able to control:

* Show full birthday
* Show month/day only
* Hide birthday
* Show age
* Hide age

Never expose a person's birthday publicly by default.

---

# 28. Memories

This is the long-term archive.

Every event can contain:

```text
Photos
Videos
Messages
Stories
Documents
```

### Memory gallery

```text
Christmas 2025
[photo] [photo] [photo]

Mom's 50th
[photo] [photo] [photo]

Kabelo's Graduation
[photo] [photo] [photo]
```

---

# 29. Photo Uploads

Photos should be associated with:

```text
family_id
event_id
uploaded_by
```

Metadata:

* File path
* MIME type
* Width
* Height
* Size
* Upload timestamp
* Caption

Generate thumbnails for galleries.

Original files should remain available for authorized users.

---

# 30. Family Timeline

This becomes the historical layer.

Example:

```text
2026
│
├── 💍 March
│   Thabo & Lerato's Wedding
│
├── 👶 June
│   Naledi was born
│
├── 🎂 September
│   Mom's 55th Birthday
│
└── 🎄 December
    Family Christmas
```

Users can browse:

```text
2026
2025
2024
2023
...
```

---

# 31. Search

Global family search.

Search:

```text
Mom
Christmas
Wedding
Pretoria
2025
Graduation
```

Results can include:

* Events
* People
* Memories
* Announcements

---

# 32. Notifications

Notification types:

### Event reminder

```text
Wedding in 7 days
```

### Birthday

```text
Mom's birthday is tomorrow 🎂
```

### RSVP

```text
Sarah RSVP'd: Going
```

### Event update

```text
Wedding details were updated
```

### Announcement

```text
New announcement from the event organizer
```

### Photo

```text
John added 18 photos to Christmas 2025
```

---

# 33. Notification Preferences

Users can control:

```text
Email notifications
Push notifications
Birthday reminders
Event reminders
RSVP updates
Photo notifications
Announcements
```

---

# 34. PWA

The web application should be installable.

Desktop:

```text
Install FamilyHub
```

Mobile:

```text
Add to Home Screen
```

Support:

* App icon
* Splash screen
* Responsive UI
* Push notifications
* Offline shell

---

# 35. Offline Behavior

The app should gracefully handle temporary connectivity loss.

Cache:

* App shell
* Recently viewed events
* Family directory
* Calendar data

Offline users can:

* View recently loaded events
* View cached family information

Changes should sync when connectivity returns where practical.

---

# 36. Database Architecture

Core tables:

```text
profiles
families
family_members
family_invitations

events
event_types
event_attendees
event_guests
event_schedules
event_announcements

locations

memories
media
comments

notifications
notification_preferences

audit_logs
```

---

# 37. Core Database Relationships

```text
profiles
   │
   ├──── family_members ──── families
   │
   └──── events
              │
              ├──── event_attendees
              ├──── event_guests
              ├──── event_schedules
              ├──── event_announcements
              ├──── memories
              ├──── media
              └──── comments
```

---

# 38. `profiles`

```text
id
user_id
display_name
first_name
last_name
avatar_url
bio
birthday
birthday_visibility
age_visibility
created_at
updated_at
```

---

# 39. `families`

```text
id
name
description
avatar_url
country
timezone
created_by
created_at
updated_at
```

---

# 40. `family_members`

```text
id
family_id
profile_id
role
relationship
status
joined_at
created_at
```

Role:

```text
owner
admin
member
```

Status:

```text
active
pending
removed
```

---

# 41. `events`

```text
id
family_id
created_by
organizer_id

event_type_id

title
description

cover_image_url

start_at
end_at
timezone

location_id

rsvp_enabled
rsvp_deadline

visibility

recurrence_rule

created_at
updated_at
deleted_at
```

---

# 42. `event_attendees`

```text
id
event_id
profile_id

status
guest_count
children_count

dietary_requirements
notes

responded_at
created_at
updated_at
```

Status:

```text
pending
going
maybe
declined
```

---

# 43. `event_guests`

For non-family guests:

```text
id
event_id

name
email
phone

plus_one_allowed
plus_one_name

rsvp_status

created_at
updated_at
```

---

# 44. `locations`

```text
id
name
address
city
province
country

latitude
longitude

notes

created_at
updated_at
```

---

# 45. `memories`

```text
id
family_id
event_id
created_by

title
description
memory_date

created_at
updated_at
```

---

# 46. `media`

```text
id
family_id
event_id
memory_id
uploaded_by

storage_path
thumbnail_path

media_type
mime_type

width
height
file_size

caption

created_at
```

---

# 47. `comments`

```text
id
family_id
event_id
memory_id
author_id

content

created_at
updated_at
deleted_at
```

---

# 48. `notifications`

```text
id
user_id
family_id

type
title
body

entity_type
entity_id

read_at
created_at
```

---

# 49. Row Level Security

This is critical.

Supabase RLS should enforce:

> A user can only access family data for families where they are an active member.

Conceptually:

```text
user
 ↓
family_members
 ↓
family_id
 ↓
authorized data
```

Never rely exclusively on frontend checks.

---

# 50. Storage Security

Storage should also follow family permissions.

Example:

```text
family-media/
    family-id/
        events/
            event-id/
                originals/
                thumbnails/
```

Users must not be able to access another family's private media simply by guessing a storage path.

---

# 51. API / Server Layer

Use Next.js server-side functions for sensitive operations.

Examples:

```text
createEvent()
updateEvent()
deleteEvent()
inviteFamilyMember()
respondToEvent()
uploadEventMedia()
createAnnouncement()
```

The server verifies:

```text
authenticated user
        ↓
family membership
        ↓
permission
        ↓
operation
```

---

# 52. Event Lifecycle

```text
Draft
  ↓
Published
  ↓
Updated
  ↓
Completed
  ↓
Archived
```

Completed events remain accessible.

They become part of:

**Memories + Family Timeline**

rather than disappearing.

---

# 53. Event Deletion

Use soft deletion.

```text
deleted_at
```

rather than immediately destroying records.

Administrators can restore accidentally deleted events.

Permanent deletion can be a separate administrative operation.

---

# 54. Audit Log

Record important actions:

```text
User created event
User edited event
User deleted event
User invited member
User removed member
User changed RSVP
User uploaded media
Admin changed permissions
```

Useful for security and troubleshooting.

---

# 55. Security Requirements

Minimum:

* HTTPS
* Secure authentication
* Supabase RLS
* Secure storage policies
* Server-side authorization
* CSRF protection where applicable
* Rate limiting for sensitive operations
* Input validation
* File type validation
* File size limits
* Secure invitation tokens
* XSS protection
* No sensitive information in client logs

---

# 56. File Upload Security

Allowed initially:

```text
JPEG
PNG
WEBP
HEIC
MP4
MOV
PDF
```

Set limits such as:

```text
Photo: 20 MB
Video: 500 MB
Document: 25 MB
```

These limits can be configurable.

Images should be processed into:

```text
Original
Large
Medium
Thumbnail
```

---

# 57. Responsive Design

### Mobile

Primary target:

```text
390 × 844
```

Must work down to approximately:

```text
320px
```

### Tablet

```text
768px+
```

### Desktop

```text
1280px+
```

Desktop can use a sidebar.

Mobile should use bottom navigation.

---

# 58. Design Language

The visual identity should feel:

**Private, elegant, warm, modern.**

Avoid:

* Corporate dashboard appearance
* Excessive gradients
* Excessive glassmorphism
* Tiny text
* Overloaded calendars
* Too many colors

Use:

* Large typography
* Generous spacing
* Rounded cards
* Family photography
* Subtle motion
* Strong hierarchy

---

# 59. Dark Mode

Support both.

### Light

Warm neutral background.

### Dark

Near-black background with soft neutral surfaces.

Accent colors can be used for:

* Event type
* RSVP status
* Notifications
* Important actions

---

# 60. Accessibility

Target:

**WCAG 2.2 AA**

Requirements:

* Keyboard navigation
* Focus states
* Screen-reader labels
* Sufficient contrast
* Accessible forms
* Reduced motion support
* Proper semantic HTML
* Touch targets of appropriate size

---

# 61. Calendar Integration

Support:

### Google Calendar

Generate calendar event links/files.

### Apple Calendar

Generate `.ics`.

### Outlook

Generate `.ics`.

Universal:

```text
Add to Calendar
```

should work even without connecting an external calendar account.

---

# 62. Location Integration

Events can have:

```text
Venue
Full address
Coordinates
```

The UI:

```text
📍 Venue Name
123 Example Street
Pretoria
```

Action:

```text
Get Directions
```

This can open the user's preferred mapping service.

---

# 63. Event Sharing

An event can have a controlled share link.

Example:

```text
Share Event
```

Options:

```text
Family only
Invited guests
Anyone with link
```

Default:

**Family only**

Public sharing should never be the default.

---

# 64. Wedding Mode

A wedding can use the standard event engine but expose additional sections.

```text
Wedding
├── Overview
├── Schedule
├── Venue
├── RSVP
├── Dress Code
├── Important Information
├── Gift Information
├── Announcements
├── Photos
└── Memories
```

No separate wedding database is necessary.

---

# 65. Birthday Mode

Birthday events can automatically connect to a family member.

Example:

```text
Mom
Birthday: 24 September
```

The system can automatically surface:

```text
Mom's Birthday
24 September
```

every year.

---

# 66. Family Timeline Algorithm

The timeline should combine:

* Events
* Birthdays
* Family milestones
* Memories

Sort chronologically.

Example:

```text
2024
│
├── Wedding
├── Baby arrival
└── Christmas

2025
│
├── Graduation
├── Mom's birthday
└── Family reunion

2026
│
└── ...
```

---

# 67. Dashboard Intelligence

The home dashboard can automatically determine:

```text
Next event
Next birthday
Events this week
Pending RSVPs
Recent memories
Recent family activity
```

No AI is required for MVP.

Later, AI could help generate:

* Event descriptions
* Birthday messages
* Photo captions
* Event summaries

But this should be optional.

---

# 68. Admin Dashboard

Family admins get:

```text
Overview

Members        42
Upcoming Events 8
Pending RSVPs  17
Photos         1,284

Recent activity
...
```

Actions:

```text
Manage members
Manage invitations
Manage events
Manage media
Manage settings
```

---

# 69. Family Settings

```text
Family name
Family photo
Description
Timezone
Country

Member permissions
Birthday visibility
Default event visibility
Photo permissions
Comment permissions

Notifications
```

---

# 70. Event Permissions

Each event can configure:

```text
Who can see it?
Who can RSVP?
Who can comment?
Who can upload photos?
Who can edit?
```

Defaults should be family-friendly and simple.

---

# 71. Data Export

Family owners should eventually be able to export:

```text
Events
Family directory
RSVPs
Memories
Photos metadata
```

Possible formats:

```text
CSV
JSON
ICS
ZIP
```

---

# 72. Account Deletion

Users can request account deletion.

Family owners should be warned before deleting a family.

Deleting an account should not automatically destroy family events that belong to the family.

Ownership transfer should be supported.

---

# 73. Performance

Targets:

### Initial load

Aim for:

```text
<2 seconds
```

on a reasonable connection.

### Images

Always use:

* Responsive images
* Lazy loading
* Thumbnails
* CDN delivery

### Calendar

Avoid loading every historical event at once.

Load the required date range.

---

# 74. SEO

The application itself is private, so most pages should not be indexed.

Use:

```text
noindex
```

for authenticated family content.

Public event pages, if eventually enabled, can be selectively indexed.

---

# 75. Analytics

Keep analytics minimal.

Track product events such as:

```text
event_created
event_viewed
rsvp_submitted
memory_uploaded
invitation_accepted
```

Do not collect unnecessary personal information.

---

# 76. Error Handling

Every operation needs clear states.

Example:

```text
Saving...
Saved ✓
```

Failure:

```text
Couldn't save your changes.
Try again.
```

Upload:

```text
Uploading 12 photos...
8 of 12 complete
```

---

# 77. Empty States

Examples:

### No events

> Your family calendar is empty.
> Create your first celebration.

### No memories

> Your family story starts here.

### No upcoming birthdays

> No birthdays coming up this month.

Avoid generic:

> No data found.

---

# 78. Core Routes

Next.js route structure:

```text
/
 /login
 /signup
 /onboarding

 /app
 /app/calendar
 /app/events
 /app/events/[id]
 /app/family
 /app/family/[memberId]
 /app/birthdays
 /app/memories
 /app/timeline
 /app/notifications
 /app/settings

 /app/admin
 /app/admin/members
 /app/admin/events
 /app/admin/invitations
 /app/admin/settings

 /invite/[token]
```

---

# 79. Component Architecture

```text
components/
├── navigation/
├── calendar/
├── events/
│   ├── EventCard
│   ├── EventHeader
│   ├── EventDetails
│   ├── EventSchedule
│   ├── EventRSVP
│   └── EventGallery
├── family/
├── birthdays/
├── memories/
├── notifications/
├── forms/
└── ui/
```

---

# 80. Application Layers

```text
app/
components/
lib/
  auth/
  supabase/
  events/
  family/
  memories/
  notifications/
  calendar/
  validation/

server/
  actions/
  services/

types/
schemas/
hooks/
utils/
```

Use a clear separation between:

```text
UI
 ↓
Application logic
 ↓
Database/services
```

---

# 81. Validation

Use a schema validation library such as Zod.

Validate:

* Event names
* Dates
* Times
* IDs
* Emails
* Invitation tokens
* File metadata
* RSVP values

Never trust browser input.

---

# 82. Testing

## Unit tests

Test:

* Event validation
* Recurrence
* RSVP calculations
* Permissions
* Birthday calculations

## Integration tests

Test:

* Authentication
* Event creation
* Invitations
* RSVP
* Photo upload
* RLS

## End-to-end

Test flows:

```text
Sign up
 ↓
Create family
 ↓
Invite member
 ↓
Create event
 ↓
Invite guests
 ↓
RSVP
 ↓
Upload photos
 ↓
View memory
```

---

# 83. Database Migration Strategy

Use versioned migrations.

```text
001_initial_schema
002_family_members
003_events
004_rsvp
005_media
006_notifications
...
```

Never manually modify production schema without a migration.

---

# 84. Environment Variables

Conceptually:

```text
NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY
SUPABASE_SERVICE_ROLE_KEY
```

Never expose:

```text
SUPABASE_SERVICE_ROLE_KEY
```

to the browser.

---

# 85. Deployment

Recommended:

```text
GitHub
   ↓
Vercel
   ↓
Next.js

Supabase
   ↓
Database
Auth
Storage
Realtime
Edge Functions
```

This gives a very clean full-stack architecture without needing to maintain your own traditional server.

---

# 86. MVP

The first release should contain:

### Authentication

* Sign up
* Login
* Logout
* Password reset

### Family

* Create family
* Invite members
* Member profiles
* Roles

### Events

* Create event
* Edit event
* Delete event
* Event types
* Date/time
* Location
* Cover image

### Calendar

* Month
* Agenda
* Upcoming events

### RSVP

* Going
* Maybe
* Can't attend

### Birthdays

* Birthday profiles
* Birthday calendar
* Reminders

### Memories

* Event photos
* Gallery
* Timeline

### Notifications

* Event reminders
* Birthday reminders
* RSVP updates

### Security

* RLS
* Private storage
* Family isolation

---

# 87. Phase 2

Add:

* Comments
* Event announcements
* Event schedules
* Guest management
* Calendar integrations
* Push notifications
* Video uploads
* Advanced family timeline
* Event sharing links
* Admin analytics

---

# 88. Phase 3

Potential advanced features:

### Family Tree

```text
Grandparents
      │
 ┌────┴────┐
Parents   Aunts/Uncles
    │
 ┌──┴───────┐
Children   Cousins
```

### Family Stories

Long-form stories attached to people/events.

### AI Family Assistant

Example:

> "When is Grandma's birthday?"

> "What family events are happening next month?"

> "Show me photos from Christmas 2025."

### Automatic yearly recap

```text
2026 — Our Family Year

8 celebrations
4 birthdays
2 weddings
1 graduation
1,842 photos
```

That could become a beautiful annual family archive.

---

# 89. The Core Data Model

The most important architectural decision is this:

```text
                    FAMILY
                       │
        ┌──────────────┼──────────────┐
        │              │              │
     MEMBERS         EVENTS         MEMORIES
        │              │              │
        │         ┌────┼────┐         │
        │         │    │    │         │
        │       RSVP  MEDIA COMMENTS   │
        │                              │
        └──────────────┬───────────────┘
                       │
                  TIMELINE
```

Everything ultimately feeds the **family timeline**.

That gives the app a much stronger identity than simply being another calendar.

---

# 90. Final Product Structure

The finished application should feel like:

```text
                         FAMILYHUB
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
       HOME              CALENDAR             EVENTS
        │                   │                   │
        ├── Upcoming        ├── Month          ├── Weddings
        ├── Birthdays       ├── Week           ├── Birthdays
        ├── Activity        └── Agenda          ├── Graduations
        └── Next event                           └── Celebrations
                            │
        ┌───────────────────┴───────────────────┐
        │                                       │
      FAMILY                                  MEMORIES
        │                                       │
        ├── Members                             ├── Photos
        ├── Profiles                            ├── Videos
        ├── Birthdays                           ├── Stories
        └── Relationships                       └── Timeline
```

## The key architectural rule

**Do not build separate systems for weddings, birthdays, graduations, Christmas, etc.**

Build **one powerful Event Engine** with different event types and optional modules.

That gives you:

```text
Event Engine
     │
     ├── Birthday
     ├── Wedding
     ├── Anniversary
     ├── Graduation
     ├── Baby Shower
     ├── Family Reunion
     ├── Memorial
     ├── Holiday
     ├── Trip
     └── Custom
```

Then the same underlying infrastructure handles **RSVPs, guests, locations, schedules, announcements, photos, comments, notifications and memories**.

That is the foundation I'd use for the actual full-stack build.

