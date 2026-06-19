# Post Studio: what to send ALEX

Goal: ALEX tells his own AI a topic ("make me a carousel about X"), the AI hands him a short
link, he opens it and Post Studio is already filled in, and he designs from there. No install,
no setup.

The simplest version: he drags one file into Codex, names a topic, gets the link. That's the
**For ALEX** folder next to this file.

---

## What to send him (pick one)

### Option A (simplest): the drag-in file

Send him the **`For ALEX`** folder (or just the single file inside it,
`Post Studio (give this to your AI).md`). Tell him:

> Built a thing I want you to try. Drag this folder into Codex, then say: "make me an Instagram
> carousel about [your topic]". Codex will write the copy and give you a short Post Studio link.
> Open it and the carousel is already filled in. Then you can pick a layout, tweak the slides, and
> export. Nothing to install.

The file is self-contained: it tells his AI exactly what to write and runs the Python that builds
the short link. He just picks the topic.

### Option B: a finished example he can click right now (zero effort)

If you want him to see it before he tries it himself, send this link. It opens a finished AI-team
carousel he can step through and edit:

> https://post-studio-pi.vercel.app/#z=eNp1krGO2zAMhl-F0Oxm6JjNBXJNhmuK2LhrUXRQJDoWLIuCRMdnHPLupXJFk6FZDPqXyP8jqXd1VuvPlWIir9YqUuZPmSfrSFUqajOI-O8_o2FKolh3cqx9ueK1wREDX5MTJ-1YZE8nEmXULpRLUjWr9a93NTgzYKnQbjfQbHdPrRz3qK1IDVOEmGiM7MIJKCDUuxU0rBNDmkIoqgZGPQJ1wD2OK8k-kl0kWyLDugSX6s7ndfsTdi081227OTQ3sxpMr_lIDLMQ5xXUMFMaOk9z8cp3lfcCMpJFDy6I_5HehM0vYAlzoYCAbyxBwVtoAl4irh7glLaf9y-bG8hXd0bAM6YFMmMEgQGaA-SIxmnvMt-hHDCjTqavwCbdcSVNoBkqyL2La2GzruswyTZAn8q3owSoTf8I5_thv3-6sWxpSrnMtoxCPMoK5t55vPaVPWK8g2mQr-0fk8NOUGiUWF4MMAlK54LLPVoJUuYP3kcY3zY_7t7Bl8l5WxzT39x697F0GXGGGXH439pb7a_OU1aX35c_oonrLg

Best move: send the folder first. Add the finished example only if you want him to see the payoff
before he tries his own topic.

---

## Notes for Shea (not for ALEX)

- The message copy above has had a humanizer pass for team sharing.
- The demo link is the AI-team carousel. Regenerate it from the Python in the `For ALEX` file if
  you want a different example.
- **Fonts:** exports look right only on a machine with Effra installed. On ALEX's machine previews
  fall back to a system font, so they look slightly off, but the layout and copy are correct. Worth
  saying if he comments on the type.
- The link opens **bare Post Studio**, not the unified Studio shell, on purpose: it drops him
  straight into the tool he's editing, and the tool switcher wouldn't change anything for one
  pre-filled post.
