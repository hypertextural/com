# CSV

## Products Import

- Delete all items in RecycleBin ^1xtqfc
- Delete `Pending` category
- Add `"…";"Dispenser System=Gilbarco® - Encore®";` to the `Product Custom Fields` column.
- Add product weights as available, `Product Weight`.
- Put the products brand/manufacturer in the `Brand Name` field/column.

## Users Import

- `Customer Group` for linking with Price Lists

## Price Lists

Example…

| Price List ID | Price List Name | Product Name | Variant ID | SKU   | Currency | New Price | New Sale Price | New MSRP | MAP |
| ------------- | --------------- | ------------ | ---------- | ----- | -------- | --------- | -------------- | -------- | --- |
| 1             | Group C         | Q13180-20BS  | 10839      | 10276 | usd      | 91.34     |                |          |     |

# Theme

- Upload a Logo, it's not in the theme settings, but the level up under storefront.
	- Same goes for the home HERO / CAROUSEL / THING
- [[On BigCommerce Imports & Deletions#CSS Truncation of 'Description' on Grid Listings|Truncate Description]]
- [[Hide Core Price for Public]]
- Add SVG logo via WebDAV to `/content/img/logo.svg` and put in the header config.
	- [Details from Flair](https://www.flairthemes.com/article/196-what-image-file-formats-can-i-use-for-my-logo)
# Misc BigCommerce / Theme Notes

- Frustrating system
- I would like to use the "Featured Categories" feature, but it requires keeping products in a weird duplicate category.
