# simple-talk

A Claude Code skill. It makes Claude explain things the way a caring teacher explains
things to a 5 year old: small words, short sentences, everyday comparisons, no jargon
and no AI filler.

It is not dumbing things down. The facts stay exactly right. Only the words get smaller.

## What it does

- Swaps jargon for plain words ("API" becomes "a doorway one program uses to talk to another")
- Kills AI slop words and filler phrases
- Explains hard things by cutting them into small pieces instead of leaving pieces out
- Leaves client-facing deliverables (emails, PDFs, code) in their normal professional style

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
