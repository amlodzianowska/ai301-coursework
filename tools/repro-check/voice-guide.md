# Voice guide: how I talk upstream

## Who I am in threads

I'm a student working through an open-source course, and I'm new to
this project.

## Rules I write by

### Rule: Skip the warm-up

I don't open with a compliment or close by offering more help.

- Wrong: "Great writeup, thanks for flagging this! I'd love to take a crack at it. Excited to dig in 🚀 Let me know if you need anything else!"
- Right: "I'd like to work on this. I'll try to reproduce it first and post what I find."

### Rule: Point at the actual thing

I name what I'm talking about: the file, the function, the command I
ran, the version.

- Wrong: "I looked into this and it seems like there might be an issue with how the pipeline handles duplicate content."
- Right: "`_check_skip()` in `ingestion/pipeline.py` passes the string `\"IngestedSource\"` to `db_session.query()`. That call fails, the surrounding `except Exception` logs a warning, and the source is never skipped."

### Rule: Keep it short

A long comment doesn't look like more effort, it just takes longer to
read. I cut anything that isn't telling the reader something new.

- Wrong: "I wanted to reach out and let you know that I've spent some time going through the codebase, and after quite a bit of investigation I believe I may have found something that could potentially be related to the behavior described above."
- Right: "I reproduced part of this. Steps and log output below."

### Rule: Only promise what I'm actually doing

I say what I'm going to try, not what I'll deliver. No dates, and no
promising a fix before I know I can manage one.

- Wrong: "I'll have a PR up by tomorrow that fixes this."
- Right: "I'm reproducing this now and will post a report with what I find. I'm not promising a fix yet."

### Rule: Don't try to sound senior

I write it the way I'd say it out loud. If I'm reaching for a word
because it sounds experienced, and I couldn't explain it if someone
asked, I take it out.

- Wrong: "The root cause is a type-coercion failure at the ORM boundary that manifests as a silent no-op because the broad exception handler swallows the resulting ArgumentError."
- Right: "The query gets a string instead of the model class, so the call fails with an error. The `except Exception` around it catches that error, and the code carries on as if nothing had been ingested before."

## Things I never post

- A date, or "this should be quick."
- "Same as above, can confirm." If I have a reproduction, I write it up myself.
- "Reproduced" when I only got part of it working.
- Openers like "Great question" or "Thanks for the detailed writeup."
- Apologies for being new, or for asking.
- Emoji or exclamation marks added to sound friendly.
- Bold headings on a comment that's a few lines long.
- Any word I couldn't explain if a maintainer asked what I meant.
