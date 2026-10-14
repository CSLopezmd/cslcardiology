# How to put cslcardiology.com online (free, using GitHub Pages)

For: S. Wagner  
Site files: this folder (`csl-github-site`)  
DNS values below were checked against GitHub's official docs (docs.github.com, "Managing a custom domain for your GitHub Pages site") on Oct 11, 2026. GitHub can change them, so glance at that page again right before step 4.

**Nothing has been published, bought, or changed yet.** These are the steps for you to do when you're ready. Plan on about 1 hour of work, then waiting (up to 24 hours) for DNS and the HTTPS padlock.

---

## Before you start: two ground rules

1. **No health information on the site, ever.** Free hosting does not come with a HIPAA agreement (BAA). The contact page only asks for name, phone, and best callback time. Keep it that way. Don't add a symptoms box or an upload button.
2. **The GitHub repository must be Public** for free Pages. That means anyone can see the website files. That's fine for this site, because it is only the same public information. Never put patient information, passwords, or internal documents in this repo.

---

## Step 1 — Buy or keep the domain at GoDaddy

- If you **already own** cslcardiology.com at GoDaddy: nothing to buy. Log in at godaddy.com → **My Products** → find **cslcardiology.com**, and confirm it's active and not about to expire. Turn on **auto-renew** so it never lapses.
- If you **don't own it yet**: search for `cslcardiology.com` on godaddy.com and buy just the domain (about one year at a time). You do **not** need GoDaddy's website builder, hosting, or email add-ons. Skip all of those upsells.
- If the domain is currently held somewhere else (for example the old Squarespace/Square account), you can leave it there and edit the DNS records there instead, or transfer it to GoDaddy first. Step 4 works the same on any registrar.
- Optional but recommended at purchase time: keep **Domain Privacy** on, so your personal contact info isn't public.

## Step 2 — Create a free GitHub account and a repository

1. Go to **github.com** → **Sign up**. Use a practice email address if you can (e.g. the practice's address, not a personal one). Pick a username that fits the practice, such as `cslcardiology`. This username is part of the temporary web address `cslcardiology.github.io`.
2. Turn on two-step login (Settings → Password and authentication). This is important, because this account controls your website.
3. Click the **+** (top right) → **New repository**.
   - **Repository name:** `cslcardiology.github.io`  
     (Using `<your-username>.github.io` makes it a "user site", which keeps things simplest. If you choose a different username, the name must be that username + `.github.io`. If you'd rather name it something else, like `csl-cardiology-website`, that works too. Just be aware that the temporary address then includes the repo name.)
   - Set it to **Public**.
   - Tick **Add a README file** (this just lets you upload on the next screen).
4. Upload the website files:
   - In the new repo, click **Add file → Upload files**.
   - Drag in the **contents** of the `csl-github-site` folder (not the folder itself): `index.html`, `meet-the-doctor.html`, `services.html`, `contact.html`, `404.html`, `styles.css`, `CNAME`, `robots.txt`, `sitemap.xml`, `favicon.ico`, `favicon-32x32.png`, `apple-touch-icon.png`, and the whole `assets` folder. 
   - You do **not** need to upload `DEPLOY.md` or the `previews` folder.
   - Make sure `index.html` ends up at the top level of the repo, not inside a subfolder.
   - Scroll down and click **Commit changes**.

## Step 3 — Turn on GitHub Pages

1. In your repo, click **Settings** → **Pages** (left sidebar, under "Code and automation").
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Under **Branch**, choose **main** and folder **/ (root)**, then click **Save**.
4. After a minute or two, refresh. A message shows "Your site is live at …". Open that address (like `https://cslcardiology.github.io`) and check all four pages.
5. **Do this next, before touching GoDaddy:** in the same Pages screen, under **Custom domain**, type `cslcardiology.com` and click **Save**. (GitHub's docs say to add the domain here *before* setting up DNS, so nobody else can claim it.) The site already includes a `CNAME` file with this name, so it will probably be filled in for you.
6. *Recommended:* verify the domain with GitHub. Click your profile picture → **Settings** → **Pages** → **Add a domain**, and follow the prompt. GitHub gives you a TXT record to add at GoDaddy, using the same DNS screen as Step 4. This stops anyone else from ever attaching your domain to their GitHub site.

The page will say "DNS check unsuccessful" until Step 4 is done. That is normal.

## Step 4 — Point the domain at GitHub (GoDaddy DNS)

1. Log in to GoDaddy → **My Products** → next to **cslcardiology.com** click **DNS** (or "Manage DNS").
2. **Delete the old records that conflict:**
   - Any existing **A** record with name `@` (often "Parked" / "WebsiteBuilder Site"). 
   - Any existing **CNAME** record named `www` (GoDaddy often has one pointing to `@` or to the old site).
   - Any **AAAA** record for `@` that points somewhere else.
   - **Leave alone** anything else, especially **NS**, **MX**, **TXT** (email and verification records), and `_domainconnect`. If you use email at this domain, deleting MX records would break it.
3. **Add four A records.** Click **Add New Record** for each:

   | Type | Name | Value | TTL |
   |------|------|-------|-----|
   | A | `@` | `185.199.108.153` | 1 hour (default is fine) |
   | A | `@` | `185.199.109.153` | 1 hour |
   | A | `@` | `185.199.110.153` | 1 hour |
   | A | `@` | `185.199.111.153` | 1 hour |

4. **Optional (IPv6, recommended by GitHub in addition to A records):** add four AAAA records, Name `@`:
   `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`.
5. **Add the www record** so `www.cslcardiology.com` works too:

   | Type | Name | Value | TTL |
   |------|------|-------|-----|
   | CNAME | `www` | `<your-github-username>.github.io` (for example `cslcardiology.github.io`) | 1 hour |

   Use **only** your username + `.github.io`. Do **not** include the repository name, and don't use a long `…pages.github.io` address.
6. Don't create any wildcard (`*`) records. GitHub warns they are a security risk.
7. Wait. Changes usually take minutes but can take **up to 24 hours**. 
8. Check it: back in GitHub → Settings → Pages, the "DNS check" should turn green. You can also try the free tool at dnschecker.org (search `cslcardiology.com`, record type A).

## Step 5 — Turn on HTTPS (the padlock)

1. In GitHub → repo → **Settings → Pages**, once the DNS check passes, GitHub automatically requests a free security certificate. This can take **up to 24 hours** after DNS is correct.
2. When the **Enforce HTTPS** checkbox becomes clickable, tick it. (If it's greyed out, wait and refresh later. If it's stuck for more than a day, remove the custom domain, click Save, then re-add it and Save again to restart the process.)
3. Visit **https://cslcardiology.com** and **https://www.cslcardiology.com**. Both should load with a padlock, and one will redirect to the other automatically.

---

## After it's live

- **Test the Request an Appointment button** on the Contact page. It opens an email in the visitor's email app, addressed to CSLCardiology@Outlook.com, with name, phone, and callback time. There is no server and nothing is stored on the website. Send yourself a test.
- **Test the phone number** on a cell phone. Tapping it should start a call.
- **Making changes later:** in the GitHub repo, click a file (for example `contact.html`) → pencil icon → edit → **Commit changes**. The site updates in about a minute. Or ask for help and we'll update the files for you.
- **Google:** to help people find the site, you can add it at search.google.com/search-console. This is optional. A sitemap is already included at `/sitemap.xml`.
- **Calendar reminder:** renew the GoDaddy domain each year (auto-renew helps), and keep the GitHub account's email and two-step login current.

## If something goes wrong

- *Site shows GitHub's 404 page:* make sure `index.html` is at the top level of the repo and Pages is set to the `main` branch / root.
- *"Domain is already taken" in GitHub:* the domain may be attached to another repo. Remove it there first (Settings → Pages → Custom domain → Remove).
- *Old website still appears:* old DNS records are still there (Step 4.2), or your browser/DNS is cached. Wait, and try a private window.
- *Email stopped working:* an MX record was deleted by mistake. Re-add what GoDaddy or your email provider lists.
