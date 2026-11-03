---
name: "send-job-email"
description: "Send personalised job application emails from Gmail drafts using your saved Gmail application template, without breaking any links. Use when the user says send job emails, send my drafts, or clear the application drafts."
---

# Send job application emails from Gmail drafts

Work in the user's Gmail account, in Chrome.

## Setup (fill these in once)

- **Template name:** the name of your saved Gmail template, for example "Application for Product Designer Position"
- **Default subject:** the subject to use when no specific position is given
- **Signature:** your Gmail signature, which this skill keeps untouched

Wherever this file says "the template", it means your template above.

## The one rule that matters

Everything happens in the Gmail interface, through the saved template. Never rewrite a draft body through the Gmail API (update_draft), never send through the API, and never hand build the application HTML. The Gmail API rewrites every link into an expiring google.com/url redirect, which breaks the portfolio, LinkedIn and resume links in your email. Editing the open compose window in the browser (clicks, or page JavaScript on that compose) keeps links clean. Reading drafts through the API (list_drafts with DRAFT_VIEW_FULL, get_draft) is fine and is the fastest way to see every recipient.

The only things that change per email: the greeting, the company name, the recipient, the subject if a specific position is given, and the freelance copy for freelance drafts.

## What a typical draft looks like

Drafts usually hold just "Hi" (sometimes "H" or "hi") followed by the Gmail signature. The subject is usually empty, or a short note like "Freelance designer". That "Hi" line is only a placeholder: remove it and everything above the signature, then insert the template. The user does not need to say this each time.

## Step 1: List drafts

Use list_drafts with view DRAFT_VIEW_FULL (page through every page) to get each draft's To, CC, subject and the text above the signature. Skip drafts with no recipient (for example a copy of the template itself) or that clearly are not applications, and mention them at the end.

Work one draft at a time. Open each draft through a search for its own address, https://mail.google.com/mail/u/0/#search/in%3Adraft+to%3A<address>, never by position in the list, and confirm the open compose's To matches before editing.

## Step 2: Merge drafts for the same company

Group by address and company domain (subdomains count, like jobs@careers.acme.com and rahul@acme.com). When two or more drafts go to the same company, send one email:
1. Named person's address in To (if both named or both generic, the earlier draft).
2. Other address(es) in CC on that email.
3. Greeting follows the To person.
4. Leftover duplicates are not sent. Claude cannot delete drafts, so list them at the end for the user to discard (open the draft, click the bin icon).

Companies emailed in earlier batches are still sent, never skipped.

## Step 3: Name and company

Recipient name:
1. If the address or a note names a person (priya@swiggy.in, priya.sharma@company.com, rahul.k@, divya4.sharma@), use the capitalised first name: "Dear Priya,". No surname unless the note gives the full name, no titles.
2. Generic addresses (hr@, careers@, jobs@, talent@, recruitment@, hiring@, hirings@, info@, hello@, people@, ceooffice@) or an unreadable run of letters get "Dear HR Team,".

Company name:
1. From the domain or the user's note, in real brand casing: careers@indmoney.com becomes INDmoney.
2. Personal mail domains (gmail.com, yahoo.com, outlook.com, hotmail.com) with no note get "your company".

Never fall back to Dear HR Team or your company when the name or company is clearly visible.

## Step 4: Clear the placeholder and insert the template

1. Open the draft. In the body, delete every node above the signature block (the div with data-smartmail="gmail_signature"), leaving one empty line above it, and put the caret on that line. If the body holds an older version of the application text, clear that too. Do not delete the signature.
2. More options (three dots, bottom right of compose) > Templates > under Insert template choose your template. The Templates item is greyed out until the body has focus. The template replaces body and subject and can leave the signature block empty.
3. Make sure exactly one full signature remains at the end. If it came out empty, rebuild it exactly as it appears in the user's other drafts, inside the compose window. Gmail blocks raw innerHTML (Trusted Types), so create a trustedTypes policy first when inserting HTML with JavaScript.

Do not add any sign off, name or closing line of your own.

Before the first send of a run, read the inserted template once and check it against the user's current situation (current employer, tenure, title, metrics). If anything is out of date, stop and flag it before sending anything.

## Step 5: Personalise

1. Greeting: "Dear HR Team," becomes the Step 3 greeting (keep bold).
2. Company line: "roles at Company" becomes "roles at <Company>" or "roles at your company" (keep bold).
3. Recipients: confirm To and any CC from Step 2.
4. Subject: your default subject unless a specific position is given, then "Application for <that position>". Every subject starts with Application.

Change nothing else in the body, except for freelance drafts below.

### Freelance drafts

When the draft's subject or note says freelance (Freelance designer, Freelancing, Freelance Figma and similar), pitch it as freelance work. Keep the template's bullets, links and signature, and change only:
1. The opening line to: "I'd love to help <Company> on a freelance basis, for a one off project or ongoing design support. I take on landing pages, store and product page UX audits, PDP and homepage redesigns, and app or web product design, all in Figma. Here's what I'd bring:" (company name and "freelance" in bold).
2. The closing ask to: "Do you have any freelance design work coming up? If this isn't the right inbox, a pointer to the right person would mean a lot."
3. The subject to "Application for Freelance <role from the note>", for example Application for Freelance Figma Designer, or Application for Freelance Designer.

## Step 6: Send

Click Send (the div with role button whose aria-label starts with "Send"; clicking by coordinate can minimise the compose instead) and wait for the "Message sent" toast before the next draft.

If the user has said not to ask for confirmation, invoking this skill is the go ahead: work through the whole batch without pausing. Stop only for an out of date template fact, a draft whose To does not match the search, or a draft whose recipient or company cannot be worked out.

For big batches it is fine to script the per draft loop in the page (open by search, check To, clear above signature, insert template through the menu, personalise, check signature, send, confirm toast, stop on any failure) and check progress periodically.

## Step 7: Report

Brief summary: how many sent, which went as freelance pitches, which duplicate drafts the user should discard, and any skipped drafts and why.

## Rules

1. Edit and send only in the browser compose window. Never update or send drafts through the Gmail API.
2. Always remove the "Hi" placeholder and anything above the signature before inserting the template.
3. Exactly one signature, unchanged, at the end. No extra sign off or name.
4. "Dear HR Team," only when no name can be found; "your company" only when no company can be found.
5. Freelance drafts get the freelance opening, ask and subject; everything else follows the template.
6. One email per company; extra addresses go in CC.
7. Every subject starts with "Application".
8. Only skip confirmation before sending if the user has said so.
