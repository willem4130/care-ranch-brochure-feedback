# Booking-Notification Email — The Care Ranch

> **Type:** transactional email (internal notification)
> **Framework:** Concierge slip (A) vs. Landed letter (B)
> **Voice:** The Care Ranch (derived from brochure-v20.html + .references/voice.md)
> **Generated:** 2026-04-14

Both variations produce HTML + plain-text bodies plus a subject line. Server-side substitution of `{{NAME}}`, `{{EMAIL}}`, `{{DATE}}` happens inside `functions/api/book.js` before the Resend call.

---

## Variation A — Concierge slip

**Angle.** Professional booking slip. Scannable hierarchy, two-second triage. One opening line of quiet warmth, structured key-value rows, brochure-native closer. Wears well on the 10th booking.

### Subject

```
New booking inquiry: {{NAME}}, {{DATE}}
```

### HTML body

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="color-scheme" content="only light">
  <meta name="supported-color-schemes" content="only light">
  <title>New booking inquiry</title>
  <style>
    @media (prefers-color-scheme: dark) {
      body, table, td { background-color: #F7F4F0 !important; color: #3D3632 !important; }
      .panel { background-color: #F2EDE4 !important; }
      .primary { color: #3D3632 !important; }
      .secondary { color: #79584A !important; }
      .accent { color: #C47D5C !important; }
      .rule { background-color: #E4DCCE !important; }
    }
    @media only screen and (max-width: 620px) {
      .container { width: 100% !important; }
      .panel-inner { padding: 28px 22px !important; }
      .row-label { display: block !important; width: 100% !important; padding: 0 0 4px 0 !important; }
      .row-value { display: block !important; width: 100% !important; padding: 0 0 16px 0 !important; }
      .header-pad { padding: 32px 22px 18px 22px !important; }
      .footer-pad { padding: 22px !important; }
    }
  </style>
</head>
<body style="margin:0; padding:0; background-color:#F7F4F0; color:#3D3632; font-family: Arial, Helvetica, sans-serif; -webkit-font-smoothing:antialiased;">
  <div style="display:none; font-size:1px; line-height:1px; max-height:0; max-width:0; opacity:0; overflow:hidden; mso-hide:all; color:#F7F4F0;">
    {{NAME}} has requested to be considered for {{DATE}}. Details below.
  </div>
  <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0" style="background-color:#F7F4F0;">
    <tr>
      <td align="center" style="padding: 32px 16px;">
        <table role="presentation" class="container" width="600" cellpadding="0" cellspacing="0" border="0" style="width:600px; max-width:600px;">
          <tr>
            <td class="header-pad" style="padding: 8px 8px 16px 8px; font-family: Georgia, 'Times New Roman', serif;">
              <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
                <tr>
                  <td align="left" style="font-family: Georgia, 'Times New Roman', serif; font-style: italic; font-size: 15px; letter-spacing: 0.08em; color:#79584A;" class="secondary">
                    The Care Ranch
                  </td>
                  <td align="right" style="font-family: Arial, Helvetica, sans-serif; font-size: 11px; letter-spacing: 0.12em; text-transform: uppercase; color:#79584A;" class="secondary">
                    Booking slip
                  </td>
                </tr>
              </table>
            </td>
          </tr>
          <tr>
            <td class="panel" style="background-color:#F2EDE4; border-radius: 4px;">
              <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
                <tr>
                  <td class="panel-inner" style="padding: 40px 44px 12px 44px; font-family: Georgia, 'Times New Roman', serif;">
                    <p style="margin: 0 0 18px 0; font-family: Georgia, 'Times New Roman', serif; font-style: italic; font-size: 16px; line-height: 1.55; color:#79584A;" class="secondary">
                      A new inquiry arrived through the brochure. The details are held below for review.
                    </p>
                    <p style="margin: 0; font-family: Arial, Helvetica, sans-serif; font-size: 22px; line-height: 1.35; color:#3D3632;" class="primary">
                      <strong style="font-weight: 700;">{{NAME}}</strong>
                    </p>
                    <p style="margin: 4px 0 0 0; font-family: Arial, Helvetica, sans-serif; font-size: 13px; letter-spacing: 0.06em; text-transform: uppercase; color:#C47D5C;" class="accent">
                      {{DATE}}
                    </p>
                  </td>
                </tr>
                <tr>
                  <td style="padding: 20px 44px 4px 44px;">
                    <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
                      <tr>
                        <td class="rule" height="1" style="background-color:#E4DCCE; font-size:1px; line-height:1px;">&nbsp;</td>
                      </tr>
                    </table>
                  </td>
                </tr>
                <tr>
                  <td class="panel-inner" style="padding: 16px 44px 8px 44px;">
                    <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0" style="font-family: Arial, Helvetica, sans-serif;">
                      <tr>
                        <td class="row-label" width="120" valign="top" style="padding: 10px 16px 10px 0; font-size: 11px; letter-spacing: 0.12em; text-transform: uppercase; color:#79584A; vertical-align: top;">Name</td>
                        <td class="row-value" valign="top" style="padding: 10px 0; font-size: 15px; line-height: 1.5; color:#3D3632; vertical-align: top;">{{NAME}}</td>
                      </tr>
                      <tr>
                        <td class="row-label" width="120" valign="top" style="padding: 10px 16px 10px 0; font-size: 11px; letter-spacing: 0.12em; text-transform: uppercase; color:#79584A; vertical-align: top;">Email</td>
                        <td class="row-value" valign="top" style="padding: 10px 0; font-size: 15px; line-height: 1.5; color:#3D3632; vertical-align: top;">
                          <a href="mailto:{{EMAIL}}" style="color:#3D3632; text-decoration: underline;">{{EMAIL}}</a>
                        </td>
                      </tr>
                      <tr>
                        <td class="row-label" width="120" valign="top" style="padding: 10px 16px 10px 0; font-size: 11px; letter-spacing: 0.12em; text-transform: uppercase; color:#79584A; vertical-align: top;">Requested</td>
                        <td class="row-value" valign="top" style="padding: 10px 0; font-size: 15px; line-height: 1.5; color:#3D3632; vertical-align: top;">{{DATE}}</td>
                      </tr>
                      <tr>
                        <td class="row-label" width="120" valign="top" style="padding: 10px 16px 10px 0; font-size: 11px; letter-spacing: 0.12em; text-transform: uppercase; color:#79584A; vertical-align: top;">Source</td>
                        <td class="row-value" valign="top" style="padding: 10px 0; font-size: 15px; line-height: 1.5; color:#3D3632; vertical-align: top;">Brochure booking form</td>
                      </tr>
                    </table>
                  </td>
                </tr>
                <tr>
                  <td class="panel-inner" style="padding: 22px 44px 44px 44px; font-family: Georgia, 'Times New Roman', serif;">
                    <table role="presentation" width="100%" cellpadding="0" cellspacing="0" border="0">
                      <tr>
                        <td class="rule" height="1" style="background-color:#E4DCCE; font-size:1px; line-height:1px;">&nbsp;</td>
                      </tr>
                    </table>
                    <p style="margin: 22px 0 0 0; font-family: Georgia, 'Times New Roman', serif; font-style: italic; font-size: 15px; line-height: 1.6; color:#79584A;" class="secondary">
                      Where needed, we are happy to connect briefly. A short reply to {{EMAIL}} opens the conversation.
                    </p>
                  </td>
                </tr>
              </table>
            </td>
          </tr>
          <tr>
            <td class="footer-pad" align="center" style="padding: 26px 22px 8px 22px; font-family: Arial, Helvetica, sans-serif;">
              <p style="margin: 0 0 6px 0; font-family: Georgia, 'Times New Roman', serif; font-style: italic; font-size: 14px; letter-spacing: 0.06em; color:#79584A;">
                The Care Ranch
              </p>
              <p style="margin: 0; font-family: Arial, Helvetica, sans-serif; font-size: 11px; line-height: 1.5; letter-spacing: 0.04em; color:#79584A;">
                Sent automatically on submission of the brochure booking form.
              </p>
            </td>
          </tr>
        </table>
      </td>
    </tr>
  </table>
</body>
</html>
```

### Plain text fallback

```
The Care Ranch
Booking slip

A new inquiry arrived through the brochure. The details are held below for review.

{{NAME}}
{{DATE}}

Name:       {{NAME}}
Email:      {{EMAIL}}
Requested:  {{DATE}}
Source:     Brochure booking form

Where needed, we are happy to connect briefly. A short reply to {{EMAIL}} opens the conversation.

The Care Ranch
Sent automatically on submission of the brochure booking form.
```

---

## Variation B — Landed letter

**Angle.** Short letter that happens to carry structured info. Italic Georgia opener in Care Ranch cadence, clean labelled list, a closing line that still names an operational expectation. Warmer than A. Best read on reception, potentially performative by the 10th booking.

### Subject

```
A request has landed: {{NAME}}, {{DATE}}
```

### HTML body

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="color-scheme" content="only light">
<meta name="supported-color-schemes" content="light">
<title>A request has landed</title>
<style>
  @media only screen and (max-width: 620px) {
    .tcr-outer { padding: 16px 12px !important; }
    .tcr-panel { padding: 28px 22px !important; }
    .tcr-opener { font-size: 19px !important; line-height: 1.55 !important; }
    .tcr-label { font-size: 11px !important; }
    .tcr-value { font-size: 16px !important; }
    .tcr-close { font-size: 14px !important; }
  }
  @media (prefers-color-scheme: dark) {
    body, .tcr-outer, .tcr-bg { background-color: #F7F4F0 !important; }
    .tcr-panel { background-color: #F2EDE4 !important; }
    .tcr-opener, .tcr-value, .tcr-close { color: #3D3632 !important; }
    .tcr-label, .tcr-footer, .tcr-footer a { color: #79584A !important; }
    .tcr-rule { background-color: #C47D5C !important; }
  }
  [data-ogsc] .tcr-outer { background-color: #F7F4F0 !important; }
  [data-ogsc] .tcr-panel { background-color: #F2EDE4 !important; }
  [data-ogsc] .tcr-opener,
  [data-ogsc] .tcr-value,
  [data-ogsc] .tcr-close { color: #3D3632 !important; }
  [data-ogsc] .tcr-label,
  [data-ogsc] .tcr-footer,
  [data-ogsc] .tcr-footer a { color: #79584A !important; }
</style>
</head>
<body class="tcr-bg" style="margin:0; padding:0; background-color:#F7F4F0; color:#3D3632; -webkit-font-smoothing:antialiased;">
<div style="display:none; max-height:0; overflow:hidden; mso-hide:all; font-size:1px; line-height:1px; color:#F7F4F0; opacity:0;">
  A new request has landed. {{NAME}} is asking for a place, {{DATE}}.
</div>
<table role="presentation" class="tcr-outer" width="100%" cellspacing="0" cellpadding="0" border="0" style="background-color:#F7F4F0; padding:32px 16px;">
  <tr>
    <td align="center">
      <table role="presentation" width="600" cellspacing="0" cellpadding="0" border="0" style="width:100%; max-width:600px;">
        <tr>
          <td class="tcr-panel" style="background-color:#F2EDE4; padding:44px 44px; border-radius:2px;">
            <table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0">
              <tr>
                <td style="font-family:Georgia,'Times New Roman',serif; font-style:italic; font-size:13px; letter-spacing:0.08em; color:#79584A; text-transform:uppercase;">
                  The Care Ranch
                </td>
              </tr>
            </table>
            <table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0" style="margin-top:28px;">
              <tr>
                <td class="tcr-opener" style="font-family:Georgia,'Times New Roman',serif; font-style:italic; font-size:21px; line-height:1.6; color:#3D3632;">
                  A request has landed. Someone is asking for a place.
                </td>
              </tr>
            </table>
            <table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0" style="margin-top:30px;">
              <tr>
                <td class="tcr-rule" height="1" style="background-color:#C47D5C; line-height:1px; font-size:0;">&nbsp;</td>
              </tr>
            </table>
            <table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0" style="margin-top:28px;">
              <tr>
                <td style="padding-bottom:18px;">
                  <div class="tcr-label" style="font-family:Arial,Helvetica,sans-serif; font-size:12px; letter-spacing:0.1em; text-transform:uppercase; color:#79584A;">Name</div>
                  <div class="tcr-value" style="font-family:Arial,Helvetica,sans-serif; font-size:17px; line-height:1.5; color:#3D3632; margin-top:4px;">{{NAME}}</div>
                </td>
              </tr>
              <tr>
                <td style="padding-bottom:18px;">
                  <div class="tcr-label" style="font-family:Arial,Helvetica,sans-serif; font-size:12px; letter-spacing:0.1em; text-transform:uppercase; color:#79584A;">Email</div>
                  <div class="tcr-value" style="font-family:Arial,Helvetica,sans-serif; font-size:17px; line-height:1.5; color:#3D3632; margin-top:4px;">
                    <a href="mailto:{{EMAIL}}" style="color:#3D3632; text-decoration:none; border-bottom:1px solid #C47D5C;">{{EMAIL}}</a>
                  </div>
                </td>
              </tr>
              <tr>
                <td style="padding-bottom:6px;">
                  <div class="tcr-label" style="font-family:Arial,Helvetica,sans-serif; font-size:12px; letter-spacing:0.1em; text-transform:uppercase; color:#79584A;">Retreat dates</div>
                  <div class="tcr-value" style="font-family:Arial,Helvetica,sans-serif; font-size:17px; line-height:1.5; color:#3D3632; margin-top:4px;">{{DATE}}</div>
                </td>
              </tr>
            </table>
            <table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0" style="margin-top:30px;">
              <tr>
                <td class="tcr-close" style="font-family:Georgia,'Times New Roman',serif; font-style:italic; font-size:15px; line-height:1.6; color:#3D3632;">
                  A reply within a day or two keeps the rhythm intact.
                </td>
              </tr>
            </table>
          </td>
        </tr>
        <tr>
          <td style="padding:22px 10px 8px 10px;">
            <table role="presentation" width="100%" cellspacing="0" cellpadding="0" border="0">
              <tr>
                <td class="tcr-footer" style="font-family:Georgia,'Times New Roman',serif; font-style:italic; font-size:12px; letter-spacing:0.06em; color:#79584A;">
                  The Care Ranch
                </td>
              </tr>
              <tr>
                <td class="tcr-footer" style="font-family:Arial,Helvetica,sans-serif; font-size:11px; line-height:1.6; color:#79584A; padding-top:6px;">
                  Submitted via the brochure form at thecareranch.com.
                </td>
              </tr>
            </table>
          </td>
        </tr>
      </table>
    </td>
  </tr>
</table>
</body>
</html>
```

### Plain text fallback

```
THE CARE RANCH

A request has landed. Someone is asking for a place.

Name:           {{NAME}}
Email:          {{EMAIL}}
Retreat dates:  {{DATE}}

A reply within a day or two keeps the rhythm intact.


The Care Ranch
Submitted via the brochure form at thecareranch.com.
```

---

## Review Notes

**Authenticity (pass, after review fixes).** Variation B originally had an em-dash in the subject and an `—` separator in the plain-text fallback. Both removed in this compiled version. No forbidden vocabulary (no "discover / transform / unlock / elevate / leverage"), no exclamation marks, no contractions in formal lines.

**Voice match (pass).** Both variations ground feeling-led lines in procedural detail, per the brochure's signature move. A reuses the brochure line "Where needed, we are happy to connect briefly." verbatim. B's opener reads in Care Ranch cadence (short fragments that land like breaths). The word "place" for a retreat spot matches the form's own confirmation copy; "seat in the circle" (B draft) was replaced because neither "seat" nor "circle" appears in the brochure.

**Specificity (pass).** Every line carries either operational information (name, email, date, source) or a concrete next step (reply to open conversation). No vague filler.

**Triggers (N/A).** Transactional email; no psychological triggers warranted. Both variations are appropriately operational.

**Cross-client rendering.** Both use table-based layouts with inline styles, preheader-hiding via the seven-property trick, `color-scheme: only light` meta + explicit wrapper backgrounds to guard against dark-mode auto-inversion, and an `@media (prefers-color-scheme: dark)` block as a secondary safety net. Variation B adds `[data-ogsc]` overrides for Outlook.com. Both should render safely in Gmail (web + app), Apple Mail (macOS + iOS), Outlook (desktop + web), and dark-mode variants of each.

**Integration.** `functions/api/book.js` currently sends plain text only. Dropping either variation in requires: adding an `html` field to the Resend API call alongside `text`, and performing `{{NAME}}`, `{{EMAIL}}`, `{{DATE}}` substitution before the call. The plain-text fallback stays as `text`; the HTML body becomes `html`.

---

## Recommendation

**Variation A (Concierge slip).** This email ships every time a form is submitted, which over time means dozens of arrivals in the same inbox. A's scannable hierarchy (masthead label, large name, terracotta date line, key-value rows) wears well on repeat. Its single italic opener and brochure-native closer carry the Care Ranch feel without every booking needing to land as a poem. B is lovely but the opener may start to feel performative by the fifth time it arrives.

Possible hybrid: keep A's structure but borrow B's opener ("A request has landed. Someone is asking for a place.") if you want a slightly warmer entry.
