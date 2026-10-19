# gdrive_rclone — Publisher site & Privacy Policy

Static [GitHub Pages](https://pages.github.com/) site that provides the
**Application home page** and **Privacy Policy** required by Google's OAuth
consent screen when creating your own Google Drive client ID for
[rclone](https://rclone.org/drive/#making-your-own-client-id).

Google requires a homepage URL and a privacy policy URL on a domain you control
before it will let you **publish** an OAuth consent screen (even for a personal,
single-user app). GitHub Pages is a free, reliable place to host both.

## What's in this repo

| File           | Purpose |
| -------------- | ------- |
| `index.html`   | Application home page / publisher information. |
| `privacy.html` | Full privacy policy (Google API Limited Use compliant). |
| `style.css`    | Shared styles for both pages (light/dark aware). |
| `.nojekyll`    | Tells GitHub Pages to serve the files as-is (no Jekyll build). |
| `README.md`    | This file. |

Once deployed, the two URLs Google needs are:

- **Home page:** `https://kpoeppel.github.io/gdrive_rclone/`
- **Privacy policy:** `https://kpoeppel.github.io/gdrive_rclone/privacy.html`

---

## 1. Publish the site on GitHub Pages

The remote `origin` is already configured
(`git@github.com:kpoeppel/gdrive_rclone.git`) and the default branch is `main`.

### 1.1 Commit and push

```bash
cd /home/korbip/Programming/Projects/gdrive_rclone

git add .
git commit -m "Add GitHub Pages home page and privacy policy for Google OAuth"

git push -u origin main
```

> If the push is rejected, make sure your SSH key is loaded
> (`ssh-add -l`) and that you have access to the repo
> (`ssh -T git@github.com`). You can also authenticate with the GitHub CLI
> (`gh auth login`) or use a personal access token over HTTPS.

### 1.2 Enable GitHub Pages

1. Open the repository on GitHub and go to **Settings → Pages**.
2. Under **Build and deployment → Source**, choose **Deploy from a branch**.
3. Under **Branch**, select `main` and the folder `/ (root)`, then click **Save**.
4. Wait ~1–2 minutes. GitHub will show:
   > Your site is live at `https://kpoeppel.github.io/gdrive_rclone/`

### 1.3 Verify

Open the two URLs above in a browser (incognito is a good check) and confirm:

- The home page loads and shows the publisher information.
- The privacy policy page loads and is linked from the home page.

If the page 404s, check **Settings → Pages** for a build error, and confirm
`.nojekyll` was pushed (some Jekyll setups otherwise ignore files).

---

## 2. Configure the Google Cloud OAuth consent screen

In the [Google Cloud Console](https://console.cloud.google.com/) for the project
where you enabled the **Google Drive API**:

1. Go to **APIs & Services → OAuth consent screen** (a.k.a. **Branding** /
   **Audience**).
2. Fill in **Branding → App information**:
   - **Application name:** e.g. `rclone` (or any name you like).
   - **User support email:** your email.
   - **Application home page:** `https://kpoeppel.github.io/gdrive_rclone/`
   - **Application privacy policy link:** `https://kpoeppel.github.io/gdrive_rclone/privacy.html`
   - *(optional)* **Application terms of service link:** leave blank or add one.
3. Under **Authorized domains**, add the domain used by the pages above:
   - Try `github.io` first.
   - If the console rejects it (because `github.io` is a *public suffix* and not a
     registrable "top private domain"), add `kpoeppel.github.io` instead.

   For a **personal app in Testing / unverified use**, Google does not require
   Search Console verification of this domain. Verification only becomes relevant
   if you later request full app verification for sensitive/restricted scopes.
4. Click **Save**.
5. Go to **Audience**, add yourself under **Test users**, then click
   **Publish app** (the button becomes available once the home page and privacy
   policy URLs are saved).

### Authorized domains: the details

Google's consent screen validates `Authorized domains` against the Public Suffix
List. `github.io` is itself a public suffix, so sometimes the console prefers the
"top private domain", which for
`https://kpoeppel.github.io/gdrive_rclone/` is `kpoeppel.github.io`.

- Use `github.io` if accepted (simplest).
- Otherwise use `kpoeppel.github.io`.
- Both work for a personal/testing app; neither needs DNS changes.

If Google ever insists on domain ownership verification, the robust alternative
is a **custom domain**:

1. Add a `CNAME` file to this repo containing your domain, e.g. `example.com`.
2. Point the domain's apex/`www` DNS records at GitHub Pages
   (A records `185.199.108.153`, `.109.153`, `.110.153`, `.111.153`, or an
   ALIAS/ANAME to `kpoeppel.github.io`).
3. Verify the domain in
   [Google Search Console](https://search.google.com/search-console)
   (Domain property via DNS TXT record is easiest).
4. Use that domain for the home page, privacy policy and authorized domain.

---

## 3. Local preview

No build step is required. To preview locally:

```bash
cd /home/korbip/Programming/Projects/gdrive_rclone
python3 -m http.server 8000
```

Then open <http://localhost:8000/>.

---

## 4. Customizing

Before pushing, update these placeholders if your details differ:

- **Publisher name:** `Korbinian Pöppel` (in `index.html` and `privacy.html`).
- **Contact:** the public pages link to GitHub Issues. If Google requires an
  email, use the OAuth consent screen's **User support email** field, which is
  not displayed publicly.
- **GitHub username / repo:** `kpoeppel` and `gdrive_rclone` in the URLs.
- **Effective date:** `2026-10-20` in both pages.

If you use a custom domain, replace all `kpoeppel.github.io/gdrive_rclone`
URLs and add a `CNAME` file.

---

## 5. Why the privacy policy matters

Google's OAuth review rejects vague or incomplete privacy policies. This one
explicitly covers the items Google looks for:

- clear publisher identity and contact method;
- exactly which Google API scopes/data are accessed;
- how the data is used, stored, shared and retained;
- confirmation that data is **not** sold or used for advertising/AI training;
- the **Google API Services User Data Policy, including Limited Use**
  disclosure;
- how to revoke access and delete data;
- GDPR (EU/UK), CCPA/CPRA rights, security, children's privacy and a
  change-notification clause.

Keep it accurate: if you change what the app does (for example, add a server or
new scopes), update `privacy.html` accordingly.

---

## 6. Keeping the site up to date

Any push to `main` that changes the HTML/CSS is deployed automatically by GitHub
Pages. After editing, just:

```bash
git add .
git commit -m "Update privacy policy"
git push
```

The public URLs stay the same, so nothing needs to change in the Google Cloud
Console.

## License / attribution

The pages describe a personal use of [rclone](https://rclone.org/) and the
[Google Drive API](https://developers.google.com/drive). rclone is licensed
under the MIT License and is not affiliated with this repository.
