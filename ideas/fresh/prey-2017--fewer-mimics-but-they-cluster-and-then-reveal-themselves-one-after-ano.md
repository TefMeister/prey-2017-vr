# Fewer mimics, but they cluster — and then reveal themselves one after another

Order: 3000
From: mod-ideas `games/prey-2017.md` (<https://github.com/TefMeister/mod-ideas/blob/main/games/prey-2017.md>), copied 2026-09-23

`[raw]` · `[not judged]` — ⚠️ **not checked against the game's own files or AI setup**

> "less mimics around but where they are, they group together and then start changing from objects
> back to mimics one after another quickly, so the player gets overwhelmed by how many is
> surrounding them, 10 - 15 of the at a time is horrible"

Trade quantity for staging. Instead of mimics being sprinkled thinly across a level, a room holds a
cluster of them hiding as ordinary objects — and when the trigger comes they drop the disguise in
quick succession, so the player watches the room turn into a circle of enemies around them. Ten to
fifteen at once is named as the target for "horrible".

**Why it is a good fit for this game specifically:** the mimic's whole point is the dread of not
knowing what is real, and the stock game spends that dread one jump at a time. This spends it all
at once, and the scare is the *count* rather than the surprise. In a headset, where you have to
physically turn your head to see how surrounded you are, it should read far stronger than it does
flat.

**What it'd take — honestly unknown yet, but the shape of it:**
- **Fewer, clustered spawns** — a level-placement change rather than a code change, if mimic
  placement turns out to live in the level data.
- **A shared reveal trigger** — one mimic being disturbed has to wake the rest of its group, with
  a short stagger between each so they pop one after another instead of all on the same frame.
  This is the part most likely to need real AI work.
- **Whether 10–15 active mimics is affordable at all** is an open question — that is a lot of
  simultaneous AI and animation, and it would be judged on the home PC, never the dev PC.

⚠️ None of the above has been checked against Prey's actual files. It is the plan you would test
first, not a verdict.

---
