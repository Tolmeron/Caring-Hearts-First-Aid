# Setting Up the New Hire Paperwork Tool

This walks through getting the onboarding tool live at a shareable web
address, the same way we did for the first aid course. ~10 minutes, all
through the browser, no coding required.

---

## Option A — Add it to your existing repo (recommended)

If you already have the `first-aid-course` repo from before, the easiest
path is to add this as a second page in the same repo, at its own URL.

1. Unzip `onboarding-tool.zip` on your computer. You should see:
   ```
   onboarding-tool/
     index.html
     assets/            (16 PDF files inside)
     ONBOARDING_SETUP.md
   ```
2. Go to your repo on github.com.
3. Click **Add file → Upload files**.
4. Rename the folder `onboarding-tool` to just **`onboarding`** on your
   computer first, then drag the *whole `onboarding` folder* (not just the
   files inside it) into the upload box. GitHub preserves the folder
   structure, so `index.html` and `assets/` will land together at
   `onboarding/` inside your repo. (`ONBOARDING_SETUP.md` is reference for
   you — fine to include it or leave it out, it won't affect the live tool.)
5. Commit the change.
6. Wait 1–2 minutes, then visit:
   `https://your-username.github.io/first-aid-course/onboarding/`

That's it — GitHub Pages automatically serves the new folder once it's
committed, no extra settings needed since Pages is already turned on for
this repo.

## Option B — Its own separate repo

If you'd rather keep onboarding paperwork completely separate from the
first aid course (different audience, maybe different access later):

1. Create a new repository, e.g. `new-hire-paperwork` (Settings → Pages →
   same steps as before: Deploy from a branch → `main` → `/ (root)`).
2. Upload the *contents* of the `onboarding-tool` folder — `index.html` and
   the `assets` folder — directly to the repo root this time (not inside
   an `onboarding` subfolder, since this repo is dedicated to just this
   tool).
3. Your live link will be:
   `https://your-username.github.io/new-hire-paperwork/`

Either option works fine — Option A is quicker if the repo already exists;
Option B is cleaner if you want this tool to have its own dedicated link
and eventual access controls, separate from the training course.

---

## After it's live: test before sending to real applicants

1. Open your live link and fill out the questionnaire once with fake data
   (e.g. "Test Testerson").
2. Click through to generate and download the .zip.
3. Open each PDF inside and skim for anything sitting slightly off —
   especially the **Zamp HR New Employee Information Form**, **W-4**, and
   **I-9**, which `ONBOARDING_SETUP.md` flags as the ones most worth a
   careful look.
4. Any field that's a little high, low, or off to the side is a one-line
   coordinate edit in `index.html` — `ONBOARDING_SETUP.md` shows exactly
   how.

## Making future edits

Same as the first aid course: click the file in GitHub, click the pencil
✏️ to edit, make your change, commit. The live site updates within a
minute or two.

## Security note (same caveat as before)

This tool never sends applicant data anywhere — everything happens in the
browser and the completed PDFs go straight to a local download, not to any
server. That was a deliberate design choice given the sensitive fields
involved (SSN, banking info, etc.). The one thing to keep in mind: because
it's a public link once deployed, anyone with the URL can open it and fill
it out. If that's a concern, consider Option B with a repo/hosting setup
that supports access restriction, or simply don't publicize the link beyond
the people who need it — there's no roster or password gate on this tool
the way there is on the first aid course, since there's no data being
collected centrally for one to protect.
