# MOQ Comparison Spreadsheet Template

> Originally developed for a supplement subscription business, now genericized for universal use.

Copy this into a Google Sheet or Excel. Use one row per supplier, one column per criterion. Update as quotes come in.

---

## Spreadsheet Structure

| Column | Header | Data Type | Notes |
|--------|--------|-----------|-------|
| A | **Supplier Name** | Text | |
| B | **Contact Name** | Text | |
| C | **Contact Email** | Text | |
| D | **Date Contacted** | Date | |
| E | **Date Responded** | Date | Track response speed |
| F | **Supplier Type** | Dropdown | Manufacturer / Co-packer / White-label |
| G | **GMP Certified?** | Yes/No | Ask for certificate number |
| H | **Other Certs** | Text | ISO, BRC, Organic, etc. |
| I | **MOQ (units)** | Number | Per SKU |
| J | **MOQ (£)** | Currency | Total order value at MOQ |
| K | **Price per Unit** | Currency | At quoted MOQ |
| L | **Price per Unit at 1,000** | Currency | Ask for tiered pricing |
| M | **Price per Unit at 5,000** | Currency | |
| N | **Setup / NPD Fee** | Currency | One-time or per SKU |
| O | **Artwork / Label Fee** | Currency | |
| P | **Lead Time (weeks)** | Number | From order to delivery |
| R | **Powder Blending** | Yes/No | In-house or outsourced? |
| S | **Sachet/Stick-Pack Filling** | Yes/No | |
| T | **Allergen Handling** | Text | Separate line / validated clean-down / none |
| U | **Minimum Run Size** | Number | Smallest run they'll do |
| V | **Packaging Options** | Text | Sachet size, material, print method |
| W | **Label/Regulatory Support** | Yes/No | |
| X | **Storage / Call-Off** | Yes/No | Can they warehouse and release on demand? |
| Y | **Payment Terms** | Text | e.g., 50% upfront, 50% on delivery |
| Z | **Samples Available?** | Yes/No | |
| AA | **Sample Cost** | Currency | |
| AB | **References Provided?** | Yes/No | |
| AC | **Communication Speed** | 1–5 | 1 = slow, 5 = immediate |
| AD | **Overall Score** | 1–10 | Weighted average of all criteria |
| AE | **Notes** | Text | Any red flags, standout positives |
| AF | **Status** | Dropdown | Contacted / Quoted / Sampled / Shortlisted / Rejected / Contracted |

---

## Conditional Formatting Rules (Visual Scoring)

Apply these to make the sheet scannable:

| Range | Rule | Colour |
|-------|------|--------|
| G (GMP) | = "No" | Red background |
| J (MOQ £) | > [first-run budget] | Yellow background |
| P (Lead Time) | > 8 | Yellow background |
| P (Lead Time) | > 12 | Red background |
| AC (Comm Speed) | < 3 | Yellow background |
| AF (Status) | = "Shortlisted" | Green background |
| AF (Status) | = "Rejected" | Grey background |

---

## Quick-View Summary Row

Add a "TARGET" row at the top for reference:

| Field | Target |
|-------|--------|
| MOQ (units) | ≤ 500 |
| MOQ (£) | ≤ [first-run budget] |
| Price per unit | ≤ £4.00 |
| Lead time | ≤ 6 weeks |
| Communication speed | ≥ 4/5 |

---

## Example Completed Row

| Field | Example Value |
|-------|---------------|
| Supplier Name | UK Powder Co |
| Contact Name | Sarah Jones |
| Contact Email | sarah@ukpowderco.com |
| Date Contacted | 2026-06-29 |
| Date Responded | 2026-06-30 |
| Supplier Type | Co-packer |
| GMP Certified? | Yes (SGS, cert #12345) |
| Other Certs | ISO 9001, BRC |
| MOQ (units) | 500 |
| MOQ (£) | £1,850 |
| Price per Unit | £3.70 |
| Price per Unit at 1,000 | £2.90 |
| Price per Unit at 5,000 | £2.10 |
| Setup / NPD Fee | £250 |
| Artwork / Label Fee | £150 |
| Lead Time (weeks) | 5 |
| Powder Blending | Yes (in-house) |
| Sachet/Stick-Pack Filling | Yes |
| Allergen Handling | Separate line for allergen-free |
| Minimum Run Size | 250 |
| Packaging Options | 10g sachet, matte foil, digital print |
| Label/Regulatory Support | Yes |
| Storage / Call-Off | Yes (£0.10/unit/month) |
| Payment Terms | 50% order, 50% delivery |
| Samples Available? | Yes |
| Sample Cost | £50 + postage |
| References Provided? | Yes (2 startups) |
| Communication Speed | 5 |
| Overall Score | 9 |
| Notes | Very responsive, startup-friendly, flexible on MOQ |
| Status | Shortlisted |
