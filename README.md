# Send Job Email: a Claude skill

A Claude skill that turns a pile of Gmail drafts into personalised job applications, sent from your own saved Gmail template, with every link left clean.

## The problem it solves

When an AI tool creates or sends email through the Gmail API, Gmail rewrites every link into a `google.com/url` redirect. Your portfolio, LinkedIn and resume links end up wrapped, and some expire. This skill does all editing and sending inside Gmail's own compose window instead, so links stay exactly as you wrote them.

## What it does

1. Reads all your drafts and works out who each one goes to.
2. Merges drafts to the same company into one email, with extra addresses in CC.
3. Finds the recipient's first name and the company's real brand name from the address.
4. Clears the placeholder text and inserts your saved Gmail template.
5. Personalises the greeting, company line and subject.
6. Switches to a freelance pitch when the draft says freelance.
7. Sends each email, waits for Gmail's "Message sent" confirmation, and reports back.

## Setup

1. In Gmail, save your application email as a template (Settings, Advanced, Templates).
2. Open `SKILL.md` and fill in the Setup section: your template name, default subject and signature.
3. In each draft, just add a recipient and type "Hi". The skill does the rest.

## Install

- **Claude Code:** copy the `send-job-email` folder into `~/.claude/skills/`.
- **Claude app:** zip the folder and upload it in the Skills section of Claude's settings.

Then say "send my job emails".

## Needs

Claude with browser access to Gmail in Chrome, and read access to Gmail drafts.
