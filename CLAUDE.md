# CLAUDE.md

This file gives Claude (and any developer) the context needed to understand, edit, and redeploy the Lyndsey Austin Transformations website in VS Code.

## Project Overview

A single-page marketing/lead-generation website for **Lyndsey Austin Transformations** — a confidence coaching, mindset and fitness transformation business based in Dubai, UAE.

**Primary goal:** convert visitors into WhatsApp leads. Every CTA, pricing card, and the contact form routes to WhatsApp (`wa.me`) rather than a backend — there is no server, database, or CMS. This is intentionally a static site.

**Owner / contact number used throughout:** `+971 58 579 3525`
**Instagram:** `@lyndseya_transformations`
**Featured press:** UAE Stories — https://uaestories.com/lyndsey-austin-inspires-women-to/

## Tech Stack

- Plain **HTML5 + CSS3 + vanilla JavaScript** — no framework, no build step, no npm dependencies.
- Fonts loaded from Google Fonts CDN: **Poppins** (headings) + **Inter** (body).
- No backend. No database. No forms submit to a server — the lead form builds a `wa.me` deep link client-side and opens WhatsApp.

## File Structure

```
/
├── index.html                          # the entire site (single file: HTML + <style> + <script>)
└── assets/
    └── lyndsey-photo.webp              # real photo of Lyndsey, used in hero + "Meet Lyndsey" video poster
```

> Note: the deployed/shared copy may currently be named `lyndsey-austin-transformations.html` — **rename it to `index.html`** before deploying so hosts serve it as the default document. Keep `assets/` in the same folder as the HTML file; the image path is relative (`assets/lyndsey-photo.webp`).

## Design System

| Token | Value | Usage |
|---|---|---|
| `--ink` | `#171712` | primary text |
| `--paper` | `#FFFFFF` | page background |
| `--mint` | `#EEF4EF` | alternating light section background |
| `--green-deep` | `#173428` | top contact bar, dark sections, footer |
| `--green` | `#3E7A54` | eyebrow labels, small accents |
| `--orange` | `#E8622D` | primary CTA buttons, price highlights |
| `--wa` | `#25D366` | WhatsApp buttons (brand green) |

Typography: **Poppins 800/900** uppercase for all headings (`h1–h4`), **Inter 400–700** for body copy. This pairing + palette was deliberately matched to reference sites the client approved (realfit.ae — dark-green top bar, bold black uppercase headlines, orange CTAs).

## Page Sections (in order)

1. Top contact bar — click-to-call + WhatsApp + Instagram, dark green
2. Sticky nav — logo, anchor links, Call Now + Start Your Transformation buttons
3. Hero — headline, CTAs, real photo of Lyndsey with mint offset panel
4. "As Featured In" strip — links to UAE Stories article
5. Meet Lyndsey / intro video — **currently a placeholder** (see Known Placeholders below)
6. Pain points (editorial grid)
7. Her Story — three "turning point" narrative cards
8. Philosophy — Mind / Body / Purpose / Life
9. The Butterfly Transformation Programme — 5-step signature framework
10. Work With Me — pricing cards (book, group coaching, 1:1) + ladder rows (free guide, membership, retreats)
11. Confidence Collective — 8-week module list
12. Online Programmes grid
13. In Her Words (quotes) + Instagram callout
14. Contact — WhatsApp/call cards + lead form (submits to WhatsApp)
15. Footer
16. Mobile sticky bottom bar — Call + WhatsApp (shows under 760px viewport)

## Known Placeholders / Follow-ups

- **Intro video**: the `.video-frame` block in the "Meet Lyndsey" section currently shows the photo with a play button that triggers an `alert()`. Replace the whole `.video-frame` div with a real embed once a video exists, e.g.:
  ```html
  <div class="video-frame">
    <iframe src="https://www.youtube.com/embed/YOUR_VIDEO_ID"
            style="width:100%;height:100%;border:0;"
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
            allowfullscreen></iframe>
  </div>
  ```
  Prefer embedding via YouTube/Vimeo/Instagram rather than self-hosting the video file — keeps the site within Azure Static Web Apps' free-tier storage/bandwidth limits.
- **Hero photo**: currently the one uploaded photo (`assets/lyndsey-photo.webp`), sourced from the UAE Stories feature. Confirm commercial usage rights, or swap in a licensed/original photo when available.
- **WhatsApp number**: hardcoded in multiple places (`tel:`, `wa.me` links, JS submit handler). If the number ever changes, search the file for `971585793525` and `+971 58 579 3525` and replace both formats everywhere.

## Editing in VS Code

- Open the project folder directly — no install or `npm install` step needed.
- Use the **Live Server** extension (or any static file server) to preview with hot reload: right-click `index.html` → "Open with Live Server."
- All styling lives in the single `<style>` block in the `<head>` — CSS custom properties (`:root`) at the top control the whole palette, so re-theming means editing values there rather than hunting through the file.
- All interactivity (mobile menu toggle, WhatsApp form submission, scroll-reveal animation) lives in the single `<script>` block at the bottom of the file.

---

## Deployment

The site is static (no backend), so any static host works. The recommended path is **Azure Static Web Apps**, since it fits this exact use case: static HTML/CSS/JS, custom domain, and free SSL — all on the free tier.

### Current Status (live)

- **Repo**: https://github.com/Tariqueakhtar/lyndsey-website (branch `main`)
- **Azure resource**: `lyndsey-website` Static Web App, resource group `rg-lyndsey`, subscription "Azure subscription 1"
- **Live URL**: https://jolly-beach-0a1360b0f.1.azurestaticapps.net
- **Deploys automatically** on every push to `main` via the GitHub Actions workflow at `.github/workflows/azure-static-web-apps-jolly-beach-0a1360b0f.yml` (auto-generated by Azure — don't rename/delete it).
- No custom domain attached yet — see step 5 below when ready.

### Credentials & Login

Two separate accounts are involved — don't mix them up.

**GitHub account**: `Tariqueakhtar` (owns the repo at github.com/Tariqueakhtar/lyndsey-website)

**Azure account**: `tarique.pce@gmail.com`
- Tenant: "Default Directory" (`tariquepcegmail.onmicrosoft.com`, tenant ID `012c31b2-db76-40e1-af3f-71c4a63b22c5`)
- Subscription: "Azure subscription 1" (`a58ee7cb-2e05-40d3-9085-0f0292ec4be1`)
- Resource group: `rg-lyndsey`

#### Logging in with Azure CLI

Plain `az login` **will fail** on this account with an MFA error (`AADSTS50076: ... you must use multi-factor authentication`). Always use the device-code flow instead:

```
az login --use-device-code
```

This prints a code and the URL `https://microsoft.com/devicelogin` — open that in a browser, enter the code, sign in as `tarique.pce@gmail.com`, and complete MFA there. When it lists subscriptions, press Enter to accept the default (`Azure subscription 1`).

Verify you're logged into the right place before running anything:
```
az account show --output table
```
Should show subscription ID `a58ee7cb-2e05-40d3-9085-0f0292ec4be1`.

Useful commands once logged in:
```
az staticwebapp list --resource-group rg-lyndsey --output table
az staticwebapp show --name <app-name> --resource-group rg-lyndsey -o json
az staticwebapp secrets list --name <app-name> --resource-group rg-lyndsey --query "properties.apiKey" -o tsv
```

#### Logging in with GitHub CLI (`gh`)

If `gh` isn't installed: `winget install --id GitHub.cli -e`. **Open a new terminal window after installing** — an already-open terminal won't have `gh` on its PATH yet (if it's still not found in a fresh terminal, the binary lives at `C:\Program Files\GitHub CLI\gh.exe` and can be called by full path).

```
gh auth login
```
Choose: **GitHub.com** → **HTTPS** → **Login with a web browser**. It shows a one-time code and opens `github.com/login/device` — enter the code and authorize as `Tariqueakhtar`.

Verify:
```
gh auth status
```
Should show `Logged in to github.com account Tariqueakhtar` with `repo` and `workflow` scopes.

`gh` being authenticated also makes plain `git push` / `git pull` work from the terminal without any separate credential prompt (it registers itself as git's credential helper).

Useful commands once logged in:
```
gh secret list --repo Tariqueakhtar/lyndsey-website
gh secret set <SECRET_NAME> --repo Tariqueakhtar/lyndsey-website    # reads value from stdin — pipe it in, never type it inline
gh run list --repo Tariqueakhtar/lyndsey-website --limit 5
gh run view <run-id> --repo Tariqueakhtar/lyndsey-website --log-failed
```

**Security note:** never paste a GitHub Personal Access Token or an Azure deployment token directly into a chat/command as plain inline text — it ends up in logs/history. Pipe secret values from a variable or file into the command instead (e.g. `$token | gh secret set ...`), and if a token was ever pasted in plain text anywhere, revoke/reset it afterward.

### Why Static Web Apps (not a regular Azure Web App)

- Regular App Service **Free (F1) tier cannot bind a custom domain at all** — you'd need to pay for at least Basic (B1) just to attach a domain.
- Azure Static Web Apps' **Free tier includes custom domains + managed SSL certificates** at no cost, because it's purpose-built for static sites.
- Static content is served from Azure's global CDN, not a compute instance — so unlike App Service F1 (which sleeps after ~20 minutes idle), a Static Web App is available 24/7 with no cold starts.
- Free tier caveat: no formal SLA, and quotas of 250MB storage / environment and 100GB bandwidth/month — both far more than this site needs.

### Steps

1. **Prepare the repo**
   - Rename the HTML file to `index.html`.
   - Confirm `assets/lyndsey-photo.webp` sits alongside it.
   - Push both to a GitHub repository (Azure Static Web Apps deploys via GitHub Actions).

2. **Create the Static Web App**
   - Azure Portal → Create a resource → **Static Web App**
   - Plan type: **Free**
   - Deployment source: **GitHub** — sign in, pick the repo/branch
   - Build details: Framework preset **Custom**, app location `/`, output location leave blank (no build step needed for plain HTML)
   - Azure will auto-create a GitHub Actions workflow in the repo that deploys on every push to the chosen branch

3. **Verify the default deployment**
   - Azure gives you a temporary URL like `https://<random-name>.azurestaticapps.net` — confirm the site loads correctly there first.

4. **Buy/point the domain**
   - Domain purchased separately (Namecheap, GoDaddy, etc.) — not required to be an Azure App Service Domain.

5. **Attach the custom domain**
   - In the Static Web App resource → **Custom domains** → **Add**
   - Choose **CNAME** (for a subdomain like `www.yourdomain.com`) or **A record + TXT** (for the apex/root domain, e.g. `yourdomain.com`)
   - Azure shows you the exact record values to add
   - Add those records at your domain registrar's DNS settings
   - Return to Azure and click **Validate** — propagation can take a few minutes to a few hours

6. **SSL**
   - Free managed certificate is issued automatically once the domain is validated — no extra step needed.

### Troubleshooting

**Oryx build error: "Could not find either 'build' or 'build:azure' node under 'scripts' in package.json"**
Azure's build system (Oryx) runs a Node.js build pass by default. This is a plain static site with no build step, so when creating the Static Web App, **leave "Output location" blank** (not `.`) in the Build Details step. If it's already set wrong, edit the workflow YAML in `.github/workflows/` and add `skip_app_build: true` to the `Azure/static-web-apps-deploy@v1` step's `with:` block, then set `output_location: ""`.

**Deploy fails with "No matching Static Web App was found or the api key was invalid" — even right after resetting the token**
This happened once (2026-09-17) and turned out to be the Azure resource itself stuck in a broken provisioning state, not a secrets/workflow problem. Confirmed by testing the deployment token directly against Azure's servers with the SWA CLI, bypassing GitHub Actions entirely — it was rejected even with a token reset seconds earlier. The fix: delete the Static Web App resource in Azure (`az staticwebapp delete --name <name> --resource-group rg-lyndsey --yes`) and recreate it via the Portal's GitHub-connected flow (see Steps above) — this generates a fresh, working resource plus a new workflow file and matching secret in the repo. Don't bother debugging the token/secret further if you hit this exact error after a fresh reset still fails; go straight to recreating the resource.

**Where the deployment token is stored**: GitHub repo → Settings → Secrets and variables → Actions → a secret named `AZURE_STATIC_WEB_APPS_API_TOKEN_<RESOURCE-NAME-SUFFIX>` (e.g. `..._JOLLY_BEACH_0A1360B0F`), referenced in the workflow file. To get the current token from Azure directly: `az staticwebapp secrets list --name lyndsey-website --resource-group rg-lyndsey --query "properties.apiKey" -o tsv`.

7. **Ongoing updates**
   - Any future edit: push to the connected GitHub branch → GitHub Actions redeploys automatically within a couple of minutes. No manual re-upload required.

### Alternative hosts (if not using Azure)

- **Netlify / Vercel** — drag-and-drop the folder or connect the GitHub repo; free tier includes custom domains + SSL, arguably the fastest path to get live.
- **Traditional cPanel hosting** — upload `index.html` + `assets/` via File Manager/FTP into `public_html`.
- **GitHub Pages** — free, works well for a repo you're already pushing to for Azure deployment; attach the custom domain in repo Settings → Pages.

---

## Full Content Appendix (Verbatim Copy)

Every piece of text on the site, section by section, so this document is a complete standalone record even if `index.html` were ever lost. If the two ever disagree, `index.html` is the source of truth (this appendix should be updated to match after any content edit).

### 1. Top Contact Bar
- Left: "Make your life better." + "Call us **+971 58 579 3525**"
- Center: "Start your **CONFIDENCE JOURNEY** today!"
- Right: "Follow us" + Instagram icon + WhatsApp icon

### 2. Navigation
- Logo: "Lyndsey **Austin**"
- Links: Her Story · The Programme · Work With Me · The Book
- Buttons: "Call Now" · "Start Your Transformation"

### 3. Hero
- Eyebrow: "Be Strong, Confident and You."
- H1: "Transform your pain into purpose."
- Subhead: "Lyndsey Austin helps women rebuild confidence, mindset and strength from the inside out — through 1:1 coaching, group programmes and the bestselling book, Flight of a Butterfly."
- CTA buttons: "Free Discovery Call" (→ WhatsApp) · "Get the Free Guide" (→ #contact)
- Credibility strip:
  - **20+ Years** — coaching & personal training
  - **Bestselling Author** — Flight of a Butterfly
  - **Dubai-Based** — working with women worldwide
- Photo badge: "Lyndsey Austin" / "Confidence Coach · Personal Trainer"

### 4. Featured In Strip
- "As Featured In" → **UAE Stories** (links to https://uaestories.com/lyndsey-austin-inspires-women-to/)

### 5. Meet Lyndsey / Video Section
- Eyebrow: "Meet Lyndsey"
- H2: "A coach who has lived the transformation she teaches"
- Paragraph 1: "Lyndsey Austin is a confidence coach and personal trainer with more than 20 years of experience, and the bestselling author of Flight of a Butterfly. Long before coaching became her career, life had already taught her resilience, patience and empathy — lessons she now brings to every client she works with."
- Paragraph 2: "At 50, she moved to Dubai on her own to build an independent new chapter, shaped by strength, purpose and self-belief. Her work today blends fitness, mindset and spiritual life coaching — helping people transform body, mind and spirit, not just their appearance."
- Video caption overlay: "Watch: Meet Lyndsey (1 min)"
- CTA button: "Message on WhatsApp"

### 6. Pain Points ("Does this sound like you?")
- Eyebrow: "Does this sound like you?"
- H2: "There's more for you. Let's get there."
- Six cards:
  1. **Stuck on autopilot** — "Going through the motions, aware something needs to change, unsure where to start."
  2. **Running low on confidence** — "Self-doubt creeping into decisions, relationships, and how you show up in your own life."
  3. **Carrying the past** — "Old experiences still shaping how you see yourself, long after they should have stopped."
  4. **In a major life transition** — "A new decade, a new city, a health scare, a relationship ending — and a fresh start needed."
  5. **Losing consistency** — "You start strong with new habits and motivation, then life pulls you back to old patterns."
  6. **Ready, but not sure how** — "You're done making excuses. You just need someone who's actually lived it to show you the way."

### 7. Her Story
- Eyebrow: "Her Story"
- H2: "I don't just talk about transformation. I've lived it."
- Intro paragraphs:
  1. "I'm Lyndsey Austin — a confidence coach, personal trainer and bestselling author of Flight of a Butterfly. For over twenty years I've helped people rebuild not just their bodies, but their mindset and their belief in themselves."
  2. "I don't believe transformation is about losing weight or changing how you look. True transformation starts from within — and I know that because I've had to rebuild myself more than once."
  3. Pull quote: "Confidence isn't something you're born with — it's something you build."
  4. "My own life has handed me more turning points than I expected. Each one taught me the same lesson: your past doesn't have to decide your future. It can become your strength."
- Turning point cards:
  - **Turning Point 01 — Starting over at 50, in a new country**: "I left everything familiar behind and moved to Dubai to reinvent my career from nothing. There were months of rebuilding one client, one class at a time. Three years on, I'm exactly where I'm meant to be."
  - **Turning Point 02 — Facing a cancer diagnosis with my mindset intact**: "When I was diagnosed and faced major surgery, I had a choice: fear, or presence. I chose to stay in high spirits and trust the same inner strength that had carried me before. I recovered faster than anyone expected."
  - **Turning Point 03 — Rebuilding my self-worth from nothing**: "At 25, after years in a relationship that had cost me my sense of self, I made the decision that changed everything — I left. I surrounded myself with women who valued themselves, and slowly learned to do the same, for me and for my children."

### 8. Philosophy
- Eyebrow: "My Philosophy"
- H2: "Mind. Body. Purpose. Life."
- Paragraph: "You can't transform a life by changing just one thing. Sometimes the body needs strengthening. Sometimes the mind needs rewiring. Sometimes you simply need someone to believe in you while you learn to believe in yourself again."
- Four cards:
  1. **Mindset** — "Challenging the beliefs and patterns keeping you stuck."
  2. **Body** — "Fitness as a vehicle for confidence, discipline and energy."
  3. **Purpose** — "Turning your hardest chapters into your strongest ones."
  4. **Life** — "Real accountability, so transformation becomes a lifestyle."

### 9. The Butterfly Transformation Programme
- Eyebrow: "My Signature Framework"
- H2: "The Butterfly Transformation Programme"
- Paragraph: "A five-step journey from surviving to thriving. Like a butterfly, lasting change takes courage, patience and trust in the process."
- Five steps:
  1. **Awareness** — "See where you are before deciding where you're going."
  2. **Acknowledgement** — "Healing begins when you accept your truth fully."
  3. **Action** — "Small, courageous steps create extraordinary change."
  4. **Strength** — "Discover the strength that has always been within you."
  5. **Consistency** — "Transformation isn't one moment — it's a lifestyle."

### 10. Work With Me (Pricing)
- Eyebrow: "Work With Me"
- H2: "Wherever you're starting from, there's a next step"
- Paragraph: "Most clients begin with the free guide, then move at their own pace — from the book, to ongoing membership, to deeper 1:1 support. Every enquiry goes straight to WhatsApp so you get a personal reply."
- Pricing cards:
  1. **Flight of a Butterfly** — AED 97 — "My bestselling book on breaking free from self-doubt, plus AED 1,000+ in bonus resources." → button "Get the Book"
  2. **8-Week Group Coaching** (tag: Most Popular) — AED 997 — "Weekly live sessions, a private accountability community and a full confidence workbook." → button "Join the Programme"
  3. **1:1 Confidence Coaching** — AED 3,000–5,000 — "A private, tailored space to rebuild confidence, set boundaries and create lasting transformation." → button "Enquire Now"
- Ladder rows:
  - **Free Confidence Starter Guide** — "A short, practical introduction to rebuilding confidence." — Free
  - **The Confidence Collective — membership** — "Ongoing community, weekly challenges and monthly live sessions." — From AED 197/mo
  - **Retreats & Live Events** — "Immersive, in-person experiences to go deeper on mindset and community." — From AED 5,000

### 11. Inside The Confidence Collective (Modules)
- Eyebrow: "Inside The Confidence Collective"
- H2: "Eight weeks. One clear path back to yourself."
- Paragraph: "Every module builds on the last — designed to move you from feeling stuck to becoming the woman you were always meant to be."
- Eight modules:
  1. Rediscover Who You Are
  2. Letting Go of Limiting Beliefs
  3. Building Unshakable Confidence
  4. Self-Worth & Boundaries
  5. Mindset & Resilience
  6. From Fear to Courage
  7. Purpose & Vision
  8. Becoming the Woman You Were Meant to Be

### 12. Online Programmes
- Eyebrow: "Online Programmes"
- H2: "Or go deeper with a focused course"
- Paragraph: "Self-paced programmes for wherever you are in your journey."
- Six programme cards:
  1. **From Pain to Purpose** (6 weeks) — "Healing from past experiences and creating a new identity."
  2. **Become the Most Confident You** (8 weeks) — "Confidence, limiting beliefs and boundaries."
  3. **The Resilient Woman** (6 weeks) — "Emotional resilience and empowering habits."
  4. **Mindset Reset** (4 weeks) — "Gratitude, visualisation and daily confidence rituals."
  5. **Confidence After 50** (Self-paced) — "Navigating midlife with confidence and purpose."
  6. **From Surviving to Thriving** (Signature) — "Rebuilding your life through courage and purpose."

### 13. In Her Words / Instagram
- Eyebrow: "In Her Words"
- H2: "Flight of a Butterfly"
- Paragraph: "My bestselling book on breaking free from self-doubt and rediscovering your purpose — available now for AED 97, with over AED 1,000 in bonus resources included."
- Quotes:
  1. "Your story doesn't end with pain — that is often where your purpose begins." — On Pain to Purpose
  2. "You are never too old and it's never too late to start again." — On starting over at 50
  3. "Every challenge taught me resilience, courage, and that growth happens outside your comfort zone." — On her own journey
- Instagram strip: "Follow the everyday transformation" — "Real stories, real mindset shifts, and daily confidence prompts — shared on Instagram between sessions." → button "Follow @lyndseya_transformations"

### 14. Contact
- Eyebrow: "Start Here"
- H2: "Your confidence is waiting. Message Lyndsey directly."
- Paragraph: "Call, WhatsApp, or fill in the form — every enquiry and registration goes straight to Lyndsey on WhatsApp, so you get a real, personal reply."
- Contact cards: "+971 58 579 3525" (tel link) · "WhatsApp: +971 58 579 3525" (wa.me link)
- Form fields:
  - First name (required)
  - WhatsApp number (required, placeholder "+971 5X XXX XXXX")
  - "What are you most ready for?" dropdown: Just the free guide, for now / The book — Flight of a Butterfly / The Confidence Collective membership / 8-week group coaching / 1:1 confidence coaching
  - "Anything you'd like Lyndsey to know? (optional)" textarea
  - Submit button: "Send via WhatsApp"
  - Form note: "Submitting opens WhatsApp with your details pre-filled, ready to send to Lyndsey directly."
- On submit, JS builds this message template and opens it in WhatsApp:
  `Hi Lyndsey, my name is {name}. My WhatsApp number is {phone}. I'm most interested in: {interest}. Additional note: {message}` (the "Additional note" clause only appears if the message field isn't empty)

### 15. Footer
- Brand: "Lyndsey **Austin**"
- Links: Her Story · The Programme · Work With Me · The Book · Contact
- Social icons: Instagram · WhatsApp
- Bottom line: "© 2026 Lyndsey Austin Transformations. All rights reserved." / "Dubai, UAE · +971 58 579 3525"

### 16. Mobile Sticky Bar
- "Call Now" (tel link) · "WhatsApp" (wa.me link, pre-filled: "Hi Lyndsey, I'd like to start my transformation.")

### Pre-filled WhatsApp Messages Used Around the Site
For reference, these are the exact pre-filled texts wired into different CTAs (all sent to `wa.me/971585793525`):
- Top bar / generic WhatsApp icon: "Hi Lyndsey, I'd like to know more about coaching with you."
- Hero "Free Discovery Call": "Hi Lyndsey, I'd like to book a free discovery call."
- Meet Lyndsey "Message on WhatsApp": "Hi Lyndsey, I'd love to learn more about working with you."
- Book pricing card: "Hi Lyndsey, I'd like to get a copy of Flight of a Butterfly."
- Group coaching pricing card: "Hi Lyndsey, I'd like to join the 8-week group coaching programme."
- 1:1 coaching pricing card: "Hi Lyndsey, I'd like to enquire about 1:1 confidence coaching."
- Mobile sticky bar: "Hi Lyndsey, I'd like to start my transformation."
- Contact form: built dynamically from the name/phone/interest/message fields (see Section 14 above).