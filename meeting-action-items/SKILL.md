---
name: meeting-action-items
description: Turn messy meeting notes into a clean action-item table with owner, due date, status and a source quote, plus open questions and a ready-to-send follow-up message. Use whenever the user pastes or points to meeting notes, a call transcript, a sync or stand-up summary, or asks "what are the action items", "who does what", "summarize this meeting" or "write the follow-up", even if they don't say "action items".
---

# Meeting action items

People paste meeting notes because they want to know one thing: who is doing what, by when. Notes are usually fragmentary, out of order, and full of commitments that were never stated cleanly ("I can probably look at that", "let's circle back Thursday"). The job is to pull those commitments out, make them unambiguous, and give the user something they can send to the group right away.

## Inputs

The notes may be pasted in the chat, attached as a file (txt, md, docx, pdf, vtt/srt transcript), or described as a path. Read attached files before answering. If the notes are in another language, write the output in that language, and keep quotes in the original wording.

If no meeting date is given, use the date the user is writing on to resolve relative deadlines ("by Friday", "next week") and state the date you assumed in one short line, since a wrong anchor silently shifts every due date.

If the meeting happened days ago and a derived due date is already behind today, keep the date but add "(past)" after it, so the user notices items that may already be overdue and can ask the owner for status.

## What to produce

Always produce these three parts, in this order, and keep the whole response scannable.

### 1. Action items

A table with exactly these columns:

| # | Action | Owner | Due | Status | Source quote |
|---|--------|-------|-----|--------|--------------|

- **Action**: a verb-first, self-contained sentence ("Send revised pricing to Acme"), understandable by someone who wasn't in the meeting.
- **Owner**: a person's name as it appears in the notes. If several people share it, list them. If the notes imply but never state an owner, write the best guess followed by "(?)". If nobody is plausible, write "Unassigned". Do not invent owners to make the table look complete: an invented owner is worse than a visible gap, because the gap gets resolved and the invention gets acted on.
- **Due**: an absolute date (e.g. "Fri 23 Oct") when it can be derived, otherwise the phrase from the notes ("after launch"), otherwise "No date". Convert relative dates using the meeting date.
- **Status**: one of `New`, `In progress`, `Blocked`, `Done`. Default to `New`. Use the others only when the notes say so ("already sent it", "waiting on legal").
- **Source quote**: a short verbatim fragment (under about 15 words) from the notes that justifies the row, so the user can verify it quickly. Never paraphrase inside the quote marks.

What counts as an action item: a commitment, an assignment, or a clear next step someone must take. What doesn't: background discussion, opinions, decisions with no follow-up work. Decisions that were made belong in the short "Decisions" line below the table, not in it.

Order rows by due date (earliest first), with undated and unassigned items at the bottom. Merge duplicates, where the same task is mentioned twice.

### 2. Open questions

A short bullet list of things the notes leave unresolved and that block or risk an action item: missing owners, missing dates, conflicting statements, unanswered questions asked in the meeting. Phrase each as a question to a named person when possible ("Anna - who owns the vendor contract review?"). If nothing is unclear, say "None" rather than padding.

### 3. Follow-up message

A ready-to-send message to the attendees. Default to email format with a subject line; if the user mentions Slack, Teams or chat, make it a short chat message instead. It should:

- open with one sentence of thanks or context, no more
- list decisions made (if any), then action items grouped by owner with due dates
- end by asking for corrections or the answers to the open questions
- match the register of the notes (casual notes, casual message)

Use the same wording as the table so the two never disagree. Don't add commitments that aren't in the notes.

## Judgment calls

- **Vague commitments** ("I'll look into it"): include them, with the vagueness reflected in Due ("No date") and a matching open question, instead of dropping them.
- **Questions of attribution**: transcripts often lack speaker labels. If you can't tell who spoke, mark the owner "(?)" and ask.
- **Very long transcripts**: skim for commitment language ("I'll", "can you", "let's", "by", "next step", "action") but read the whole thing, because commitments also hide in the last five minutes.
- **Sensitive content** (performance, layoffs, legal): keep the follow-up message neutral and leave out anything the notes don't clearly intend for all attendees; mention to the user that you did so.
- **Nothing actionable**: say so plainly, give a 2-3 sentence summary, and skip the table.

## Format notes

Lead with the table; the user came for it. Keep commentary to a minimum. If the user asked only for one part ("just the follow-up"), produce that part, still grounded in the same extraction. If the user wants a file (Excel, Word), build it with the matching skill after producing the content.
