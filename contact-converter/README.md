# Contact Converter

Converts Zoho Books contact exports into the Ocealgo party-import Excel format.

**Live:** https://ocealgo.github.io/internal-tools/contact-converter/

---

## What it does

The Ocealgo party-import template needs 12 specific columns (Name, Phone, Address, Place/Area, District, State, Pincode, Outstanding, Type, Category, Status, Email). Zoho Books exports 70+ columns with the data scattered across them. This tool maps Zoho's columns to the Ocealgo template and applies smart heuristics to fill in fields Zoho doesn't have directly.

## How to use

1. Export contacts from Zoho Books → Contacts → Export → Excel (.xlsx).
2. Open the converter in your browser.
3. Drag the exported file onto the dropzone (or click to browse).
4. Review the mapped data. Cells highlighted **red** are required fields that came out empty — fill them in inline before downloading.
5. Edit any field directly in the table. Type/Category/Status are dropdowns; everything else is text.
6. Delete unwanted rows with the `✕` button on the left.
7. Click **Download Mapped Excel** to get the import-ready file.

The downloaded file uses the exact column names from the Ocealgo party-import template, so it can be imported as-is.

## Field mapping

| Zoho field | → | Ocealgo field |
|-----------|---|---------------|
| Display Name (parenthetical stripped) | → | Name |
| Phone → MobilePhone → Billing Phone (`+91-` stripped) | → | Phone |
| EmailID | → | Email |
| Billing Address + Billing Street2 | → | Address |
| Billing Street2 → Billing City | → | Place / Area |
| Billing County → Billing City | → | District |
| Billing State | → | State |
| Billing Code | → | Pincode |
| Opening Balance | → | Outstanding Receivables |
| Parsed from Display Name keywords | → | Type & Category |
| Status (lowercased & normalized) | → | Status |

### Auto-detection rules

**Type** (distributor/retailer)
- Name contains `distributor` → `distributor`
- Name contains `store`, `shop`, `mart`, `retailer`, `pharmacy`, `supermarket` → `retailer`
- Customer Sub Type is `business` → `retailer`
- Otherwise → `retailer` (safe default — review and adjust)

**Category** (FMCG/Pharma/General Store/Supermarket/Online/Other)
- Name contains `pharma`, `pharmacy`, `medical` → `Pharma`
- Name contains `fmcg` → `FMCG`
- Name contains `amazon`, `flipkart`, `online` → `Online`
- Name contains `supermarket`, `hyper` → `Supermarket`
- Name contains `baby`, `store`, `shop`, `mart`, `enterprise`, `trader` → `General Store`
- Otherwise → `Other`

If you find recurring contacts the auto-detection gets wrong, the rules are easy to extend — see the `extractTypeCategory` function in `index.html`.

## Tech

- Single HTML file, no build step, no backend.
- [SheetJS](https://sheetjs.com/) loaded from CDN for `.xlsx` parsing and writing.
- All processing happens in the browser. Nothing is uploaded.
