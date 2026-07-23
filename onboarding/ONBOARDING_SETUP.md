# New Hire Paperwork Tool — Setup & Fine-Tuning Guide

## What this is

A standalone web app: an applicant answers one multi-step questionnaire,
and the tool fills in the matching fields across all 13 fillable documents
from your new-employee packet, then hands them a single .zip of completed
PDFs to review and sign. Nothing is uploaded anywhere — the filling happens
entirely in the browser using the PDF assets bundled alongside it.

## Folder structure

```
onboarding/
  index.html          <- the app itself
  assets/
    background_check.pdf
    w4.pdf
    i9.pdf
    contract.pdf
    health_eval.pdf
    zamp_new_employee_info.pdf
    direct_deposit.pdf
    zamp_acknowledgment.pdf
    arbitration.pdf
    zamp_intro.pdf
    econsent_medical.pdf
    w2_econsent.pdf
    handbook.pdf
    health_insurance.pdf      (reference only, nothing pre-filled)
    abuse_ack.pdf              (reference only, nothing pre-filled)
    welcome_and_checklist.pdf  (reference only, nothing pre-filled)
```

Keep this whole `onboarding` folder together — the PDFs are fetched by
relative path from `index.html`, so if you rename or move one, update the
`file:` path in the `DOCS` config near the top of `index.html`'s `<script>`.

## Deploying it

Same idea as the first aid course: drop the `onboarding` folder into your
GitHub repo (as a subfolder, e.g. `yourrepo/onboarding/`) and it'll be live
at `https://your-username.github.io/yourrepo/onboarding/` once GitHub Pages
is turned on for the repo. No build step, no server.

## What got filled vs. left blank, and why

- **Filled**: name, address, SSN, DOB, phone, email, job details, W-4 Step 1,
  I-9 Section 1, health screening info, banking details, and similar
  informational fields, everywhere they appear across the packet.
- **Left blank on purpose**: every signature line and "date signed" line,
  across all 13 documents. Signing is a distinct legal moment from filling
  in information, and I didn't want a form to imply an auto-generated date
  represents when someone actually signed. Print-and-sign or route through
  a separate e-sign step, same as your process today.
- **Left blank on purpose**: I-9 Section 2 (the section you or your admin
  complete after physically examining ID documents) and the W-4's Steps
  2(b)/4(b) worksheets (multiple-jobs and itemized-deduction calculations
  that need the employee's own numbers and review).

## Fine-tuning field positions

Every field's position lives in the `DOCS` array near the top of the
`<script>` section in `index.html`. Each entry looks like:

```js
{page:0, x:117, bottom:159.7, size:9, key:"firstName"},
```

- `page`: which page of that PDF (0-indexed)
- `x`: distance from the left edge, in points (72 points = 1 inch)
- `bottom`: distance from the *top* of the page down to where the text
  baseline sits, in points
- `size`: font size
- `key`: which intake-form answer to print there

To nudge something: increase `x` to move text right, increase `bottom` to
move it further down the page. Small adjustments (2–5 points) are usually
enough.

**Worth a visual check before rolling this out to your team:** I positioned
every field using the documents' actual text coordinates (extracted
programmatically), which is accurate for the vast majority of fields. Two
things are worth a closer look:

1. **The Zamp HR New Employee Information Form** (`zamp_new_employee_info.pdf`)
   was the one document in the packet that had been flattened into a plain
   image with no underlying text — so its coordinates are estimated from a
   rendered image rather than measured exactly like the others. It's flagged
   with a `note` field in the config and is the one I'd proofread first.
2. **Form W-4 and Form I-9** have the most fields packed into a small area
   (checkboxes, small print). Worth generating one test packet and eyeballing
   those two pages specifically.

The easiest way to check everything: fill in a test entry (use fake data
like "Test Testerson"), generate the zip, and open each PDF. Anything that's
sitting a little high, low, or off to the side is a one-line coordinate edit.

## Editing the questions themselves

The questionnaire is the `STEPS` array, just above `DOCS`. Each step is a
section (e.g. "Personal Information") with a list of fields. Adding a new
question means adding an entry there *and* adding a matching overlay entry
in `DOCS` wherever that answer should appear on a PDF.

## A couple of process notes

- **Zamp HR's forms**: this tool auto-fills the Zamp-branded documents in
  your packet (new employee info, direct deposit, arbitration agreement,
  handbook acknowledgment, etc.). Since those are Zamp's forms, it's worth
  a quick check with them that this fits how they want documents submitted,
  versus routing new hires through their own Employee Self Service Portal.
- **I-9 timing**: Section 1 must be signed no earlier than accepting the
  job offer and no later than the first day of employment; Section 2 is due
  within 3 business days after that and requires you (or your admin) to
  examine the applicant's ID documents. This tool doesn't change or enforce
  that timeline — it just gets the paperwork ready faster.
- **The Contract of Employment's $5,000 clause and the Mutual Arbitration
  Agreement** are legal terms worth an employment attorney's sanity check
  independent of anything here — this tool only fills in blanks, it doesn't
  touch the legal substance of what's being signed.
