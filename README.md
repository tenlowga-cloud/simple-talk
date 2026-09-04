# simple-talk

> **Mirror, not the source.** The living copy of this skill is
> `skills/simple-talk/SKILL.md` in the private `tenlowga-cloud/claude-config` repo.
> This repo is the public copy. Edit it there, then sync here. If the two ever
> disagree, claude-config wins.
>
> Synced 2026-09-04. Before that this repo sat a month behind and the install
> commands below were handing out the old August version.

A Claude Code skill. It keeps every message short and plain: 2 to 4 sentences most of
the time, never more than 8, in words a 5th grader knows. Answer first, then the why,
then the next step.

It is not dumbing things down. Facts, numbers, prices, dates and names stay exact.
Only the words get smaller.

## What it does

- Swaps jargon for plain words ("API" becomes "a doorway one program uses to talk to another")
- Kills AI slop words and filler phrases
- Explains hard things by cutting them into small pieces instead of leaving pieces out
- Leaves client-facing deliverables (emails, PDFs, code) in their normal professional style
- Leaves back-of-house work at full depth. The short rule is for messages a human reads

## Install

Copy `SKILL.md` into a skill folder Claude Code reads.

Personal, every project:

```bash
mkdir -p ~/.claude/skills/simple-talk
curl -fsSL https://raw.githubusercontent.com/tenlowga-cloud/simple-talk/main/SKILL.md \
  -o ~/.claude/skills/simple-talk/SKILL.md
```

One project only:

```bash
mkdir -p .claude/skills/simple-talk
curl -fsSL https://raw.githubusercontent.com/tenlowga-cloud/simple-talk/main/SKILL.md \
  -o .claude/skills/simple-talk/SKILL.md
```

Start a new session. Claude picks the skill up on its own, or you can call it with
`/simple-talk`.

## Make it yours

`SKILL.md` names Tyler in a few places. Swap that name for yours, or delete the name
and leave the rules. Nothing else needs to change.

## License

MIT. See `LICENSE`.
