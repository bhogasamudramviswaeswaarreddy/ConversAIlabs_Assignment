
📝 Supabase Notes Mini–Project

A minimal backend for a personal note-taking app built with Supabase Edge Functions.

---------------------------------------------------

 Schema Design

CREATE TABLE notes (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id UUID NOT NULL,
  title TEXT,
  content TEXT NOT NULL,
  created_at TIMESTAMPTZ DEFAULT now(),
  updated_at TIMESTAMPTZ DEFAULT now()
);

Why?
- UUID: scalable, secure, and globally unique.
- user_id: enforces per-user data ownership.
- title: optional, allowing flexibility for drafts or short notes.
- content: required — the essential part of a note.
- created_at, updated_at: useful for sorting and future auditing/versioning.

---------------------------------------------------

⚙️ Setup & Deployment Steps

1. Create a Supabase project at https://app.supabase.com
2. Run schema.sql in the SQL Editor to create the notes table.
3. Install Supabase CLI if not installed:
   npm install -g supabase
4. Link your project:
   supabase login
   supabase link --project-ref <your-project-ref>
5. Deploy edge functions:
   supabase functions deploy post_notes
   supabase functions deploy get_notes
6. Set environment variables in the dashboard or CLI:
   - SUPABASE_URL
   - SUPABASE_ANON_KEY

---------------------------------------------------

🔌 Edge Functions

post_notes.ts
// POST /notes — because we are creating a resource.
// Reads user input from the JSON body.

get_notes.ts
// GET /notes — because we are retrieving data for the current user.
// Auth token read from Authorization header.

---------------------------------------------------

 Demo Commands

Create a Note

curl -X POST https://<your-project-ref>.functions.supabase.co/notes \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"title": "Team Sync", "content": "Discussed launch roadmap."}'

Expected Response:
[
  {
    "id": "uuid-value",
    "user_id": "uuid-user",
    "title": "Team Sync",
    "content": "Discussed launch roadmap.",
    "created_at": "2025-05-06T12:00:00Z",
    "updated_at": "2025-05-06T12:00:00Z"
  }
]

List Notes

curl -X GET https://<your-project-ref>.functions.supabase.co/notes \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"

Expected Response:
[
  {
    "id": "uuid-value",
    "user_id": "uuid-user",
    "title": "Team Sync",
    "content": "Discussed launch roadmap.",
    "created_at": "2025-05-06T12:00:00Z",
    "updated_at": "2025-05-06T12:00:00Z"
  }
]

---------------------------------------------------

📁 Project Structure

ConversAIlabs_Assignment/
  ├── functions/
  │   ├── post_notes.ts
  │   └── get_notes.ts
  ├── schema.sql
  └── README.md

---------------------------------------------------

🚀 Summary

This Supabase-powered notes service demonstrates:
- Thoughtful schema design
- Secure, user-scoped data
- Clear API endpoints
- Practical, testable output

Built in 2 hours to showcase backend clarity and cloud-native design.
