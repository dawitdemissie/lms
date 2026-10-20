# Dawit Demissie · Learning Platform

A complete course website you host for free on **GitHub Pages**:
course materials, announcements, assignments, timed exams, private
messages with students, and a gradebook.

The site works in two modes:

| Mode | When | Where data is kept |
|---|---|---|
| **Demo** | `config.js` is left empty (as delivered) | Only in the visitor's own browser. A "Viewing as" menu switches between instructor and student. Good for trying it out. |
| **Live** | You paste your Supabase settings into `config.js` | In your free Supabase database, with real sign-in and security rules. Use this with real students. |

---

## Files

| File | What it is |
|---|---|
| `index.html` | The whole website |
| `config.js` | Your settings. Empty = demo mode |
| `logo-light.png`, `logo-dark.png`, `favicon.svg` | Your logo and browser-tab icon |
| `supabase/setup.sql` | Creates the database, security rules and file storage (live mode) |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |

---

## Part 1 · Put it on GitHub Pages (about 5 minutes)

1. Sign in at **github.com** → **+** (top right) → **New repository**.
   Name it, for example, `learn`. Choose **Public**, then **Create repository**.
2. Click **uploading an existing file**. Drag in **everything inside this folder**
   (including the `supabase` folder), then **Commit changes**.
3. Open **Settings → Pages**. Under *Build and deployment* choose
   **Deploy from a branch**, branch **main**, folder **/ (root)**, then **Save**.
4. After a minute or two your site is live at
   `https://YOUR-USERNAME.github.io/learn/`.

The site now runs in **demo mode** with a sample course. Click around as the
instructor, then use **Viewing as** to see what students see.

---

## Part 2 · Go live with real students (about 15 minutes)

### 2.1 Create a free Supabase project
1. Go to **supabase.com** → **Start your project**, and sign in (you can use GitHub).
2. **New project**: give it a name, set a database password (save it somewhere),
   pick the region closest to your students, then **Create new project**.
   Wait about a minute while it sets up.

### 2.2 Create the database
1. Open `supabase/setup.sql` in a text editor (Notepad is fine).
2. Near the top, replace `you@example.com` with **the email you will sign in with**.
   You can list several instructors:
   `('you@example.com'), ('colleague@school.edu')`
3. In Supabase, open **SQL Editor** → **New query**. Paste the **whole file** and click **Run**.
   You should see *Success. No rows returned*.

### 2.3 Choose how people sign in
In Supabase open **Authentication → Sign In / Providers → Email** (the wording
may vary slightly):

- **Recommended for classes: turn OFF "Confirm email."** Students can then create
  an account with a password straight away, and no email is needed.
  Supabase's free built-in email service sends only a few emails per hour, so
  with confirmation ON, a whole class signing up on day one would get stuck.
- If you want confirmation emails or "Email link" sign-in for everyone, set up
  your own email sender under **Project Settings → Authentication → SMTP**.
  Free options include Resend and Brevo.

### 2.4 Tell Supabase your site's address
**Authentication → URL Configuration**:
- **Site URL:** `https://YOUR-USERNAME.github.io/learn/`
- **Redirect URLs:** add the same address.

This makes email links (password reset, email sign-in) come back to your site.

### 2.5 Connect the website
1. In Supabase open **Project Settings → API** (or **Data API**). Copy the
   **Project URL** and the **anon public** key.
   The anon key is meant to be public; your data is protected by the security
   rules from `setup.sql`. Never use the `service_role` key here.
2. On GitHub, open `config.js` → the pencil icon (Edit) and fill in:
   ```js
   window.LMS_CONFIG = {
     supabaseUrl: "https://abcdefghijklmnop.supabase.co",
     supabaseAnonKey: "eyJhbGciOi..."
   };
   ```
3. Click **Commit changes**. A minute later the site shows a sign-in page.

### 2.6 First sign-in
1. Open your site → **Create account** using **the instructor email from step 2.2**.
   You'll see the instructor menu (People, Course settings).
2. Open **Course settings** and enter your course title, code and term.
3. Share the site link with students. They choose **Create account** and appear
   under **People** after signing up.

> Signed up before adding your email to `setup.sql`? Add it, then run
> `setup.sql` again. It is safe to re-run and will promote your account.

---

## What students and instructors can do

**Instructor:** post and pin announcements · organise materials into modules
(notes, links, files) · create assignments with due dates and points · see who
has submitted, download work, grade with feedback · build exams (multiple choice,
true/false, short answer, essay) with time limits and open/close times · mark
essays and release results · message any student · gradebook with CSV export.

**Student:** see what's coming up · read materials · submit text or files
(and resubmit until graded) · take timed exams (answers are saved as they type;
time's-up submits automatically) · see grades and feedback · message the instructor.

## Security (live mode)

The rules in `setup.sql` are enforced by the database itself, so they apply even
if someone pokes at the site with browser tools:
- Students see only their own submissions, exam attempts, scores and messages.
- Exam **answer keys** are never sent to students; exam questions appear only
  after a student presses Start; exams are scored inside the database.
- Students can't grade themselves, change a graded submission, take an exam twice,
  or make themselves instructors.
- Uploaded files are private; each student can only reach their own uploads.

## Good to know
- **Limits:** files up to 25 MB (the Supabase free plan includes 1 GB of storage
  and a 500 MB database, plenty for a course). Free projects pause after a week
  with no visits; open the Supabase dashboard to wake one up.
- **Several courses:** each copy of the site plus its own Supabase project is one course.
- **Backups:** Supabase → **Table Editor** lets you export any table as CSV,
  and the Gradebook page exports all grades.
- **Updating the site later:** upload a new `index.html` to GitHub. Your
  `config.js` and all course data stay as they are.
