---
name: crm-entry
description: Turn raw relationship-manager notes (call notes, voice-memo transcripts, forwarded email threads, meeting invites with notes, German or English) into a CRM entry in the fixed CRM-entry.md format. Use when the user asks to create, write, or generate a CRM entry from notes, for one note or a whole folder of notes.
---

# crm-entry

Turn one raw note into one CRM entry in the fixed format below. Never invent information. A missing fact stays missing and is marked as such.

## Output format (fixed)

Use exactly these field names, in this order. Do not add or rename fields, with one exception (see "Restrictions").

```
Client ID: C-104
Contact: Mira Keller
Meeting date: 2026-11-12
Goal: compare two custody fee options
Next action: RM sends fee table
Due date: unknown
Open questions:
- ...
Assumptions (not verified):
- possible move to Zurich
```

Provenance of the format: target example `CRM-entry.md` supplied by the user (pasted into the conversation when this skill was requested).

## Field rules

- **Client ID**: format `C-<number>`. If the note's ID is garbled or marked uncertain (e.g. `C-29O ?`), write your best reading followed by `(unverified)`. If absent, write `unknown`.
- **Contact**: the client's full name as given, without titles (Mr, Fr., Herr). Use the spelling the note corrects itself to.
- **Meeting date**: ISO `YYYY-MM-DD`, the date of the call or meeting itself, not the date of a follow-up email or the date you are writing the entry.
  - Resolve relative dates ("this morning", "Tuesday 17 Nov") only when the note itself anchors them. Otherwise write `unknown`.
  - Partial dates (a weekday plus day of month, such as "Friday the 20th", with no month or year) may be completed only if exactly one nearby month and year fits that weekday and day. Today's date is the reference. Mark the result `(inferred)`. If more than one date fits, write `unknown`.
  - If a typo makes the date a guess (`1Z.11.2026`), write the likely reading with `(unverified)`.
  - The same date rules apply to Due date, except that Due date never carries a marker (see below). A partial date that fits uniquely becomes a plain ISO date, and the inference is noted under Open questions (e.g. `- due date inferred from "Friday the 20th"`). If the date is only a typo-based guess, or the RM flags it as uncertain in the note ("I think", "should double check"), write `unknown` and put the likely date under Open questions.
- **Goal**: one line stating what the client wants to achieve, in neutral wording. State the request only. Never write that something was approved, ordered, or executed unless the note says so explicitly.
- **Next action**: one line, written as `<who> <does what>`. Pick the action the RM owes next, usually the one with the earliest deadline. If a note has several RM actions, put the others into **Open questions** as to-do items so nothing is lost.
- **Due date**: either an ISO date or the single word `unknown`, nothing else, so the field stays machine-readable. It is the deadline for the Next action. Never infer a date from "soon", "before Christmas", or similar. Put such vague wording under **Open questions** instead, e.g. `- exact date to be agreed (client said "before Christmas")`.
- **Open questions**: a bullet list of things the note leaves unresolved that the RM must clarify or check, plus any secondary actions (see above). Write `- none noted` if there are none.
- **Assumptions (not verified)**: a bullet list of anything that is hearsay, a client claim without evidence, or tentative ("might move", "my accountant says it's fine", "3a already maxed (she says)"). Keep these out of Goal. Write `- none` if there are none.

## Restrictions (the one allowed extra line)

If the note contains a client restriction or compliance-relevant instruction (e.g. "do not contact my wife", "no power of attorney yet"), add one extra line directly below Goal:

```
Restrictions: do not contact client's wife about this matter
```

Omit this line when there is no such restriction.

## Handling the source note

- The note may be German, Swiss German, or English, or a mix. Always write the entry in English.
- The note may be a voice-memo transcript, a forwarded email thread, a calendar invite with RM notes underneath, or terse shorthand. Use only the RM's own account and the client's own statements.
- Ignore logistics noise (room, coffee, "please bring ID") unless it affects the next action.
- Correct self-corrections in the note ("300, no sorry, 350") to the final value. Do not mention the first value.
- Amounts: keep the currency stated in the note. If none is stated, write the number as given and add the question "currency not stated" under Open questions.
- Do not add advice, recommendations, or compliance commentary of your own.

## Process

1. Read the note(s). For a folder, process every note, one entry per note.
2. Draft each entry in the format above.
3. Save each entry as `C-<number>_<surname>.md`, lowercase surname, in a `crm_entries/` folder next to the notes folder. Use `C-unknown_<surname>.md` if the ID is missing. Do not overwrite an existing file without asking.
4. Reply with the list of files created and, per entry, anything marked `unknown` or `(unverified)` so the RM can fix it.
5. When I ask you to run your skill on every note in a folder, follow steps 1-4 for each note and as a final step, compile the results in results.xlsx in the following format: id, question, answer, sources - and save this file in the same folder as the notes.
