# Post Studio carousel: test handoff for Aleks

Goal: Aleks asks his own Codex (or any AI that can run a shell) to invent a fake brief and
carousel copy, and hand back a link that opens Post Studio already filled in. No skill to
install, nothing to set up. The prompt below is self-contained.

Live tool: https://post-studio-pi.vercel.app

## What to send Aleks

Send him this message:

> I've updated the Studio. Want to test the new carousel mode? Open Codex, paste the block
> below, and tell it the topic you want (or let it pick one). It'll hand you a link. Open the
> link, click any of the nine styles, and step through the carousel. You can edit any slide and
> download one or all of them.

Then send him the prompt in the next section.

## The prompt for Aleks to paste into Codex

```
You are helping me make an on-brand Instagram carousel for Influential, a marketing and
communications agency. Do three things:

1. Invent a short, realistic fake brief for an Instagram carousel (pick a topic relevant to a
   marketing agency, e.g. a campaign insight, a PR tip, a social trend). One or two lines.

2. Write the carousel copy: a cover slide that hooks, three or four content slides (one idea
   each), and a final slide with a call to action. Each slide has:
   - kicker: a short eyebrow label, max 28 characters (e.g. THE SHIFT)
   - head: the slide's headline
   - body: one short supporting sentence (leave empty "" on the cover and the final slide)
   - cta: only on the final slide (max 34 characters), empty "" elsewhere
   House style: plain and confident, no em dashes, no buzzwords.

3. Turn that copy into a Post Studio link by running this exact Python, with your slides in the
   posts array, then give me the printed URL:

python3 - <<'PY'
import json, base64
payload = {
  "v": 2, "tool": "post-studio",
  "sector": "digital",      # pick one: insights | digital | creative | pr | campaign
  "placement": "portrait",  # portrait is the carousel default
  "logo": "main",
  "posts": [
    {"kicker": "COVER",   "head": "Your hook here",              "body": "",            "cta": ""},
    {"kicker": "POINT 1", "head": "First point",                "body": "One sentence.", "cta": ""},
    {"kicker": "POINT 2", "head": "Second point",               "body": "One sentence.", "cta": ""},
    {"kicker": "CTA",     "head": "What to do next",            "body": "",            "cta": "Get in touch"}
  ]
}
enc = base64.urlsafe_b64encode(
    json.dumps(payload, separators=(",", ":"), ensure_ascii=False).encode("utf-8")
).decode().rstrip("=")
print("https://post-studio-pi.vercel.app/#d=" + enc)
PY

Give me the brief, the slide copy in a readable list, and the final link.
```

## What Aleks should see

Opening the link shows the nine styles, each previewing slide 1 (the cover). He clicks a style,
it builds the whole carousel in that template (bold cover, calmer inner slides, a slide number on
each), and he can step through with the arrow keys or the filmstrip, edit any slide, and use
"This slide" or "All slides" to download PNGs.

Note on fonts: exports render in the Influential brand font (Effra) only on a machine with Effra
installed. On another machine previews fall back to a system font, so they look slightly off, but
the layout and copy are correct.
