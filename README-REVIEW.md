# LeiteMelo — Product Review (internal)

Private page at **https://lmengravings.com/review/**, which isn't linked anywhere and is set to noindex. Robots.txt also blocks `/review/`.
**Data is split for privacy:**
- **Private repo `tobotMelo/leitemelo-products` → `products.json`:** every proposal, with notes, QA notes, research ranges and hold reasons. Only someone with your token can read it.
- **Public repo `tobotMelo/leitemelo-site` → `data/products.json`:** only publishable items, with public fields only. The review page regenerates and commits this file automatically after every Accept, Reject or Reset. Never edit it by hand.

## 1. Create your token (once, ~2 min)
1. Sign in to GitHub as **tobotMelo**.
2. Open https://github.com/settings/personal-access-tokens/new. To get there by clicking: profile photo → **Settings** → **Developer settings** → **Personal access tokens** → **Fine-grained tokens** → **Generate new token**.
3. **Token name:** `LeiteMelo review`. **Expiration:** your choice, for example 90 days or 1 year.
4. **Resource owner:** tobotMelo. **Repository access:** *Only select repositories* → select **both** `tobotMelo/leitemelo-products` and `tobotMelo/leitemelo-site`.
5. **Permissions → Repository permissions → Contents:** **Read and write**. Leave everything else at No access. *Metadata: Read* is added automatically.
6. Click **Generate token** and copy the `github_pat_…` value. GitHub shows it only once.

## 2. Log in
Open https://lmengravings.com/review/, paste the token, and tap **Log in**.
The page checks with GitHub that the token belongs to tobotMelo and can write to **both** repos. If the check fails, you see only the login box.
The token is saved in that browser's local storage. **Log out** removes it. If the token is lost or a device is lost, revoke it on GitHub.

## 3. Review
- Use the tabs All / Pending / Accepted / Rejected to filter.
- **Accept** saves `products.json` to the private repo, then regenerates the public `data/products.json`. The product appears on the public site after GitHub Pages rebuilds, usually 1–10 minutes.
- **Reject** asks for an optional note, then saves. **Reset to pending** undoes either one.
- Every action is a commit to `main`, so you can see history and undo it on GitHub. A message confirms success or shows the error.


## QA gate
Every product has a `qa` field, set to `pending`, `approved` or `flagged`, plus an optional `qaNote`. All products start as `pending`.
- **Accept** stays disabled until **all** of these are true. The card shows the reason when it is locked.
  - `qa` is `approved`.
  - `priceConfirmed` is `true`, with `price` set to your real price.
  - `hold` is `false`.
  - Today is on or before `activeUntil`, if the product has one.
- **Reject** always works. The same rules are checked again when saving, and again on the public site.
- **`researchRange`** is competitor research, **not your price**. It appears only on the review page. Current price: **$35 for every product**, all confirmed. To change a price, the Webmaster edits `price`. An optional `priceLabel`, for example `"Launch price"`, appears in small text above the price on both review and public cards. None is set yet.
- **`hold`/`holdReason`:** held cards show a Hold badge, can't be accepted, and are never public. Currently held: p018 (shirt kit unconfirmed), p021 (pet chew, waiting on Jardyson's decision), p024 (ADA risk; suggested "decorative raised-text sign, not ADA-certified").
- **`availability`**, for example "Available after the O1 arrives in November", appears on both review and public cards.
- **`activeUntil`**, for example p023 at `2026-10-31`, hides the product from the public site and locks Accept after that date (Central time).
- **How QA approves:** the QA/QC Reviewer tells the **Webmaster** the result for each product, for example "p006 approved" or "p012 flagged: text too small". The Webmaster edits `products.json` in the **private** repo, setting `"qa": "approved"` or `"qa": "flagged"` and `"qaNote": "…"`, and commits. The review page shows the QA badge and note the next time it loads.

## 4. Adding new products (team)
Add an object to the `products` array in **`products.json` in the private repo `leitemelo-products`** with `"status": "Pending"`:
```json
{"id":"p026-short-slug","title":"…","description":"…","personalization":"…","price":"TBD","priceConfirmed":false,"researchRange":"$20–30","hold":false,"holdReason":"",
 "machine":"F1 Ultra | P2S | O1 Omni","method":"…","image":"images/products/p026-short-slug.jpg",
 "image_is_placeholder":false,"status":"Pending","note":"","qa":"pending","qaNote":"","updated":"YYYY-MM-DD","internal_notes":"…","source":"…"}
```
**Designer:** put mockups in `images/products/` (square JPG/PNG, about 1200px) and point the product's `"image"` at the file.
Until a mockup exists, products use a generated placeholder in `images/placeholders/`, marked "PLACEHOLDER — NO MOCKUP".

## Privacy, honestly
Proposals, notes, research ranges and hold reasons now live in the **private** repo. The public site repo contains only items you accepted, with public fields, plus the review page's code, which holds no secrets.
The token is what protects the page: without it the page shows nothing and nothing can be changed.
Mockup images in `images/products/` in the public repo are publicly readable, even for products that haven't been accepted.
