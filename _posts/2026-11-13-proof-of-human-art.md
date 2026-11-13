---
layout: post
title: "I tried to build proof that art isn't AI. The people who ban AI didn't need it."
date: 2026-11-13
---

<img src="/assets/images/timelapse.png" alt="Procreate time-lapse compared with the final image" class="post-figure-right">

For three weeks I worked on a side project with a simple premise: AI
detectors guess from the final image, but whether a human made a picture is a
fact about *how* it was made. Digital illustrators already record that. Procreate
saves a time-lapse of every stroke inside the project file. So why not check
the recording against the delivered image automatically, and give
illustrators a proof that holds up?

I wrote the code. Then I went looking for someone who would pay for it. This
post is about what I found, because I think the findings are more useful than
the code (which is now [open source](REPO_URL)).

## The idea, in one paragraph

An illustrator uploads her working file and the final image. The tool replays
the recorded process and checks a few things: does the last frame match the
delivered image, does the drawing build up over time the way drawings do, are
there large regions that appear in one step (a pasted image), does the final
file differ structurally from what was drawn. It doesn't say "AI" or "not AI".
It says whether the delivered image is consistent with a recorded human
process, and flags what a reviewer should look at.

## First problem: the cheap alternative is very good

Before talking to anyone, the obvious substitute was detectors. A detector
like Hive costs about $0.006 per image. In an
[academic evaluation](https://arxiv.org/abs/2402.03214) it reached 98% accuracy
with 0% false positives on clean human art. Its weak spots are real but narrow:
false positives rose to about 3% on human art protected with Glaze, and Glaze
also cut its detection of AI images to around 68%. Invisible watermarks on AI
output are coming too (the EU AI Act requires machine-readable marking), but
[they can be removed](https://arxiv.org/abs/2306.01953).

So "is this AI?" is mostly answered for routine screening at well under a cent.
Process evidence only matters where a guess isn't good enough: a disputed
case, a prize, an accusation.

## Second problem: the "human-made" market is crowded and nearly free

I found at least seven services that certify work as human-made, most launched
in the last two years:

- some take a declaration (Stampr explicitly does not verify human creation);
- some check identity rather than the work (the Authors Guild's "Human
  Authored" mark says it is "not aware of any truly reliable way to test" a
  manuscript);
- some have reviewers look at sketches, layers and time-lapses by hand (Human
  Made Art at $1 per review, I've Made This for free, Proudly Human with
  reviewers and published limits on AI use).

The one that publishes numbers reports 36 certified works. There are more
"human-made" marks than demand for them. And every certifier that actually
reviews process evidence does it manually, which was the one gap I could see:
nobody analyzes the working file automatically.

## Third problem: the people with an AI ban don't feel a problem

Illustrators don't pay for proof; good-faith doubters are usually convinced by
a free time-lapse, and bad-faith accusers aren't convinced by anything (more on
that below). So the buyer had to be someone who *enforces* an AI rule.

Every major illustration competition I checked bans AI:

| Competition | Rule | Verification described in the rules |
|---|---|---|
| Bologna Illustrators Exhibition | AI tools "not permitted" | none |
| Society of Illustrators Annual | AI "at any capacity" not eligible | none |
| Communication Arts Illustration | no text-to-image submissions | hi-res files from winners |
| 3x3 International Illustration Show | no AI-generated images | hi-res files from winners |
| Illustration West | entrant warrants work isn't mainly AI | none |
| San Diego Comic-Con Art Show | AI "partially or wholly" not allowed | the coordinator decides |

Bologna alone received 20,840 illustrations from 4,168 illustrators in 2026.
Nobody can check that by hand. That looked like the opening.

Then the organizers answered. Two of the large US annuals replied:

- One said the rule deters most AI entries. Some AI work had been entered, and
  some had even been **selected as winners**; they catch it when they ask
  winners for more information. Their verdict: they don't see a tool being
  useful for their process.
- The other said "no AI" is basically an honor system, but illustrators are so
  strongly against AI that they doubt they receive AI entries at all.

A children's book publisher I interviewed commissions 100 to 500 illustrations
a year, half from new illustrators, and has never once doubted one. They follow
the sketches as the work progresses and rely on a "no AI" clause in the
contract.

Put together: the rule is there, enforcement is light, and nobody experiences
that as a cost. You can't sell a painkiller to someone who isn't in pain.

## What the public threads showed

I also read through public accounts of illustrators accused of using AI. Selected
for incidents, so they show *what* happens, not *how often*:

- Accusations are common for artists who are visible online, and grow with
  visibility.
- People acting in good faith are usually convinced by one time-lapse, a
  sketchbook, or someone vouching.
- People acting in bad faith aren't convinced by anything. One moderator
  refused to look at a layered file; an artist drawing live at a market stall
  still got accused.
- Layer screenshots and edited speedpaints are increasingly dismissed as "super
  easy to fake".
- The money at stake is small: in one case a client withheld half of a $100
  commission.

A stronger proof doesn't help much with either group: the first already
believes cheap proof, the second won't believe any.

## Where it does hurt: photography

One organizer pointed out that photography contests have the harder problem,
and the public record agrees. An AI image reached the
[finalists of the Hasselblad Masters 2026](https://www.photoworkout.com/?p=221203);
Reddit flagged it within hours, while the jury had missed it. Nikon
[disqualified the winner](https://www.photoworkout.com/?p=223576) of its 2026
Small World in Motion contest after scientists spotted implausible footage.
The Canadian association CAPA now
[checks finalists](https://capacanada.ca/new-ai-authentication-standards/) with
RAW files, forensics, reverse image search and Hive, and plans optional
Content Credentials from 2027.

The pattern: organizations act *after* a public embarrassment, not before.
That's hard to sell into in advance.

## The threat to the whole approach

Process evidence has its own expiry date. AI agents that operate software can
already draw stroke by stroke in a real app, which would produce a genuine
recording of a process no human performed. The moment AI images get good
enough that juries *and* crowds can't tell, process recordings may get easier
to fake too. And when demand does arrive, the cheapest answer is the drawing
app itself exporting a signed time-lapse, not a third party analyzing it.

## What I'd do differently

- **Search for substitutes and competitors before writing code.** I found the
  certifiers by searching their names, after the prototype existed.
- **Ask enforcers about past cases, not hypotheticals.** "Have you ever doubted
  a submission? What happened?" told me more than any pitch.
- **Set kill criteria you can actually fail on their merits.** Mine asked for a
  paid pilot in three weeks for a product that didn't exist; it would have
  failed whatever the truth was.

## What's left

The code is [on GitHub](REPO_URL) under AGPL: it reads a Procreate file or any
time-lapse video, compares it with the final image, and reports consistency,
pasted regions and structural changes. All thresholds are calibrated on
synthetic images only, and the README says where they're weak. If you run a
competition, a certification service or a commission platform and *do* spend
time checking process files by hand, I'd like to hear from you.

Maybe it's simply too early. If an illustration competition ever adds a
verification step to its rules, or a drawing app ships signed process exports,
I'll know.