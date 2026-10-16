# LeiteMelo — Product Review (internal)

Private page at **https://lmengravings.com/review/**, which isn't linked anywhere and is set to noindex. Robots.txt also blocks `/review/`.
Product proposals are stored in `data/products.json`. The public site shows only products marked **Accepted**, in its "New Collection" section.

## 1. Create your token (once, ~2 min)
1. Sign in to GitHub as **tobotMelo**.
2. Open https://github.com/settings/personal-access-tokens/new. To get there by clicking: profile photo → **Settings** → **Developer settings** → **Personal access tokens** → **Fine-grained tokens** → **Generate new token**.
3. **Token name:** `LeiteMelo review`. **Expiration:** your choice, for example 90 days or 1 year.
4. **Resource owner:** tobotMelo. **Repository access:** *Only select repositories* → `tobotMelo/leitemelo-site`.
5. **Permissions → Repository permissions → Contents:** **Read and write**. Leave everything else at No access. *Metadata: Read* is added automatically.
6. Click **Generate token** and copy the `github_pat_…` value. GitHub shows it only once.

## 2. Log in
Open https://lmengravings.com/review/, paste the token, and tap **Log in**.
The page checks with GitHub that the token belongs to tobotMelo and can write to leitemelo-site. If the check fails, you see only the login box.
The token is saved in that browser's local storage. **Log out** removes it. If the token is lost or a device is lost, revoke it on GitHub.

## 3. Review
- Use the tabs All / Pending / Accepted / Rejected to filter.
- **Accept** saves `data/products.json` to GitHub. The product appears on the public site after GitHub Pages rebuilds, usually 1–10 minutes.
- **Reject** asks for an optional note, then saves. **Reset to pending** undoes either one.
- Every action is a commit to `main`, so you can see history and undo it on GitHub. A message confirms success or shows the error.


## QA gate
Every product has a `qa` field, set to `pending`, `approved` or `flagged`, plus an optional `qaNote`. All products start as `pending`.
- **Accept** stays disabled, with the reason shown on the card, until `qa` is `approved` **and** `price` is a real price rather than `TBD`. **Reject** always works.
- The public site also shows only products that are Accepted **and** QA approved **and** have a real price, as a second safeguard.
- **How QA approves:** the QA/QC Reviewer tells the **Webmaster** the result for each product, for example "p006 approved" or "p012 flagged: text too small". The Webmaster edits `data/products.json`, setting `"qa": "approved"` or `"qa": "flagged"` and `"qaNote": "…"`, and commits. The review page shows the QA badge and note the next time it loads.

## 4. Adding new products (team)
Add an object to the `products` array in `data/products.json` with `"status": "Pending"`:
```json
{"id":"p026-short-slug","title":"…","description":"…","personalization":"…","price":"$20–30 or TBD",
 "machine":"F1 Ultra | P2S | O1 Omni","method":"…","image":"images/products/p026-short-slug.jpg",
 "image_is_placeholder":false,"status":"Pending","note":"","qa":"pending","qaNote":"","updated":"YYYY-MM-DD","internal_notes":"…","source":"…"}
```
**Designer:** put mockups in `images/products/` (square JPG/PNG, about 1200px) and point the product's `"image"` at the file.
Until a mockup exists, products use a generated placeholder in `images/placeholders/`, marked "PLACEHOLDER — NO MOCKUP".

## Privacy, honestly
The repository is **public**. Anyone who looks can read `data/products.json`, which holds every proposal including rejected ones, notes, internal_notes and prices, along with the review page's code.
The token is what protects the page: without it the page shows nothing and nothing can be changed.
If the proposals need to be confidential, move the data to a private repo. That needs a different setup, because GitHub Pages serves only public content on the free plan.
