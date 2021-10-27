\*\*\*\*---
layout: post
title: "HTML email"
date: 2021-09-30
categories: html email

---

# Html email format

## DOCTYPE and Namespance

- Most of the email clients use XHTML as the doctype and namespace

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" lang="en"></html>
```

## Character set

- Utf-8 for the character set

```html
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
```

## IE compatible meta tags

```html
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
```

## Dark mode support meta tags

```html
<meta name="color-scheme" content="light dark" /> <meta name="supported-color-schemes" content="light dark" />
```

## Inline styles are preferred in html emails

## Preheader text

- This is the text displayed in the listview of the emails.
- Apply style so that this text is not displayed in actual email.

Example Preheader

```html
<div
  style="font-size: 0px;color: #fafdfe;line-height: 1px;mso-line-height-rule:exactly;display: none;max-width: 0px;max-height: 0px;opacity: 0;overflow: hidden;mso-hide:all;"
/>
```

## Outlook

### Styles

- Margin should start with capital 'M'

### conditional CSS

- Below if condition is to support body background color in outlook versions great than or equal to 2000 or in IE browser
- It also sets the font to sans-serif etc. Since the online imported CSS fonts are not supported

```html
<!--[if (gte mso 9)|(IE)]>
  <style type="text/css">
    body {
      background-color: #dde0e1 !important;
    }
    body,
    table,
    td,
    p,
    a {
      font-family: sans-serif, Arial, Helvetica !important;
    }
  </style>
<![endif]-->
```

### Ghost table

- Outlook needs a ghost table so that we can specify the styles in it.
- The table width is table style instead of CSS style.

Example:

```html
<!--[if (gte mso 9)|(IE)]>
  <table width="600" align="center" style="border-spacing:0;color:"#565656" role="presentation">
    <tr>
      <td>Content goes here</td>
    </tr>
  </table>
<![endif]-->
```

### Gifs

- The gifs are support in the outlook. We need to use [Vector Markup Language(VML)](https://docs.microsoft.com/en-us/windows/win32/vml/web-workshop---specs---standards----introduction-to-vector-markup-language--vml-)

## Responsive layout

- Two display the table side by side, set below styles to table.

| CSS Style             | Description                      |
| --------------------- | -------------------------------- |
| vertical-align: top   | align vertically to top          |
| display: inline-block | displays inline instead of block |

## Centering the html

- Center tag is used to center the content. This tag is no more supported in HTML5 but it works in email clients

## Images

- For images add alt and title attributes. The border to 0.

## Styles

- Reset the body, table, td, p, a, h\* Margin, padding, border-spacing in the style section. This works for most of the clients except outlook.

### CSS Styles

| CSS Style           | Description                               |
| ------------------- | ----------------------------------------- |
| table-layout: fixed | Ensures the table is set by width         |
| border-spacing: 0   | Ensure there is no space taken for border |
| max-width: 600px    | 600px is the standard for emails          |
| width: 100%         | Takes 100% of the container               |
| padding:0           | Set table data (td) padding to 0          |
| width:100%;         | Set these to all sub table                |
| border-spacing:0;   | Set these to all sub table                |
| role=presentation   | Set these to all sub table                |

### Table Styles

| Table Style        | Description                            |
| ------------------ | -------------------------------------- |
| align: center      | algins the table content to center     |
| width: 600         | Only needed for ghost table in outlook |
| role: presentation | For screen reader                      |

## User online resources

[Puts email to test emails](https://putsmail.com/)

[CSS Guide](https://www.campaignmonitor.com/css/)

[Outlook conditional CSS](https://stackoverflow.design/email/base/mso/)
