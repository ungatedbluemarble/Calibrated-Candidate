# Document Style Guide
## Calibrated Candidate: Visual Standards for All Generated Documents

This file is the single source of truth for visual formatting across every document produced by the Calibrated Candidate skill package. Skills 02 and 04 both reference this file. Do not define colors, fonts, or layout standards anywhere else.

---

## Font

| Element | Font | Size | Weight |
|---|---|---|---|
| Body text | Arial | 11pt | Regular |
| Section headers | Arial | 13pt | Bold |
| Document title | Arial | 16pt | Bold |
| Callout box text | Arial | 11pt | Regular |
| Table header text | Arial | 11pt | Bold |
| Footer text | Arial | 9pt | Regular |

Arial is used throughout. It is universally supported across Windows, Mac, and Google Docs without substitution. Do not use Calibri, Times New Roman, or any system font that may render differently across platforms.

---

## Color Palette

| Token | Hex Code | Use |
|---|---|---|
| PRIMARY_BLUE | `#1A3C6E` | Section headers, primary headings, document title |
| ACCENT_GREEN | `#1D9E75` | Positive callouts, strong fit indicators, match highlights |
| AMBER | `#92400E` | Warnings, gaps, watch points, things to avoid |
| LIGHT_BLUE_BG | `#D6E4F0` | Background fill for blue callout boxes |
| LIGHT_GREEN_BG | `#E1F5EE` | Background fill for green callout boxes |
| AMBER_BG | `#FEF3CD` | Background fill for amber/warning callout boxes |
| GRAY_BG | `#F2F4F6` | Alternating table row shading |
| WHITE | `#FFFFFF` | Default page and cell background |
| BLACK | `#000000` | Body text, table borders |

---

## Page Layout

| Setting | Value |
|---|---|
| Page size | US Letter: 8.5 x 11 inches (12240 x 15840 DXA) |
| Top margin | 1 inch (1440 DXA) |
| Bottom margin | 1 inch (1440 DXA) |
| Left margin | 1 inch (1440 DXA) |
| Right margin | 1 inch (1440 DXA) |
| Content width | 9360 DXA (page width minus both margins) |

---

## Headers and Footers

**Header:** Candidate name (left-aligned) and company/role (right-aligned), separated by a bottom border in PRIMARY_BLUE. Font: Arial 9pt.

**Footer:** Document title (left-aligned) and page number (right-aligned). Font: Arial 9pt. Use tab stops for two-column footer layout: do not use tables in footers (tables have minimum height and render incorrectly in headers and footers).

---

## Tables

- All tables use `WidthType.DXA`: never `WidthType.PERCENTAGE` (breaks in Google Docs)
- Table width must equal the sum of all column widths
- Every cell must declare its own width: both `columnWidths` on the table AND `width` on each cell
- Cell padding: `top: 80, bottom: 80, left: 120, right: 120`
- Borders: `BorderStyle.SINGLE`, size 1, color `#CCCCCC`
- Shading: always `ShadingType.CLEAR`: never `ShadingType.SOLID` (causes black backgrounds)
- Header rows: fill with PRIMARY_BLUE (`#1A3C6E`), white text
- Alternating data rows: fill with GRAY_BG (`#F2F4F6`) on odd rows, WHITE on even rows

---

## Callout Boxes

Callout boxes are used for: core narrative statements, closing statements, things to avoid, and key warnings. They are implemented as single-cell tables with colored background fill.

| Callout Type | Background | Border Color | Use |
|---|---|---|---|
| Blue callout | `#D6E4F0` | `#1A3C6E` | Core narrative, key instructions |
| Green callout | `#E1F5EE` | `#1D9E75` | Strong match, positive signals |
| Amber callout | `#FEF3CD` | `#92400E` | Warnings, gaps, things to avoid |

---

## Lists

Never use unicode bullet characters directly in text. Always use the `docx` numbering config with `LevelFormat.BULLET`. Unicode bullets render inconsistently across Word versions and fail ATS parsing.

```javascript
numbering: {
  config: [
    {
      reference: "bullets",
      levels: [{
        level: 0,
        format: LevelFormat.BULLET,
        text: "•",
        alignment: AlignmentType.LEFT,
        style: { paragraph: { indent: { left: 720, hanging: 360 } } }
      }]
    }
  ]
}
```

---

## Spacing

| Element | Before | After |
|---|---|---|
| Section heading (H1) | 240 DXA | 120 DXA |
| Section heading (H2) | 180 DXA | 90 DXA |
| Body paragraph | 0 DXA | 120 DXA |
| Bullet item | 0 DXA | 60 DXA |
| Callout box | 120 DXA | 120 DXA |

---

## Critical Rules (do not override)

- Set page size explicitly on every document: `docx` defaults to A4, not US Letter
- Never use `\n` inside text runs: use separate Paragraph elements
- PageBreak must be inside a Paragraph: standalone PageBreak creates invalid XML
- ImageRun requires a `type` property: always specify `png`, `jpg`, etc.
- Never use tables as visual dividers or horizontal rules: use a bottom border on a Paragraph instead
- Run the validation script after every document generation: `python scripts/office/validate.py [file]`

---

## Validation

After generating any document, validate before delivering to the user:

```bash
python scripts/office/validate.py /path/to/output.docx
```

If validation fails, unpack the XML, identify the error, fix it, and repack before presenting the file.
