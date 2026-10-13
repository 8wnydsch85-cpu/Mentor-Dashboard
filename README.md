# Mentor Ledger

A private website for running a Hizmet talebe group. The site has two parts:

- **The mentor dashboard:** the weekly programme, tecvid course, çetele review and settings.
- **A çetele for each student:** every student fills in their own.

The site is hosted free on GitHub Pages. The data lives in a free Supabase project.

Who can see what:

- **The mentor** sees everything.
- **Each student** sees only their own çetele.
- **Anyone else** sees only a sign-in screen. Only emails you invite can sign in.

---

## What is in this folder

| File | What it is |
|---|---|
| `index.html` | The whole website. |
| `supabase-setup.sql` | Creates the database, its security rules, and your existing data. Run it once. |
| `README.md` | These instructions. |

---

## Setup (about 20 minutes)

### 1. Create the Supabase project

1. Go to supabase.com, sign in, and choose **New project**. Any name and region will do; save the database password somewhere safe.
2. Wait for the project to finish setting up, which takes about two minutes.

### 2. Create the database

1. In the project, open **SQL Editor** and choose **New query**.
2. Open `supabase-setup.sql`. Find the line marked `<-- YOUR EMAIL` and replace `you@example.com` with the email you will sign in with.
3. Paste the whole file into the editor and choose **Run**. It should finish with "Success. No rows returned".

### 3. Lock sign-in to invited people only

1. Open **Authentication**, then **Sign In / Providers**.
2. Make sure **Email** is enabled.
3. Turn **off** "Allow new users to sign up". Invited people can still join; strangers cannot.

### 4. Copy your project keys into the site

1. Open **Project Settings**, then **API** (on newer projects: **API Keys** and **Data API**).
2. Copy the **Project URL** and the **anon / public** key.
3. Open `index.html` in a text editor. Near the top of the second `<script>` block you will find:
   ```js
   const SUPABASE_URL = "https://YOUR-PROJECT-REF.supabase.co";
   const SUPABASE_ANON_KEY = "YOUR-ANON-PUBLIC-KEY";
   ```
   Paste your two values between the quotes and save the file.

The anon key is designed to be public; the security rules from step 2 protect the data. **Never** paste the `service_role` key into the site.

### 5. Put the site on GitHub Pages

1. On github.com, create a new repository, for example `mentor-ledger`. Public is fine, because the repository holds no student data.
2. Choose **Add file**, then **Upload files**, and upload `index.html` and `README.md`. Do **not** upload the SQL file, since it contains your students' names.
3. Open **Settings**, then **Pages**. Under "Build and deployment", choose **Deploy from a branch**, branch `main`, folder `/ (root)`, and save.
4. After a minute the address appears at the top, for example `https://yourname.github.io/mentor-ledger/`.

### 6. Tell Supabase where the site lives

1. Open **Authentication**, then **URL Configuration**.
2. Set **Site URL** to your GitHub Pages address, ending in `/`.
3. Add the same address under **Redirect URLs**.

### 7. Invite yourself, then your students

1. Open **Authentication**, then **Users**, then **Invite user**. Enter **your own** email first: the one you put in the SQL file.
2. Open the invitation email. You will land on the dashboard.
3. Invite each student the same way. When they open their invitation, they type their name once and their çetele appears in your Çetele tab.

After the first time, anyone can sign in from the site by entering their email and clicking the link that arrives.

---

## Updating the site later

When Claude gives you a new `index.html`:

1. Paste your `SUPABASE_URL` and `SUPABASE_ANON_KEY` into it again.
2. Upload it to the repository, replacing the old one.

GitHub Pages updates within a minute. Your data is not affected, because it lives in Supabase.

## Good to know

- **Email limits.** Supabase's built-in email sender allows only a small number of sign-in emails per hour. That is enough for a group of five. If students report links not arriving, wait an hour, or connect your own email sender under **Authentication**, then **Emails**, then **SMTP Settings**.
- **Adding another mentor.** Add their email in the SQL Editor:
  `insert into public.mentors (email) values ('their@email.com');`
  Then invite them.
- **Removing a student.** Delete them under **Authentication**, then **Users**. Their çetele stays in the database until you delete those rows in the **Table Editor**: every row in `docs` whose `path` starts with `cetele/` followed by their user id.
- **Pausing.** Free Supabase projects pause after a week with no activity. If the site stops loading, open the project in Supabase and choose **Restore**.
