# Current Inventory — Summit Group

One-page property inventory site with a built-in admin panel.
Static HTML, no build step. Inventory is managed at **/admin/**.

```
summit-inventory/
├─ index.html        the page
├─ listings.json     your properties live here
├─ thanks.html       where the contact form lands
├─ admin/
│  ├─ index.html     the CMS panel
│  └─ config.yml     what fields appear in the panel
└─ images/
   ├─ logo.png
   └─ uploads/       photos you add through the panel land here
```

---

## Updating inventory (the easy way)

Go to **inventory.summit-realestate-group.com/admin/**, log in, and you get a form.

- **Add a property** — click *Add Property*, fill in the fields, upload a photo.
- **Remove one** — click the trash icon on that property.
- **Reorder** — drag the handle.
- **Change a price** — type the new number.

Then click **Publish**. The site updates itself in about a minute. No code, no redeploying.

Field notes:
- **Price** — numbers only. `749000`, not `$749,000`.
- **Beds / Baths / Sq Ft** — enter `0` and that cell disappears from the card. Useful for land or a commercial suite.
- **Baths** accepts halves: `2.5`.
- **Type** — Residential or Commercial. Sets the badge on the photo.
- **Listing link** — optional. Leave it blank and the card shows a Call button instead.

Everything else on the page updates itself: the property count, the badges, the "Updated [month]" line, and the dropdown in the contact form.

---

## One-time setup for the admin panel

The panel needs the site connected to GitHub — it works by saving changes to your repo. Drag-and-drop deploys can't support it.

**1. Put the site on GitHub**
Create a new repository, upload this folder to it. Name the default branch `main` (GitHub's default).

**2. Connect it to Netlify**
Netlify → *Add new site* → *Import an existing project* → pick the repo.
Build command: leave empty. Publish directory: `/`.

**3. Turn on Netlify Identity**
Site configuration → *Identity* → **Enable Identity**.
Then under *Registration*, set it to **Invite only** so strangers can't sign up.

**4. Turn on Git Gateway**
Identity → *Services* → **Enable Git Gateway**. This is what lets the panel save to GitHub.

**5. Invite yourself**
Identity → *Invite users* → enter your email. You'll get an email with a link; click it, set a password, and you're in. Repeat for anyone else who should manage listings.

**6. Visit /admin/ and log in.**

If the branch in your repo isn't called `main`, open `admin/config.yml` and change the `branch:` line to match.

---

## Updating inventory (by hand, if you prefer)

Open `listings.json` and edit it directly. Each property is one block:

```json
{
  "type": "Residential",
  "price": 749000,
  "address": "Golden Gate Estates",
  "city": "Naples, FL",
  "beds": 4,
  "baths": 3,
  "sqft": 2480,
  "image": "/images/uploads/photo.jpg",
  "link": ""
}
```

Every block needs a comma after its closing `}` except the last one. Save, commit, push — or drag the folder into Netlify if you're not using GitHub.

---

## Deploying without the admin panel

If you skip the GitHub setup, the site still works — just drag the `summit-inventory` folder onto app.netlify.com. You'd then update inventory by editing `listings.json` and re-dragging the folder.

## Pointing your domain at it

Netlify → *Domain management* → *Add a domain* → `inventory.summit-realestate-group.com`.

In your DNS:

```
Type:   CNAME
Name:   inventory
Value:  your-site-name.netlify.app
TTL:    3600
```

Only the `inventory` subdomain is affected — your main site keeps running untouched. HTTPS is issued automatically once DNS resolves.

## Contact form notifications

Netlify → *Forms* → `inventory-inquiry` → *Settings & usage* → *Form notifications* → *Add notification* → *Email notification*.

---

## Design system

| Token | Value | Used for |
|---|---|---|
| `--black` | `#0a0a0a` | nav, dark sections, footer |
| `--white` | `#f9f7f5` | page background |
| `--warm` | `#60564d` | accent — the brown from your logo |
| `--warm-light` | `#8a7d73` | secondary text |
| `--warm-pale` | `#e8e2db` | hairlines, borders |
| `--warm-ultra` | `#f4f0ec` | hero + form background |
| `--mid` | `#2a2420` | contact band |

Cormorant Garamond (display) + Montserrat (body).

## A note on previewing locally

If you open `index.html` by double-clicking it, the listings won't appear — browsers block local file reads for security. It works fine on the live site. To preview locally, run `python3 -m http.server` in this folder and visit `localhost:8000`.

## Compliance

The footer carries Equal Housing Opportunity, the Samson Companies, LLC brokerage line, license #SL3389594, and a standard "deemed reliable but not guaranteed" disclaimer. Check these against Samson's and your local MLS's advertising rules — some MLSs require the brokerage's office phone or a specific IDX disclosure verbatim.
