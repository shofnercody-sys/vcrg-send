# vcrg-send

Landing page for the Veridata Compliance Research Group mailer. Homeowners scan the QR code and upload copies of the pre-foreclosure solicitations they receive.

- Live: https://shofnercody-sys.github.io/vcrg-send/ (also `/send/`)
- Files: `index.html` (main page), `thanks.html`, and `send/` (identical copies, so `veridatacompliance.org/send` works once the domain is connected)

## 1. How uploads work (no login needed)

The upload form is built into the page. Each submission is emailed, with the photos attached, to
**cody@quickleadsllc.org** through [FormSubmit](https://formsubmit.co) (free, no account).

- **One-time activation:** the very first submission makes FormSubmit email that inbox a
  confirmation link. Click it once. Until then, submissions are held, not delivered.
- After activation FormSubmit also emails a random string. You can put it in place of the email
  address in the form's `action` (in both `index.html` and `send/index.html`) to hide the address
  from spam bots.
- Photos are shrunk on the phone before sending (max 1800px JPEG). Limit: 10 photos, under ~9.5MB per submission.
- Visitors see a quick "I'm not a robot" check from FormSubmit, then land on `thanks.html`.
- To change the inbox, edit the `action="https://formsubmit.co/..."` line in both pages.

## 2. Re-deploy after any edit

```bash
git add -A && git commit -m "Update page" && git push
```

GitHub Pages rebuilds in about a minute.

## 3. Connect veridatacompliance.org (later)

Do these steps in this order. If you add the CNAME file before DNS is set, the github.io link will redirect to a domain that doesn't work yet.

**a. DNS records** (at your domain registrar, e.g. GoDaddy/Namecheap/Cloudflare):

| Type  | Name | Value |
|-------|------|-------|
| A     | @    | 185.199.108.153 |
| A     | @    | 185.199.109.153 |
| A     | @    | 185.199.110.153 |
| A     | @    | 185.199.111.153 |
| AAAA  | @    | 2606:50c0:8000::153 |
| AAAA  | @    | 2606:50c0:8001::153 |
| AAAA  | @    | 2606:50c0:8002::153 |
| AAAA  | @    | 2606:50c0:8003::153 |
| CNAME | www  | shofnercody-sys.github.io |

Delete any other A/AAAA records on `@` (e.g. the registrar's parking page). **Keep your MX records**, which handle email for ashlee@veridatacompliance.org.

**b. Add the CNAME file** to this repo with one line, then push:

```bash
echo veridatacompliance.org > CNAME && git add CNAME && git commit -m "Custom domain" && git push
```

**c. In GitHub:** repo Settings, then Pages, then Custom domain should show `veridatacompliance.org`. Wait for the DNS check to pass (minutes to a few hours), then tick **Enforce HTTPS**.

After that the page is live at `https://veridatacompliance.org/` and `https://veridatacompliance.org/send/`.

Optional: verify the domain under your GitHub account (Settings, then Pages, then Add a domain) so nobody else can claim it on GitHub Pages.
