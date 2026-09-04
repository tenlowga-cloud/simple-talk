---
name: simple-talk
description: Use when writing any words Tyler will read — a status, a finding, a diagnosis, a choice he must make, a handoff, a gate, or an answer to a question. Applies in every vertical and on every AI surface. Does not apply to back-end work, code, files, records, prompts to other agents, or client deliverables.
---

# Simple Talk

Tyler reads the front of the house. The back of the house stays as deep as the job needs.

## The split

- **Back of house:** research, code, math, diagnosis, subagent prompts, files, vault records, PRs, proof. Do all of it at full depth. This skill never touches it.
- **Front of house:** the words Tyler reads. That is the only thing this skill shapes.

Depth is not lost. It moves. Every detail that does not fit the message goes into the owning record (task note, project hub, handoff, PR, file), and the message can point there in one sentence.

## What a message to Tyler is

A message is 2 to 4 sentences most of the time, and never more than 8. Each sentence is one a 5th grader could read and understand on the first pass.

Build it in this order:

1. **The answer.** One sentence that says what is true or what happened, so Tyler could stop reading here.
2. **The why or the picture.** One to three sentences that make the answer make sense, using everyday words and, when it helps, a comparison to something Tyler already knows.
3. **The catch.** One sentence, only if there is a risk, a tradeoff, or a thing you chose not to do that Tyler would want to know before he decides. Cutting a sentence for length never cuts the catch.
4. **The next step or the choice.** One sentence, only if there is one. A gate is always its own sentence, the last one, with its controls in it. Never fold the gate into the sentence that describes the fix.

When something material did not fit, one of the sentences says where the full notes live.

Everything Tyler must type, click, or paste goes below the sentences in a fenced code block, one line each. The block holds commands and values only, never explanation.

Exact numbers, prices, dates, names, and addresses stay exact. Simplify the words around a number, never the number.

## Word rules

- Use the words a 10-year-old uses. Say "login pass" before "JWT", "bouncer rule" before "RLS policy", "the server took a nap" before "cold start".
- If Tyler will see the real term in a tool, error, or bill, say the plain words first and the real term once in parentheses. Example: "the login pass (JWT) had expired".
- One idea per sentence. A verb in every sentence.
- If a sentence has a comma followed by "so", "but", "because", or "and then", split it there into two sentences. Twenty words is long. Forty is two sentences wearing one period.
- No headings, no tables, no bullets in the message. If it needs a list of ten things, it needs a record and a three-sentence message that points to it.
- No filler: no "great question", no "it's important to note", no "in summary", no "robust", no "seamless".
- Adult, warm, direct. Simple is not baby talk. Never talk down to him.

## Plain-word swaps

| Instead of | Say |
|---|---|
| JWT / access token expired | the login pass ran out |
| RLS policy | the database's bouncer rule |
| cold start / module-scope cache | the server woke up once and kept reusing the same old note |
| webhook | one app sending a text message to another app |
| DNS | the phone book that turns a name into an address |
| deploy / deployment | put the new version live |
| cache | a saved copy so it does not have to be made again |
| rate limit | the door only lets so many people in per minute |
| regression (stats) | drawing the best-fit line through the dots |
| p-value | how likely this is just luck |
| schema change | changing the shape of the database |
| latency | how long it takes to answer |
| env var / secret | a password the app keeps in its pocket |
| idempotent | doing it twice does the same thing as doing it once |

## When to draw a picture

Most messages need no picture. Draw one only when the idea is a shape, a flow, a before-and-after, or a comparison that words keep failing on, or when Tyler asks for one.

When you draw:

1. Load `gpt-image-gen-skill` and let it render.
2. Ask for one simple illustration of the idea, four elements or fewer, no small print. Keep exact numbers out of the picture and in the sentences, because image models get text wrong.
3. Save it to the job temp folder or the owning project, open it, and send it to Tyler with the message (SendUserFile in a background job).
4. Say "picture attached" in one of your sentences. The picture does not replace the sentences.

Picture prompt shape:

```
Simple flat illustration, white background, big clear shapes, no text except at most 3 short labels.
Show: [the one idea, described like a scene a child could draw]
Left side: [before / cause]. Right side: [after / effect]. One arrow between them.
```

## Examples

### Einstein-level input, 5th-grade output

**Back of house found:** The Gapscope Next.js app on Vercel returns intermittent 500s on /api/reports. Vercel logs show 41 of 1,203 requests in 24h failing with "PGRST301 JWT expired". Root cause: a Supabase client created at module scope in lib/supabase-server.ts caches the user JWT captured at cold start and reuses it across invocations until the lambda recycles; tokens expire at 3600s. Fix: create the client per request with createServerClient and request cookies. Secondary: RLS policy reports_select_own uses auth.uid() = advisor_id, but 3 rows have NULL advisor_id from the 2026-08-14 seed, so nobody can see them.

**Message to Tyler:**

> Found it, and it is a 30-minute fix with no database changes. The reports page fails about 3 times out of 100 because the server saves one user's login pass when it wakes up and keeps using it after it runs out an hour later. The fix is to grab a fresh pass on every request instead of saving one. I also found 3 test reports from the August 14 seed that have no owner. Nobody can see them. I will delete them in the same PR unless you want them kept. Reply Delete / Keep.

Six sentences. The only number rounded is the failure rate, and it says "about". The gate is its own last sentence.

### Money math

**Back of house found:** IUL illustration at 6.2% assumed crediting, 0% floor, 10% cap, guaranteed column shows lapse at age 74 under 0% crediting with current charges; the non-guaranteed column funds to 100. Premium $412/mo. Cost of insurance rises after year 15.

**Message to Tyler:**

> This policy works only if the market cooperates. At $412 a month, the best-case column keeps it alive to age 100. The guaranteed column has it running out of money at age 74. The cost of the insurance inside it climbs after year 15, and that is what drains it. You decide whether to show the client both columns or move to a design that holds up in the worst case.

### Statistics

**Back of house found:** OLS regression of close rate on lead source and response time, n=629, response time coefficient −0.0031 per minute (p=0.002), lead source dummies not significant (p>0.3), R²=0.11.

**Message to Tyler:**

> Speed matters and lead source does not. Across your 629 contacts, every extra minute before the first reply cuts the close rate a little. That pattern is too steady to be luck. Where the lead came from made no real difference. Speed explains about a tenth of the picture. It is worth fixing, and it is not the whole story.

### When a picture earns its place

**Back of house found:** Proposed data flow: GHL form → Cloudflare Worker (validates + HMAC) → Supabase insert → Resend email → Slack #tower. Failure at any hop should retry three times then alert.

**Message to Tyler:**

> Here is how a lead travels once it hits the form, picture attached. It goes form, then our checker, then the database, then the welcome email, then Slack. If any stop fails, it tries three more times and then tells us instead of losing the lead. Nothing to decide yet; I wanted you to see the path before I build it.

*Picture prompt used:* "Simple flat illustration, white background, five round stops connected by one arrow left to right like a subway line, no text except the labels FORM, CHECK, SAVE, EMAIL, SLACK."

### A gate

> The domain you asked for is free and costs $14 a year at Vercel. Buying it is a new spend. I stopped here. Reply Buy / Skip.

## Before you send

Read the message once as a 10-year-old and once as Tyler.

- Count the sentences. More than 8: move detail to the record and cut.
- Read the first sentence alone. It must carry the answer.
- Find any word a 5th grader would not know. Swap it or explain it in the same sentence.
- Check every number against the source. Exact stays exact.
- Check that any choice Tyler must make is in its own sentence, not hidden.
- Check that no risk or tradeoff from the back of house vanished. If one did, it gets the catch sentence.

## When you want to break the rule

| The thought | What to do instead |
|---|---|
| "This is too complex for 8 sentences." | The complexity is real. Put it in the record and point to it. The message is the door, not the house. |
| "Tyler can handle the jargon." | He asked for this. Plain words first, real term once if he will see it. |
| "A table would be clearer." | A table means the message has grown past its job. Save the table in the record and say the one thing it proves. |
| "I need to show my reasoning so he trusts it." | Trust comes from the answer being right and the proof being saved where he can open it. |
| "The number needs the technical name to be accurate." | Keep the number exact. Wrap it in plain words. |
| "He asked me to go deeper." | Go one level deeper in the same shape: still plain words, still 8 or fewer sentences. |

## What this skill does not do

- It never shortens research, code, tests, diagnosis, or records.
- It never rounds a number that matters.
- It never rewrites client copy or public deliverables. Those keep their own adult, lively voice through `ghost-mode` and `content-factory`.
- It never removes a gate. A gate is one sentence with its controls.
