---
layout: post
title: "Browser Drak Mode"
date: 2019-01-01
categories: cmder setup
---

## Install dark reader extension

[Dark Reader](https://darkreader.org/)

## Update the userChrome.css and userContent.css

- [Follow these instructions to create the userChrome.css and userContent.css files](https://www.userchrome.org/how-create-userchrome-css.html)

- [source](https://support.mozilla.org/en-US/questions/1269392)

- userChrome.css

```css
@-moz-document url(chrome://browser/content/browser.xhtml) {
  #main-window,
  browser[type="content-primary"],
  browser[type="content"],
  tabbrowser#content,
  #content,
  browser[type="content"] > html {
    background: #323234 !important;
  }
}
```

- userContent.css

```css
@charset "utf-8"; /* CSS Document */

@-moz-document url("about:newtab") {
  body {
    background-color: #011326 !important;
  }
}

@-moz-document url(chrome://browser/content/browser.xhtml) {
  browser[type="content-primary"] {
    background: #011326 !important;
  }
}
```

## Update firefox flags

```
toolkit.legacyUserProfileCustomizations.stylesheets: true
browser.startup.blankWindow: false
```

## Set dark theme in firefox browser

- Use any dark theme
- Set the Option in firefox setting: Override the colors specified by the page with your selections above: Never
