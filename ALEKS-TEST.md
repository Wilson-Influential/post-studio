# Post Studio: what to send Alex

Goal: Alex tells his own AI what he wants ("make me a carousel about X"), the AI hands him a
link, he opens it and Post Studio is already filled in, and he designs from there. No install,
no setup.

Send him **two things**, in two messages so it does not feel like a wall of text.

---

## Message 1: the idea + a link he can click right now

> Built something I want you to try. Quick version of how it works: you (or your AI) plan the
> copy for a post or carousel, then you just tell the AI to open it in the Post Studio. It hands
> you a link. You open the link and the Studio is already filled in with your copy, the sector
> colour, the placement, the logo, across nine on-brand layouts. You pick the one you like, tweak
> any slide, and export real PNGs. It is the same way I have been working: plan the words, let the
> AI set it up, then design.
>
> Here is a finished example so you can see it end to end. Open it, click any of the nine styles,
> and step through the carousel with the arrow keys or the filmstrip. Edit a slide, then use "This
> slide" or "All slides" to download.
>
> https://post-studio-pi.vercel.app/#z=eNp1krGO2zAMhl-F0Oxm6JjNBXJNhmuK2LhrUXRQJDoWLIuCRMdnHPLupXJFk6FZDPqXyP8jqXd1VuvPlWIir9YqUuZPmSfrSFUqajOI-O8_o2FKolh3cqx9ueK1wREDX5MTJ-1YZE8nEmXULpRLUjWr9a93NTgzYKnQbjfQbHdPrRz3qK1IDVOEmGiM7MIJKCDUuxU0rBNDmkIoqgZGPQJ1wD2OK8k-kl0kWyLDugSX6s7ndfsTdi081227OTQ3sxpMr_lIDLMQ5xXUMFMaOk9z8cp3lfcCMpJFDy6I_5HehM0vYAlzoYCAbyxBwVtoAl4irh7glLaf9y-bG8hXd0bAM6YFMmMEgQGaA-SIxmnvMt-hHDCjTqavwCbdcSVNoBkqyL2La2GzruswyTZAn8q3owSoTf8I5_thv3-6sWxpSrnMtoxCPMoK5t55vPaVPWK8g2mQr-0fk8NOUGiUWF4MMAlK54LLPVoJUuYP3kcY3zY_7t7Bl8l5WxzT39x697F0GXGGGXH439pb7a_OU1aX35c_oonrLg

---

## Message 2: make your own with your AI

> Now make one yourself. Open Codex (or Claude), paste the block below, and tell it the topic you
> want, or let it pick. It will write the copy and hand you a link. Open it and you are straight
> into the Studio with your own carousel.

Then paste him this prompt:

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
import json, base64, zlib
payload = {
  "v": 2, "tool": "post-studio",
  "pack": "studio",         # studio (clean brand poster) | social (IG house style)
  "sector": "digital",      # pick one: insights | digital | creative | pr | campaign
  "placement": "portrait",  # portrait is the carousel default
  "logo": "main",
  "posts": [
    {"kicker": "COVER",   "head": "Your hook here",   "body": "",            "cta": ""},
    {"kicker": "POINT 1", "head": "First point",      "body": "One sentence.", "cta": ""},
    {"kicker": "POINT 2", "head": "Second point",     "body": "One sentence.", "cta": ""},
    {"kicker": "CTA",     "head": "What to do next",  "body": "",            "cta": "Get in touch"}
  ]
}
raw = json.dumps(payload, separators=(",", ":"), ensure_ascii=False).encode("utf-8")
enc = base64.urlsafe_b64encode(zlib.compress(raw, 9)).decode().rstrip("=")
print("https://post-studio-pi.vercel.app/#z=" + enc)
PY

Give me the brief, the slide copy in a readable list, and the final link.
```

---

## Notes for Shea (not for Alex)

- **Run message 1 + 2 through the humanizer before sending** (team-shared copy rule). They are
  written to house style but have not been humanized this session.
- The demo link in message 1 is the AI-team carousel. Regenerate it with the Python in message 2
  if you want a different example.
- **Fonts:** exports look right only on a machine with Effra installed. On Alex's machine previews
  fall back to a system font, so they look slightly off, but the layout and copy are correct. Worth
  saying if he comments on the type.
- The link opens **bare Post Studio**, not the unified Studio shell. That is deliberate: the link
  drops him straight into the tool he is editing, and the top-bar tool switcher would not change
  anything for a single pre-filled post.
